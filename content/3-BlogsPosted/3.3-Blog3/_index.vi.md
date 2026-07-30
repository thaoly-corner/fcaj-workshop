---
title: "Blog 3: Xây dựng RAG tối ưu"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Hiện Thực Hóa Hệ Thống RAG Khép Kín Với Amazon Bedrock Và Aurora pgvector

**Retrieval-Augmented Generation (RAG)** là phương pháp kết hợp sức mạnh của các Mô hình ngôn ngữ lớn (LLM) với nguồn dữ liệu tri thức nội bộ. Để một hệ thống RAG hoạt động hiệu quả và chính xác, "trái tim" của kiến trúc nằm ở hai thành phần cốt lõi: **Embedding Model** (chuyển đổi văn bản thành vector) và **Vector Database** (lưu trữ và truy xuất vector). 

Dưới đây là hành trình nhóm xây dựng "trái tim" này hoàn toàn dựa trên hệ sinh thái AWS, ưu tiên tối đa hóa hiệu năng và tính bảo mật.

### 1. Amazon Bedrock: Sức mạnh nhúng (Embedding) linh hoạt và tiết kiệm
Thay vì tự triển khai (self-host) các mô hình mã nguồn mở như HuggingFace SentenceTransformer trên EC2 (rất tốn kém chi phí duy trì GPU) hay ép chạy trên Lambda (thường gặp tình trạng xử lý chậm và lỗi cold-start), nhóm đã lựa chọn sử dụng dịch vụ **Amazon Bedrock** với mô hình `amazon.titan-embed-text-v2:0`.

* **Hiệu suất và chi phí tối ưu:** API của Bedrock phản hồi với độ trễ cực thấp, giúp xử lý embedding cho hàng ngàn đoạn chunk văn bản chỉ trong tích tắc. Đặc biệt, cơ chế tính phí theo số lượng token (pay-per-token) biến đây thành giải pháp cực kỳ tiết kiệm cho các dự án linh hoạt.
* **Đảm bảo tính nhất quán tuyệt đối:** Nguyên tắc sống còn của hệ thống RAG là dữ liệu lưu trong Database và câu hỏi của người dùng (Query) phải được ánh xạ vào không gian vector (vector space) bởi **cùng một mô hình**. Bằng việc gọi Bedrock API ở cả luồng xử lý dữ liệu (ETL pipeline) lẫn luồng truy vấn (RAG API), hệ thống luôn duy trì được độ chính xác cao nhất khi so khớp ngữ nghĩa.

### 2. Amazon Aurora Serverless v2 + pgvector: Lưu trữ "Tất cả trong một"
Thay vì sử dụng các cơ sở dữ liệu Vector độc lập của bên thứ ba (như Qdrant, Pinecone hay Milvus), nhóm quyết định tận dụng **Amazon Aurora PostgreSQL** kết hợp với extension **`pgvector`**.

* **Kết hợp Vector và Relational Data:** PostgreSQL cho phép nhóm lưu trữ siêu dữ liệu (metadata như tác giả, ngày đăng, URL) theo cấu trúc quan hệ truyền thống, đặt cạnh cột dữ liệu kiểu `vector` chứa embedding. Thiết kế này giải quyết xuất sắc các bài toán truy vấn lai (Hybrid Search). Ví dụ: *"Tìm các bài báo có nội dung liên quan đến biến động kinh tế (Vector Search) nhưng chỉ lọc các bài xuất bản trong 7 ngày qua (SQL Filter)"*.
* **Tối ưu tốc độ với chỉ mục HNSW:** Để xử lý bài toán tìm kiếm trên hàng trăm ngàn chunk dữ liệu tin tức, nhóm đã cấu hình chỉ mục **HNSW (Hierarchical Navigable Small World)** trên cột vector. Nhờ đó, các truy vấn tính toán độ tương đồng (cosine similarity) trả về kết quả gần như ngay lập tức (real-time).
* **Tự động mở rộng (Auto-scaling):** Tận dụng kiến trúc của Aurora Serverless v2, database có khả năng tự động điều chỉnh tài nguyên (scale up/down từ 0 đến 2 ACU) dựa trên tải thực tế của hệ thống. Điều này đảm bảo trải nghiệm hỏi-đáp luôn mượt mà trong giờ cao điểm mà không lãng phí tài nguyên lúc rảnh rỗi.

### Tầm nhìn kiến trúc và Kết luận
Kiến trúc kết hợp giữa Amazon Bedrock và Aurora pgvector mang lại một pipeline RAG khép kín (End-to-End) hoàn toàn trên AWS. Toàn bộ dữ liệu tin tức nhạy cảm không bao giờ phải rời khỏi mạng nội bộ (VPC) để gọi ra các API bên ngoài, đảm bảo tuyệt đối tính bảo mật và giảm thiểu tối đa độ trễ mạng. 

Đây chính là điểm nhấn kỹ thuật mang tính nền tảng mà nhóm tâm đắc nhất trong toàn bộ quy trình thiết kế kiến trúc.

---
**Tài liệu tham khảo:**
1. [Amazon Titan Text Embeddings models](https://docs.aws.amazon.com/bedrock/latest/userguide/titan-embedding-models.html)
2. [Running pgvector in production on Amazon Aurora PostgreSQL](https://aws.amazon.com/blogs/database/running-pgvector-in-production-on-amazon-aurora-postgresql/)
3. [pgvector: Open-source vector similarity search for Postgres](https://github.com/pgvector/pgvector)