---
title: "Báo cáo Công việc - Tuần 5"
date: 2026-06-29
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu Tuần 5:
* **Khởi đầu thuận lợi:** Kiểm thử API Amazon Bedrock (`amazon.titan-embed-text-v2:0`) trên local sau khi gỡ limit thành công.
* **Hạ tầng Cơ sở dữ liệu:** Tiến hành tạo DB Aurora Serverless và cấu hình trên AWS, kèm theo extension `pgvector`.
* **Migrate luồng Vectorize:** Đưa toàn bộ logic tiền xử lý và chunking đã viết ở local (từ Tuần 3) lên AWS Lambda, kết hợp gọi API Bedrock.
* **Public RAG API:** Triển khai Lambda RAG API kết hợp API Gateway, hoàn thành luồng Truy xuất (HNSW Search) và Sinh văn bản.

### Các công việc thực hiện trong tuần:
| Day | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| :---: | :--- | :---: | :---: | :--- |
| 2 | - Test thử nghiệm gọi API Bedrock qua `boto3` ở local để đảm bảo model hoạt động ổn định. <br> - Tiến hành tạo DB Aurora Serverless và cấu hình trên AWS, kích hoạt extension `pgvector`. | 29/06/2026 | 29/06/2026 | [Boto3 Bedrock Runtime](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/bedrock-runtime.html) |
| 3 | - Migrate logic Vectorize lên AWS Lambda: Đưa thuật toán từ local lên môi trường Serverless. | 30/06/2026 | 30/06/2026 | [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/) |
| 4 | - Tích hợp Bedrock Titan Embed vào Lambda: Xử lý nhúng các chunks thành vector 1024 chiều. <br> - Viết logic xử lý lỗi (exception handling) khi gọi API Bedrock. | 01/07/2026 | 01/07/2026 | [Amazon Bedrock Docs](https://docs.aws.amazon.com/bedrock/) |
| 5 | - Cấu hình thư viện `psycopg2` để Lambda kết nối và insert trực tiếp dữ liệu vào database Aurora vừa tạo. | 02/07/2026 | 02/07/2026 | [psycopg2 Docs](https://www.psycopg.org/docs/) |
| 6 | - Migrate hàm RAG API lên Lambda: Nhận câu hỏi -> Embed -> Truy vấn HNSW lấy Top-K từ pgvector -> Nhồi ngữ cảnh vào prompt gọi LLM. | 03/07/2026 | 03/07/2026 | [pgvector HNSW index tuning](https://github.com/pgvector/pgvector#hnsw) |
| 7 | - Sửa các lỗi lặt vặt (bugs) phát sinh khi ghép nối pipeline. <br> - Viết báo cáo tổng kết tiến độ tuần 5. | 04/07/2026 | 04/07/2026 | N/A |

### Thành tựu Tuần 5:

* Khai thông hoàn toàn bế tắc hạ tầng: Kiểm thử local thành công và khai thác hiệu quả Foundation Models của AWS thông qua API Amazon Bedrock.

* Tiến hành tạo DB Aurora Serverless và cấu hình trên AWS thành công, đồng thời kích hoạt extension `pgvector` để chuẩn bị môi trường lưu trữ vector an toàn.

* Dịch chuyển (Migrate) thành công toàn bộ mã nguồn xử lý dữ liệu từ local lên AWS Lambda: Tối ưu hóa luồng Vectorize để hoạt động trơn tru trên môi trường Serverless.

