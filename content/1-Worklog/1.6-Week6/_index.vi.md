---
title: "Báo cáo Công việc - Tuần 6"
date: 2026-07-06
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu Tuần 6:
* Rà soát, tối ưu hóa và kiểm tra lại toàn bộ mã nguồn Backend/Lambda của luồng RAG.
* Ghép nối hoàn chỉnh và chạy kiểm thử toàn bộ pipeline (End-to-End) để xử lý dứt điểm các lỗi phát sinh.
* Viết tài liệu kỹ thuật nội bộ (Internal Documentation) để bàn giao và ghi nhận kiến trúc hệ thống.

### Các công việc thực hiện trong tuần:
| Day | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| :---: | :--- | :---: | :---: | :--- |
| 2 | - Rà soát lại toàn bộ mã nguồn Backend và các Lambda function (Vectorize, RAG API) để chuẩn hóa code structure. | 06/07/2026 | 06/07/2026 |  |
| 3 | - Kiểm tra lại luồng kết nối cơ sở dữ liệu Aurora và các biến môi trường (Environment Variables) trên AWS. | 07/07/2026 | 07/07/2026 |  |
| 4 | - Ghép nối tổng thể pipeline (Bedrock Embedding → Aurora pgvector HNSW Search → Prompt Construction → LLM Response). | 08/07/2026 | 08/07/2026 |  |
| 5 | - Tiến hành chạy thử nghiệm (End-to-End Testing) và tập trung sửa các lỗi phát sinh (Bug fixing) về logic, exception handling hoặc timeout. | 09/07/2026 | 09/07/2026 |  |
| 6 | - Tinh chỉnh hiệu năng truy vấn vector và tối ưu hóa thời gian phản hồi của Lambda API. | 10/07/2026 | 10/07/2026 |  |
| 7 | - Bắt đầu soạn thảo tài liệu kỹ thuật nội bộ: Mô tả kiến trúc luồng RAG, cấu hình AWS và cách vận hành các Lambda function. | 11/07/2026 | 11/07/2026 |  |

### Thành tựu Tuần 6:

* Đã rà soát và tối ưu hóa toàn bộ mã nguồn Backend, đảm bảo các Lambda function vận hành trơn tru và bảo mật.

* Ghép nối thành công và vượt qua các bài kiểm thử End-to-End của toàn bộ pipeline RAG, xử lý dứt điểm các lỗi logic và kết nối.

* Ho hoàn thành bộ tài liệu kỹ thuật nội bộ chi tiết về luồng xử lý RAG và cấu hình hạ tầng AWS, phục vụ cho việc bàn giao và bảo trì sau này.

---