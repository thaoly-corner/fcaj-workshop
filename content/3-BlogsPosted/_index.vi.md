---
title: "Các bài Blogs & Kinh nghiệm"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

> **Ghi chú:**  
> Theo kế hoạch ban đầu của chương trình thực tập, các bài viết chia sẻ kỹ thuật này dự kiến sẽ được xuất bản công khai trên cộng đồng Facebook. Tuy nhiên, với đặc thù của dự án NewsRAG – liên tục thu thập và xử lý khối lượng lớn dữ liệu từ các trang báo điện tử lớn – nhóm quyết định đóng gói những kiến thức này dưới dạng **Blog Nội Bộ** trong báo cáo. Đây không chỉ là nhật ký dự án, mà còn là sự đúc kết chân thực nhất về những thách thức kỹ thuật, tư duy tối ưu hóa và kinh nghiệm thực chiến mà toàn đội đã tích lũy được trong suốt quá trình xây dựng hệ thống.

---

Dưới đây là 3 bài blog điểm lại những cột mốc kỹ thuật quan trọng nhất trong việc xây dựng và tối ưu hóa hệ thống NewsRAG:

###  [Blog 1 - VƯỢT GIỚI HẠN TIMEOUT CỦA AWS LAMBDA BẰNG ECS FARGATE](3.1-Blog1/)
* **Thách thức:** Trong quá trình vận hành, hệ thống Crawler gặp phải "nút thắt cổ chai" do giới hạn thời gian xử lý tối đa 15 phút của AWS Lambda.
* **Giải pháp & Bài học:** Bài viết kể về quyết định tái cấu trúc linh hoạt của nhóm khi chuyển đổi sang dịch vụ container **Amazon ECS Fargate**. Sự thay đổi này không chỉ giúp crawler vượt qua rào cản thời gian mà còn đảm bảo khả năng thu thập dữ liệu liên tục, ổn định và bền bỉ hơn.

###  [Blog 2 - TỐI ƯU HÓA CHI PHÍ HỆ THỐNG: TỪ APACHE KAFKA SANG AMAZON SQS](3.2-Blog2/)
* **Thách thức:** Việc duy trì server 24/7 cho hệ thống message broker như Apache Kafka tạo ra gánh nặng chi phí hạ tầng không nhỏ, đặc biệt là trong giai đoạn phát triển.
* **Giải pháp & Bài học:** Bài viết phân tích sâu vào bài toán đánh đổi (trade-off) kiến trúc. Bằng việc mạnh dạn loại bỏ Kafka để chuyển sang sử dụng dịch vụ Serverless **Amazon SQS**, nhóm đã cắt giảm chi phí duy trì hàng đợi xuống mức **gần như 0$**, đồng thời vẫn giữ nguyên hiệu suất và khả năng mở rộng của pipeline luồng dữ liệu.

###  [Blog 3 - HIỆN THỰC HÓA HỆ THỐNG RAG KHÉP KÍN VỚI AMAZON BEDROCK VÀ AURORA PGVECTOR](3.3-Blog3/)
* **Thách thức:** Làm thế nào để xây dựng một kiến trúc RAG không chỉ mạnh mẽ mà còn phải đảm bảo tính bảo mật tuyệt đối, không rò rỉ dữ liệu ra các dịch vụ bên ngoài?
* **Giải pháp & Bài học:** Bài viết nhấn mạnh vào tư duy thiết kế hệ thống bảo mật cao. Thay vì sử dụng cơ sở dữ liệu Vector của bên thứ ba, nhóm đã tích hợp thành công **Amazon Bedrock** cùng **Aurora Serverless v2 (hỗ trợ pgvector)**. Kết quả là tạo ra một quy trình RAG vận hành hoàn toàn khép kín bên trong môi trường AWS, đáp ứng các tiêu chuẩn khắt khe về kiến trúc hệ thống doanh nghiệp.