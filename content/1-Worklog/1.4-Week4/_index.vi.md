---
title: "Báo cáo Công việc - Tuần 4"
date: 2026-06-22
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu Tuần 4:
* Bắt đầu dịch chuyển (Migrate) kiến trúc RAG từ local lên môi trường AWS Serverless.
* Hoàn tất khâu xác thực API Amazon Bedrock (`amazon.titan-embed-text-v2:0`) chuẩn bị cho việc ghép nối pipeline.
* Chuẩn bị sẵn sàng các module (Database, cấu hình mạng) để tích hợp ngay khi API Amazon Bedrock được mở khóa.

### Các công việc thực hiện trong tuần:
| Thứ | Công việc | Ngày bắt đầu | Ngày kết thúc | Nguồn tài liệu |
| :---: | :--- | :---: | :---: | :--- |
| 2 | - Khởi tạo Lambda, viết script test gọi Titan Bedrock. <br> - **Lỗi:** Gặp Exception AccessDenied/Model Access. <br> - **Xử lý:** Tìm nguyên nhân (do kẹt giới hạn Foundation Models) -> Mở Support Ticket (Case) kêu cứu AWS Support ngay trong ngày. | 22/06/2026 | 22/06/2026 | [AWS Support Center](https://docs.aws.amazon.com/awssupport/latest/user/getting-started.html) |
| 3 | - Trong lúc chờ phản hồi, lôi code ra thử 7749 cách fix local (đổi Region, check IAM Role, thêm Policy `bedrock:*`, check Billing...) nhưng vẫn bất lực do kẹt từ phía hệ thống AWS. | 23/06/2026 | 23/06/2026 | [AWS IAM Documentation](https://docs.aws.amazon.com/iam/) |
| 4 | - Nhận được phản hồi từ Support Team yêu cầu thêm thông tin. Giao tiếp qua lại, cung cấp Use Case dự án thực tập để chứng minh mục đích an toàn. <br> - Tranh thủ viết trước logic kết nối Aurora qua `psycopg2`. | 24/06/2026 | 24/06/2026 | N/A |
| 5 | - Tiếp tục chờ AWS đội ngũ chuyên trách verify tài khoản. <br> - Viết nháp hàm `retrieve()` dùng thuật toán HNSW để chuẩn bị trước luồng truy xuất vector. | 25/06/2026 | 25/06/2026 | [pgvector HNSW index tuning](https://github.com/pgvector/pgvector#hnsw) |
| 6 | - Ticket vẫn đang trong trạng thái "Pending AWS". <br> - Rà soát lại toàn bộ cấu hình VPC và Security Group cho Lambda. | 26/06/2026 | 26/06/2026 | N/A |
| 7 | - Trạng thái Ticket chưa có tiến triển. <br> - Tìm hiểu trước tài liệu cấu hình Amazon API Gateway (Lambda Proxy Integration) để chuẩn bị cho tuần sau. | 27/06/2026 | 27/06/2026 | [API Gateway Docs](https://docs.aws.amazon.com/apigateway/) |


### Thành tựu Tuần 4:

* Trải nghiệm thực tế một "đặc sản" của việc làm Cloud: Vượt qua quy trình quản lý rủi ro và xác minh tài khoản khắt khe của hệ sinh thái đám mây AWS.

* Rèn luyện sức chịu đựng và kỹ năng làm việc dai dẳng với bộ phận AWS Support xuyên suốt cả tuần: Phản ứng nhanh nhạy, biết cách cung cấp thông tin liên tục và rõ ràng bằng tiếng Anh để bám sát quá trình xử lý ticket.

* Tận dụng tối ưu thời gian bị chặn (blocker) để thiết lập sẵn mã nguồn cho hạ tầng cơ sở dữ liệu (Aurora) và tìm hiểu trước API Gateway, không để lãng phí thời gian trống.
