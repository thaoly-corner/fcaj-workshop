---
title: "Tổng quan Workshop"
date: 2026-07-25
weight: 1
chapter: false
pre: "<b>5.1</b>"
---

# News RAG Pipeline trên AWS

Workshop này hướng dẫn xây dựng một **News Retrieval-Augmented Generation (News RAG) Pipeline** hoàn chỉnh trên AWS theo kiến trúc **serverless**. Toàn bộ pipeline được tự động hóa từ quá trình thu thập tin tức, xử lý dữ liệu, tạo vector embedding, lưu trữ trong Data Warehouse cho đến triển khai hệ thống hỏi đáp sử dụng Large Language Model (LLM).

Sau khi hoàn thành workshop, bạn sẽ có thể triển khai một hệ thống RAG thực tế có khả năng vận hành tự động trên AWS với chi phí tối ưu và dễ dàng mở rộng.

## Kiến trúc tổng thể

{{< event-image src="images/5-Workshop/5.1-Workshop-overview/RAG-Pipeline.png" alt="News RAG Pipeline Architecture" >}}

Pipeline bao gồm các giai đoạn chính:

1. **Crawler** thu thập bài viết từ các trang báo.
2. **Amazon SQS** tiếp nhận và phân phối dữ liệu.
3. **Lambda Consumer** lưu dữ liệu vào Aurora PostgreSQL.
4. **ETL Lambda** làm sạch dữ liệu, chia nhỏ văn bản và tạo embedding bằng Amazon Bedrock.
5. **Aurora PostgreSQL + pgvector** lưu trữ dữ liệu quan hệ và vector.
6. **RAG API** thực hiện tìm kiếm ngữ nghĩa và sinh câu trả lời bằng LLM.
7. **Frontend Dashboard** cung cấp giao diện tìm kiếm, trò chuyện và giám sát pipeline.

---

# Mục tiêu học tập

Sau khi hoàn thành workshop này, bạn sẽ có thể:

- Triển khai hạ tầng AWS bằng **Terraform**.
- Đóng gói và triển khai **Scrapy Crawler** trên ECS Fargate.
- Thiết kế Data Warehouse theo **Star Schema** trên Aurora PostgreSQL.
- Xây dựng pipeline xử lý dữ liệu với **Amazon SQS** và **AWS Lambda**.
- Tạo vector embedding bằng **Amazon Bedrock Titan Embeddings v2**.
- Xây dựng hệ thống RAG sử dụng **pgvector** và LLM.
- Theo dõi, kiểm thử và tối ưu toàn bộ pipeline.

---

# Nội dung Workshop

| Module | Nội dung | Thời lượng |
|---------|-----------|-----------:|
| **5.1** | Tổng quan Workshop | 30 phút |
| **5.2** | Điều kiện tiên quyết | 30 phút |
| **5.3** | Triển khai hạ tầng bằng Terraform | 60 phút |
| **5.4** | Xây dựng Crawler trên ECS Fargate | 45 phút |
| **5.5** | Amazon SQS & Lambda Consumer | 45 phút |
| **5.6** | ETL, Chunking & Embedding | 60 phút |
| **5.7** | Xây dựng RAG API | 45 phút |
| **5.8** | Tích hợp Frontend Dashboard | 60 phút |
| **5.9** | Kiểm thử và Giám sát | 30 phút |
| **5.10** | Dọn dẹp tài nguyên | 15 phút |

---

# Điều kiện tiên quyết

Trước khi bắt đầu, hãy chuẩn bị:

### AWS

- AWS Account
- Quyền sử dụng:
  - VPC
  - ECS
  - ECR
  - Aurora PostgreSQL
  - Lambda
  - Amazon SQS
  - Amazon EventBridge
  - API Gateway
  - Amazon Bedrock
  - CloudWatch
  - IAM

### Công cụ

- AWS CLI
- Terraform >= 1.5
- Docker & Docker Compose
- Python 3.10+
- Git
- Visual Studio Code (khuyến nghị)

---

# Chi phí ước tính

Các dịch vụ dưới đây được tính theo khu vực **ap-southeast-2 (Sydney)**.

| Dịch vụ | Vai trò | Chi phí/tháng |
|---------|----------|--------------:|
| ECS Fargate | Chạy Scrapy Crawler | ~$1.11 |
| Aurora Serverless v2 | Data Warehouse + pgvector | ~$44.66 |
| Amazon Bedrock | Embedding & LLM | ~$10.00 |
| AWS Lambda | ETL & RAG API | ~$3.00 |
| VPC Endpoints | Private networking | ~$28.00 |
| Amazon S3 | Lưu trữ log và Terraform State | ~$0.80 |
| Amazon SQS + EventBridge | Queue & Scheduler | ~$0.00 |
| CloudWatch | Logging & Monitoring | ~$5.80 |
| **Tổng cộng** | | **~$93.37/tháng** |

> **Lưu ý**
>
> Đây là chi phí tham khảo cho môi trường demo. Khi sử dụng AWS Free Tier, tắt tài nguyên sau workshop hoặc giảm quy mô hệ thống, chi phí thực tế có thể thấp hơn đáng kể.

### Giả định workload

| Hạng mục | Giá trị |
|----------|---------|
| Website | 5–10 trang báo |
| Bài viết mới | ~500 bài/ngày |
| Độ dài trung bình | ~1.200 từ |
| Chunk tạo ra | ~3.000 chunks/ngày |
| Embedding | ~3.000 vectors/ngày |
| Truy vấn RAG | ~120 requests/ngày |

---

# Cấu trúc dự án

```text
AWS-Projects/
├── config/
├── crawler/
├── consumer/
├── etl/
├── search/
├── database/
├── terraform/
├── main.py
├── Dockerfile
├── docker-compose.yml
├── deploy.sh
├── requirements.txt
└── README.md
```

---

# Chuyển đổi kiến trúc: v1 → v2

Workshop này được xây dựng dựa trên việc nâng cấp kiến trúc của phiên bản khóa luận nhằm hướng tới một hệ thống **đơn giản hơn, serverless hơn và dễ triển khai hơn trên AWS**.

| Thành phần | v1 | v2 |
|------------|----|----|
| Crawler | Lambda + Scrapy | ECS Fargate + SitemapSpider |
| Message Queue | Kafka | Amazon SQS |
| Embedding | BGE chạy cục bộ | Amazon Bedrock Titan Embed v2 |
| Vector Database | Qdrant | Aurora PostgreSQL + pgvector |
| Vectorization | Fargate riêng | Tích hợp trong ETL Lambda |
| Query Embedding | Local Model | Amazon Bedrock |

### Vì sao cần chuyển sang v2?

- Loại bỏ các dịch vụ phải tự quản lý (Kafka, Qdrant).
- Giảm số lượng thành phần cần vận hành.
- Tận dụng các dịch vụ serverless của AWS.
- Dễ triển khai bằng Terraform.
- Đồng nhất hệ sinh thái AWS.
- Thuận tiện mở rộng và bảo trì.

> **Lưu ý quan trọng**
> ETL và RAG API **phải sử dụng cùng một embedding model** (`amazon.titan-embed-text-v2:0`).
> Nếu hai giai đoạn sử dụng hai embedding model khác nhau, vector sẽ nằm trong hai không gian biểu diễn khác nhau và kết quả tìm kiếm sẽ không còn chính xác.
---

**Tiếp theo:** [Điều kiện tiên quyết](5.2-Prerequisites/)