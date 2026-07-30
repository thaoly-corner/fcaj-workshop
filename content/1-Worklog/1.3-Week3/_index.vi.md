---
title: "Báo cáo Công việc - Tuần 3"
date: 2026-06-15
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu Tuần 3:
* Phối hợp đồng bộ với luồng xử lý dữ liệu (ETL) của nhóm: Hoàn thiện phần Truy xuất và Sinh văn bản (Retrieval - Augmented Generation).
* Xây dựng kiến trúc truy xuất 2 giai đoạn (two-stage retrieval) kết hợp Vector Search và Cross-Encoder Reranking để tối ưu độ chính xác của ngữ cảnh.
* Tích hợp LLM (Google Gemini) để hoàn thiện luồng hỏi đáp dựa trên ngữ cảnh.
* Tìm hiểu hạ tầng đám mây AWS (Lambda, Bedrock) vào cuối tuần.

### Các công việc thực hiện trong tuần:
| Thứ | Công việc | Ngày bắt đầu | Ngày kết thúc | Nguồn tài liệu |
| :---: | :--- | :---: | :---: | :--- |
| 2 | - Đăng ký API Key và viết script test gọi Google Gemini API. | 15/06/2026 | 15/06/2026 | [Gemini API Docs](https://ai.google.dev/docs) |
| 3 | - Dùng `sentence-transformers` load model `BAAI/bge-m3` để nhúng (embed) câu hỏi truy vấn (đồng bộ không gian vector với nhóm). | 16/06/2026 | 16/06/2026 | [HuggingFace - BAAI/bge-m3](https://huggingface.co/BAAI/bge-m3) |
| 4 | - Cài `qdrant-client`, kết nối cluster Qdrant. <br> - Chốt cấu trúc Payload (Metadata) với team ETL. | 17/06/2026 | 17/06/2026 | [Qdrant Python Client](https://qdrant.tech/documentation/interfaces/python/) |
| 5 | - Xây dựng Retrieve Pipeline 2 giai đoạn: <br> 1. Truy vấn Qdrant lấy Top-N (Retrieve). <br> 2. Dùng mô hình Cross-Encoder (vd: `BAAI/bge-reranker-base`) để chấm điểm và xếp hạng lại (Rerank) ra Top-K. | 18/06/2026 | 18/06/2026 | [Advanced RAG - Reranking](https://www.pinecone.io/learn/advanced-rag-techniques/) |
| 6 | - Kỹ thuật thiết kế prompt (Prompt Engineering): Nhồi Top-K ngữ cảnh đã rerank vào Gemini để sinh câu trả lời tiếng Việt chính xác. | 19/06/2026 | 19/06/2026 | [Prompt Engineering Guide](https://www.promptingguide.ai/) |
| 7 | - **Test End-to-End với team:** Đợi luồng ETL đẩy data xong -> Chạy query -> Rerank -> LLM sinh câu trả lời. | 20/06/2026 | 20/06/2026 | N/A |

### Thành tựu Tuần 3:

* Tối ưu hóa chất lượng ngữ cảnh bằng việc áp dụng thành công kiến trúc truy xuất 2 giai đoạn (two-stage retrieval), sử dụng Cross-Encoder để tinh chỉnh và đánh giá lại độ phù hợp của kết quả tìm kiếm thô từ Qdrant.

* Khớp nối thành công quy trình làm việc nhóm, tách biệt rõ ràng luồng đẩy dữ liệu (Data Ingestion) và luồng truy vấn (Data Consumption).

* Đồng bộ hóa không gian vector bằng cách sử dụng chung mô hình `BAAI/bge-m3` với luồng ETL.

* Hoàn thiện công cụ tìm kiếm ngữ nghĩa và tích hợp mượt mà với mô hình sinh ngôn ngữ tự nhiên (Google Gemini), pass toàn bộ kịch bản test End-to-End trên local.
