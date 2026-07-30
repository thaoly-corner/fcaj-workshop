---
title: "Blog 1: Vượt qua giới hạn Timeout với AWS Fargate"
date: 2026-06-25
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

Trong quá trình xây dựng hệ thống **NewsRAG Pipeline**, một trong những thách thức kỹ thuật lớn nhất mà nhóm gặp phải là thu thập dữ liệu từ các trang báo điện tử như **VnExpress**, **Thanh Niên** và **VietnamNet**. Ban đầu, toàn bộ quá trình thu thập dữ liệu được triển khai trên **AWS Lambda** nhằm tận dụng kiến trúc serverless và giảm chi phí vận hành.

## Vấn đề: Giới hạn thời gian thực thi của AWS Lambda

AWS Lambda là một dịch vụ serverless mạnh mẽ, cho phép chạy mã mà không cần quản lý máy chủ. Tuy nhiên, Lambda có một giới hạn quan trọng: **mỗi lần thực thi chỉ được phép chạy tối đa 15 phút (900 giây)**.

Đối với hệ thống NewsRAG, crawler phải quét sitemap của nhiều trang báo, lấy danh sách hàng nghìn bài viết, tải nội dung HTML, phân tích dữ liệu và gửi kết quả vào hàng đợi để xử lý tiếp theo.

Trong quá trình này, crawler còn phải tuân thủ tốc độ truy cập hợp lý nhằm tránh bị các website chặn IP hoặc giới hạn tốc độ (Rate Limiting). Vì vậy, thời gian thực thi thường vượt quá giới hạn của Lambda, khiến tiến trình bị dừng giữa chừng trước khi hoàn thành việc thu thập dữ liệu.

## Giải pháp: Chuyển sang Amazon ECS Fargate

Để giải quyết triệt để vấn đề này nhưng vẫn giữ được ưu điểm của kiến trúc serverless, nhóm đã chuyển crawler sang chạy trên **Amazon ECS Fargate**.

{{< event-image src="images/3-Blogs/fargate.png" alt="AWS Fargate Architecture" >}}

Việc sử dụng ECS Fargate mang lại nhiều lợi ích cho hệ thống:

- **Không giới hạn thời gian thực thi**

  Khác với Lambda, một Fargate Task có thể chạy cho đến khi hoàn thành toàn bộ quá trình thu thập dữ liệu mà không bị giới hạn 15 phút.

- **Đóng gói ứng dụng bằng Docker**

  Toàn bộ crawler được xây dựng bằng **Scrapy** và đóng gói thành Docker Image. Image sau đó được lưu trữ trên **Amazon Elastic Container Registry (Amazon ECR)** để dễ dàng triển khai và quản lý.

- **Tự động chạy theo lịch**

  Nhóm sử dụng **Amazon EventBridge Scheduler** để kích hoạt crawler tự động vào **01:00** và **02:00** mỗi ngày, đảm bảo dữ liệu luôn được cập nhật trong thời gian hệ thống có ít lưu lượng truy cập.

- **Tối ưu tài nguyên**

  Nhờ Fargate cho phép cấu hình tài nguyên linh hoạt, crawler hiện chỉ cần:

  - **0.25 vCPU**
  - **0.5 GB RAM**

  nhưng vẫn đáp ứng tốt khối lượng công việc hằng ngày.

## Kết quả đạt được

Sau khi chuyển sang Amazon ECS Fargate, quá trình thu thập dữ liệu trở nên ổn định hơn đáng kể.

Crawler có thể hoạt động liên tục khoảng **30–40 phút** mỗi đêm để thu thập hàng trăm bài viết mới mà không còn gặp lỗi timeout như trước đây.

Bên cạnh đó, chi phí vận hành vẫn được duy trì ở mức thấp nhờ cơ chế **Pay-as-you-go** của Fargate. Hệ thống chỉ phải trả chi phí cho đúng khoảng thời gian crawler thực sự chạy, thay vì phải duy trì một máy chủ hoạt động liên tục.

> **Bài học rút ra**
>
> Trong quá trình thiết kế kiến trúc trên AWS, không có một dịch vụ nào phù hợp cho mọi trường hợp. AWS Lambda rất hiệu quả với các tác vụ ngắn, trong khi Amazon ECS Fargate lại là lựa chọn phù hợp hơn đối với các tiến trình cần thời gian xử lý dài hoặc khối lượng công việc lớn.
>
> Việc hiểu rõ ưu điểm và giới hạn của từng dịch vụ, sau đó lựa chọn hoặc chuyển đổi sang giải pháp phù hợp, là yếu tố quan trọng để xây dựng một hệ thống có khả năng mở rộng, ổn định và tối ưu chi phí.

---

## Tài liệu tham khảo

1. AWS Lambda Developer Guide – Limits
   https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html

2. Amazon ECS Fargate Documentation
   https://aws.amazon.com/fargate/

3. Scrapy Framework
   https://scrapy.org/

4. AWS Decision Guide – AWS Lambda or AWS Fargate
   https://docs.aws.amazon.com/decision-guides/latest/fargate-or-lambda/fargate-or-lambda.html

5. Task Networking in AWS Fargate
   https://aws.amazon.com/blogs/compute/task-networking-in-aws-fargate/