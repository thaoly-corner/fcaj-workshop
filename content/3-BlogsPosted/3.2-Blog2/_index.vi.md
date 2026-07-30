---
title: "Blog 2: Tối ưu chi phí bằng SQS"
date: 2026-07-04
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

Một tiêu chí quan trọng khi thiết kế hệ thống trên nền tảng Cloud là **Tối ưu hóa chi phí (Cost Optimization)**. Trong phiên bản đầu tiên (v1) phát triển ở môi trường cục bộ (local), dự án NewsRAG sử dụng **Apache Kafka** làm hệ thống hàng đợi (message broker) để luân chuyển dữ liệu từ Crawler sang Database. Tuy nhiên, khi đưa hệ thống lên AWS, bài toán chi phí nhanh chóng trở thành một rào cản lớn.

### Bài toán chi phí với Kafka
Apache Kafka là một công cụ xử lý luồng (stream processing) cực kỳ mạnh mẽ. Nhưng để triển khai Kafka trên AWS, nhóm phải đối mặt với hai lựa chọn tốn kém:
1. **Amazon MSK (Managed Streaming for Apache Kafka):** Dịch vụ quản lý hoàn toàn nhưng chi phí rất đắt đỏ, có thể lên tới hàng chục hoặc hàng trăm USD mỗi tháng.
2. **Tự host Kafka trên EC2:** Phải duy trì máy chủ EC2 chạy 24/7, tốn kém chi phí cố định (ít nhất ~$10-15/tháng) cộng với công sức quản trị hệ thống, cập nhật bảo mật và duy trì cụm ZooKeeper.

Trong khi đó, luồng dữ liệu của NewsRAG có đặc thù là **Batch processing (Xử lý theo lô)**: Crawler chỉ chạy 1-2 lần mỗi ngày vào ban đêm và đẩy khoảng 500 - 700 tin tức mới. Việc duy trì một cụm Kafka chạy liên tục 24/7 chỉ để phục vụ một luồng dữ liệu đứt quãng (bursty traffic) như vậy là vô cùng lãng phí đối với ngân sách dự án.

### Lựa chọn thay thế: Amazon SQS (Simple Queue Service)
Sau khi phân tích bài toán đánh đổi (Trade-off), nhóm đã quyết định thay thế toàn bộ kiến trúc hàng đợi sang **Amazon SQS**. Đây được xem là lựa chọn hoàn hảo cho dự án với 3 lý do cốt lõi:

* **Fully Serverless & Pay-as-you-go:** SQS không yêu cầu quản lý máy chủ và chỉ tính phí dựa trên số lượng request. Với lượng tin tức hàng ngày của hệ thống, số lượng API request hoàn toàn nằm gọn trong gói **Free Tier** (1 triệu request đầu tiên mỗi tháng miễn phí). Kết quả là, chi phí cho thành phần hàng đợi được giảm xuống **còn 0$**.
* **Tích hợp tự nhiên với AWS Lambda:** SQS đóng vai trò là một "Event Source" hoàn hảo để tự động kích hoạt Lambda function (Lambda Consumer). Khi Crawler đẩy bài báo vào SQS, SQS sẽ tự động gọi Lambda để làm sạch (parsing) và lưu vào Database mà nhóm không cần phải tự viết đoạn code vòng lặp lắng nghe (polling) phức tạp.
* **Cơ chế Dead-Letter Queue (DLQ):** SQS cung cấp sẵn DLQ. Nếu một bài báo bị lỗi định dạng và Lambda không thể lưu vào cơ sở dữ liệu sau nhiều lần thử lại (retries), message đó sẽ tự động được đẩy sang DLQ. Điều này giúp chúng tôi dễ dàng theo dõi và phân tích lỗi sau đó, đảm bảo không một mảnh dữ liệu nào bị thất thoát.

### Kết quả đạt được
Bằng việc mạnh dạn loại bỏ Kafka và chuyển sang kiến trúc SQS kết hợp Lambda, hệ thống không chỉ trở nên nhẹ nhàng, dễ bảo trì hơn mà còn tiết kiệm được một khoản ngân sách vận hành rất lớn. 

> **Đúc kết kinh nghiệm:**  
> Trong quá trình xây dựng hệ thống, lựa chọn công nghệ không phải là chọn thứ "xịn nhất" hay "phức tạp nhất", mà là chọn công cụ **"phù hợp nhất"** với quy mô, bài toán kinh doanh và ngân sách hiện tại.

---
**Tài liệu tham khảo:**
1. [Amazon SQS pricing & Free Tier - AWS Documentation](https://aws.amazon.com/sqs/pricing/)
2. [Using AWS Lambda with Amazon SQS](https://docs.aws.amazon.com/lambda/latest/dg/with-sqs.html)
3. [Amazon SQS Dead-Letter Queues (DLQ)](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html)