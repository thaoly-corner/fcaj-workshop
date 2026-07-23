---
title: "Bản đề xuất"
date: 2026-06-15
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
Phần này tóm tắt các công việc và định hướng kỹ thuật **dự kiến thực hiện** trong khuôn khổ workshop.

# News RAG Pipeline trên nền tảng AWS
## Hệ thống xử lý dữ liệu tin tức Serverless tích hợp RAG hỗ trợ tra cứu thông minh

### 1. Tóm tắt tổng quan
**News RAG Pipeline** là giải pháp xây dựng ứng dụng tra cứu và hỏi đáp tin tức tự động. Hệ thống chủ động thu thập bài viết từ các trang báo điện tử lớn tại Việt Nam (VnExpress, Thanh Niên, VietnamNet), chuẩn hóa dữ liệu về mô hình Kho dữ liệu (Star Schema), tạo vector embedding thông qua Amazon Bedrock và hỗ trợ người dùng truy vấn bằng ngôn ngữ tự nhiên nhờ mô hình RAG (Retrieval-Augmented Generation). Toàn bộ hệ thống vận hành trên hạ tầng Serverless của AWS kết hợp cùng các API LLM tiên tiến (Groq, Gemini), thể hiện khả năng ứng dụng thực tế của kiến trúc đám mây trong việc phát triển các nền tảng tra cứu AI.

### 2. Đặt vấn đề & Giải pháp
#### Thách thức thực tế
Việc cập nhật thông tin từ nhiều nguồn tin tức khác nhau hiện tốn khá nhiều thời gian do phải tìm kiếm và đọc thủ công. Thị trường đang thiếu một nền tảng tập trung cho phép hỏi đáp trực tiếp về sự kiện mới và cung cấp nguồn trích dẫn xác thực. Trong khi đó, các công cụ AI phổ biến như ChatGPT lại chưa cập nhật kịp thời các tin tức thời gian thực.

#### Giải pháp đề xuất
News RAG Pipeline tự động hóa toàn bộ quy trình xử lý thông qua 4 bước: (1) Thu thập bài viết từ sitemap báo chí bằng Scrapy chạy trên ECS Fargate, (2) Chuyển dữ liệu qua SQS để lưu tạm vào Aurora PostgreSQL, (3) Sử dụng Lambda ETL để làm sạch mã HTML, cấu trúc hóa dữ liệu theo Star Schema và tạo vector embedding với Amazon Bedrock Titan Embed v2, (4) Khởi chạy Lambda RAG API để tiếp nhận câu hỏi tự nhiên, thực hiện tìm kiếm tương đồng vector trên pgvector (chỉ mục HNSW) và tổng hợp câu trả lời bằng LLM. Toàn bộ luồng công việc được điều phối tự động bởi EventBridge Scheduler.

#### Giá trị mang lại & Tối ưu chi phí
Dự án cung cấp môi trường thực hành chuyên sâu về MLOps, RAG và kiến trúc Serverless trên AWS. Những lợi ích cốt lõi bao gồm: tổng hợp tin tức tự động loại bỏ thao tác tìm kiếm thủ công, hỗ trợ hỏi đáp AI kèm trích dẫn chính xác giúp tiết kiệm thời gian nghiên cứu, tối ưu hóa ngân sách vận hành nhờ mô hình serverless (chi phí ước tính chỉ khoảng $21–$26 USD/tháng).

### 3. Kiến trúc & Thiết kế hệ thống
Hệ thống vận hành dựa trên kiến trúc Serverless AWS với hai tuyến xử lý chính: (1) **Data Pipeline** — EventBridge Scheduler kích hoạt crawler trên ECS Fargate định kỳ lúc 01:00 UTC để cào dữ liệu và đẩy vào SQS; Lambda Consumer tiếp nhận message, thực hiện lọc trùng lặp bằng mã hash SHA256 trước khi lưu dữ liệu thô vào Aurora PostgreSQL. (2) **ETL + RAG Pipeline** — EventBridge kích hoạt Lambda ETL lúc 02:00 UTC để làm sạch HTML, phân đoạn văn bản (chunk size 500 tokens), tạo embedding 1024 chiều qua Bedrock Titan Embed v2 và lưu vào Aurora pgvector (chỉ mục HNSW). Khi người dùng gửi câu hỏi qua API Gateway, Lambda RAG API sẽ vector hóa truy vấn, tìm kiếm văn bản tương đồng trên pgvector và điều hướng tới LLM (Groq/Gemini) để sinh phản hồi hoàn chỉnh kèm nguồn tham chiếu.

### Danh mục dịch vụ AWS sử dụng
- **Amazon ECS Fargate**: Môi trường thực thi bộ cào dữ liệu Scrapy SitemapSpider (cấu hình 0.25 vCPU, 0.5 GB RAM).
- **Amazon SQS Standard**: Hàng chờ tin nhắn trung gian thay thế Kafka (chi phí xấp xỉ $0/tháng).
- **AWS Lambda** (3 hàm thực thi): Consumer (chuyển SQS sang Aurora), ETL + Bedrock Embed, RAG API.
- **Amazon Aurora Serverless v2**: Cơ sở dữ liệu PostgreSQL 15.4+ tích hợp sẵn extension pgvector.
- **Amazon Bedrock**: Tạo vector embedding chất lượng cao với Titan Embed Text v2 (1024 dimensions).
- **Amazon API Gateway**: Cung cấp giao diện REST API cho phía Frontend.
- **Amazon EventBridge Scheduler**: Tự động hóa lịch trình chạy định kỳ (vào 01:00 và 02:00 UTC).
- **Amazon ECR**: Lưu trữ các Docker Image phục vụ cho tác vụ Fargate.
- **AWS IAM**: Quản lý phân quyền tối thiểu (Least-Privilege) cho Fargate Tasks và Lambda Roles.
- **Amazon CloudWatch**: Ghi nhận log tập trung và theo dõi hệ thống (lưu vết 7 ngày).

### Chi tiết các thành phần
- **Crawler (Fargate)**: Dùng Scrapy SitemapSpider bóc tách file sitemap_news.xml từ 3 trang báo, trích xuất nội dung và gửi tới SQS. Thời gian chạy ~30 phút/ngày.
- **Hàng chờ (SQS)**: Standard queue tích hợp DLQ (Dead Letter Queue), thời gian lưu vết 14 ngày, cơ chế thử lại tối đa 3 lần.
- **Xử lý tiếp nhận (Lambda Consumer)**: Kích hoạt bởi SQS, mã hóa URL bằng SHA256 để khử trùng bài viết trước khi nạp vào Aurora.
- **Biến đổi & Vector hóa (Lambda ETL)**: Loại bỏ các thẻ HTML thừa, cắt nhỏ văn bản (500 tokens/chunk, độ đè 50 tokens), gọi Bedrock Titan Embed v2 và lưu vector 1024d vào pgvector kèm chỉ mục HNSW.
- **Bộ xử lý RAG (Lambda RAG API)**: Vector hóa câu hỏi truy vấn, tìm kiếm độ tương đồng Cosine trên pgvector, sau đó tổng hợp câu trả lời qua Groq/Gemini có kèm liên kết nguồn.
- **Giao diện người dùng (Next.js + FastAPI)**: Dashboard theo dõi KPI, biểu đồ trực quan (Recharts), khung AI Chat hỗ trợ chọn mô hình LLM, Trình quản lý bài viết và Màn hình giám sát đường ống dữ liệu.

### 4. Triển khai kỹ thuật
#### Các giai đoạn thực hiện
Dự án được chi tiết hóa qua 4 giai đoạn chính:
- **Giai đoạn 1 – Hạ tầng & Đóng gói (Tuần 1-2)**: Viết kịch bản Terraform để khởi tạo VPC, Aurora pgvector, ECS Cluster, ECR, Lambda, EventBridge, IAM và CloudWatch. Đóng gói Docker multi-stage cho Fargate Task.
- **Giai đoạn 2 – Phát triển cục bộ (Tuần 3-6)**: Xây dựng môi trường Docker Compose gồm PostgreSQL, Qdrant và Kafka. Phát triển Scrapy SitemapSpider, Kafka Consumer, pipeline ETL chuẩn Star Schema và mô hình tạo vector SentenceTransformer.
- **Giai đoạn 3 – Triển khai AWS Production (Tuần 6-7)**: Đưa Fargate crawler cùng lịch chạy EventBridge lên Cloud, thiết lập Lambda Consumer với SQS trigger, tích hợp Lambda ETL với Bedrock Titan Embed v2, xây dựng Lambda RAG API qua API Gateway và hoàn thiện giao diện Next.js.
- **Giai đoạn 4 – Đánh giá & Tối ưu (Tuần 7-8)**: Thiết lập dashboard/alert trên CloudWatch, kiểm thử tải với Locust và tối ưu ngân sách.

#### Yêu cầu kỹ thuật cốt lõi
- **Data Pipeline**: Scrapy (SitemapSpider), Kafka (Local) / SQS (Cloud AWS), PostgreSQL hỗ trợ pgvector.
- **ETL Pipeline**: Làm sạch nội dung HTML bằng Regex, phân đoạn văn bản 500 tokens, embedding qua SentenceTransformer (Local) / Bedrock Titan v2 (AWS).
- **Hệ thống RAG**: Tìm kiếm tương đồng pgvector HNSW (đo khoảng cách Cosine), tích hợp Groq API (Qwen3-8B, Llama 3.1), Gemini 2.0 Flash dự phòng, thiết kế Prompt có cấu trúc chuẩn trích dẫn nguồn.
- **Quản lý hạ tầng**: Infrastructure as Code với Terraform, Docker multi-stage build, gói triển khai Lambda, lịch trình tự động EventBridge Cron.

### 5. Lộ trình & Mốc phát triển
**Tiến độ thực hiện dự án**
- **Giai đoạn chuẩn bị (Tuần 0)**: Nghiên cứu tài liệu, học kiến thức AWS cơ bản và lập kế hoạch chi tiết.
- **Giai đoạn thực thi (Tuần 1-3)**:
  - *Tuần 1*: Khởi tạo hạ tầng, thiết lập môi trường dev local và hoàn thiện bộ cào tin.
  - *Tuần 2*: Xây dựng đường ống ETL, thiết kế mô hình Star Schema, vector hóa dữ liệu và phát triển RAG API.
  - *Tuần 3*: Triển khai toàn bộ lên AWS Production, đánh giá chất lượng, thiết lập giám sát và tối ưu chi phí.
- **Giai đoạn sau ra mắt**: Bảo trì hệ thống và mở rộng tính năng (semantic chunking, hybrid search, cảnh báo theo chủ đề).

### 6. Dự toán ngân sách
Bảng ước tính chi phí hạ tầng hàng tháng (Tham khảo từ [AWS Pricing Calculator](https://calculator.aws/#/)):

| Dịch vụ | Chi phí dự kiến (/tháng) |
| :--- | :--- |
| Aurora Serverless v2 (2 ACU) | ~$15 - $20 |
| ECS Fargate Crawler (0.25 vCPU, 0.5 GB, 30 phút/ngày) | ~$0.50 |
| AWS Lambda (3 functions) | ~$2 - $3 |
| SQS Standard | ~$0.00 |
| API Gateway | ~$0.30 |
| Bedrock Titan Embed | ~$0.50 |
| CloudWatch Logs (Retention 7 ngày) | ~$1 - $2 |
| **Tổng cộng** | **~$21 - $26 / tháng** |

### 7. Quản trị rủi ro
#### Ma trận rủi ro & Tác động
- **Crawler bị chặn truy cập**: Tác động Trung bình, Xác suất Trung bình (Giảm thiểu: Thêm headers phù hợp, tuân thủ robots.txt, cài đặt `DOWNLOAD_DELAY`).
- **Bedrock bị quá tải (Throttling)**: Tác động Trung bình, Xác suất Thấp (Giảm thiểu: Thiết lập cơ chế thử lại với thuật toán Exponential Backoff).
- **Sự cố API LLM (Groq/Gemini)**: Tác động Cao, Xác suất Thấp (Giảm thiểu: Tích hợp cơ chế fallback đa mô hình).
- **Phát sinh chi phí ngoài dự kiến**: Tác động Trung bình, Xác suất Thấp (Giảm thiểu: Thiết lập cảnh báo ngân sách, tối ưu hóa quy mô tài nguyên).

#### Biện pháp giảm thiểu rủi ro
- **Crawler**: Tuân thủ robots.txt, duy trì `DOWNLOAD_DELAY` tối thiểu 1s, bật tính năng AutoThrottle.
- **Xử lý Throttling**: Áp dụng Exponential Backoff khi gọi Bedrock API, cấu hình `max_attempts=3`.
- **Dự phòng LLM**: Thiết lập chuỗi fallback ưu tiên: Groq Qwen3 $\rightarrow$ Llama 3.1 $\rightarrow$ Gemini 2.0 Flash.
- **Kiểm soát ngân sách**: Cài đặt AWS Budget alert ở mốc 80%, dùng Fargate Spot cho crawler, tinh chỉnh bộ nhớ RAM cho các hàm Lambda.

#### Kế hoạch ứng phó sự cố
- Nếu trang báo chặn crawler: Chuyển hướng sang các nguồn tin thay thế hoặc hỗ trợ nạp dữ liệu thủ công.
- Nếu Bedrock ngưng hoạt động: Chuyển sang mô hình SentenceTransformer chạy cục bộ.
- Nếu chi phí tăng cao: Thu hẹp quy mô Aurora Serverless xuống mốc 1 ACU.

### 8. Kỳ vọng đầu ra
#### Tối ưu kỹ thuật
Tự động hóa hoàn toàn quy trình tổng hợp tin tức, giảm thiểu thao tác đọc thủ công. Khả năng trả lời câu hỏi thông minh đi kèm trích dẫn nguồn rõ ràng. Hạ tầng Serverless linh hoạt, dễ dàng mở rộng khi lưu lượng bài viết tăng lên.

#### Giá trị lâu dài
- Tạo nền tảng RAG chuẩn mực phục vụ cho các bài toán NLP/AI nâng cao trong tương lai.
- Các mô-đun trong đường ống dữ liệu có khả năng tái sử dụng cho các mảng nội dung khác (tin công nghệ, bài báo nghiên cứu...).
- Tích lũy kinh nghiệm thực chiến chuyên sâu với hệ sinh thái Serverless trên AWS.
- Xây dựng kho dữ liệu báo chí chất lượng (~5.000 bài viết) làm cơ sở cho các bài toán phân tích sau này.