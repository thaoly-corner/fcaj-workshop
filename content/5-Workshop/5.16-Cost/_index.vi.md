---
title: "Tối ưu chi phí"
date: 2026-07-28
weight: 16
chapter: false
pre: " <b> 5.16 </b> "
---
Phần này trình bày các chiến lược tối ưu chi phí cho hệ thống **News RAG Pipeline** trên AWS, đồng thời vẫn đảm bảo hiệu năng, khả năng mở rộng và tính ổn định của hệ thống.

## Chi phí ước tính (Theo tháng, khu vực ap-southeast-2)

| Dịch vụ | Cấu hình | Chi phí ước tính | Tỷ lệ |
|---------|----------|-----------------:|------:|
| **Aurora Serverless v2** | PostgreSQL 16, pgvector, 0.5–2 ACUs | ~$44.66 | ~45% |
| **VPC Endpoints** | Bedrock, ECR, CloudWatch Logs, S3 | ~$28.00 | ~28% |
| **Amazon Bedrock** | Titan Embed Text v2 và mô hình sinh văn bản | ~$10.00 | ~10% |
| **Amazon RDS Proxy** | Pool kết nối Aurora PostgreSQL | ~$5.00 | ~5% |
| **AWS Lambda** | Consumer, ETL và RAG API | ~$3.00 | ~3% |
| **CloudWatch Logs** | Lưu log trong 7 ngày | ~$5.80 | ~6% |
| **Amazon ECS Fargate** | Chạy Crawler theo lịch | ~$1.11 | ~1% |
| **Amazon S3** | Terraform State và lưu trữ log | ~$0.80 | ~1% |
| **Amazon API Gateway** | HTTP API (~120 yêu cầu/ngày) | ~$0.10 | ~0% |
| **Amazon ECR** | Lưu Docker Image | ~$0.20 | ~0% |
| **Amazon SQS** | Standard Queue | ~$0.00 | ~0% |
| **Amazon EventBridge** | Scheduler | ~$0.00 | ~0% |
| **Tổng cộng** | | **~98.67 USD/tháng** | **100%** |

> **Giả định tải hệ thống**
>
> - 5–10 trang báo điện tử
> - Khoảng 500 bài viết mới mỗi ngày
> - Khoảng 3.000 chunks mỗi ngày
> - Khoảng 3.000 embeddings mỗi ngày
> - Khoảng 120 truy vấn RAG mỗi ngày

---

## Các chiến lược tối ưu chi phí

### 1. Aurora Serverless v2

Aurora là dịch vụ chiếm phần lớn chi phí của hệ thống. Để giảm chi phí, nên cấu hình mức ACU thấp nhất phù hợp với nhu cầu.

```hcl
serverlessv2_scaling_configuration {
  min_capacity = 0.5
  max_capacity = 2
}
```

**Khuyến nghị**

- Development: `0.5–1 ACU`
- Production: `1–2 ACUs`
- Chỉ tăng `max_capacity` khi thực sự cần xử lý nhiều truy vấn đồng thời.
- Theo dõi chỉ số **ServerlessDatabaseCapacity** trên CloudWatch để điều chỉnh phù hợp.

---

### 2. Cấu hình bộ nhớ Lambda hợp lý

Mỗi Lambda nên được cấp phát bộ nhớ phù hợp với khối lượng xử lý nhằm cân bằng giữa tốc độ thực thi và chi phí.

| Lambda | Bộ nhớ khuyến nghị |
|---------|-------------------|
| Consumer | 512 MB |
| ETL | 1024 MB |
| RAG API | 1024 MB |

Việc tăng bộ nhớ giúp Lambda có nhiều CPU hơn, từ đó giảm thời gian thực thi mà không làm tăng đáng kể chi phí.

---

### 3. Chạy Crawler theo lịch bằng ECS Fargate

Crawler chỉ được kích hoạt theo lịch thông qua Amazon EventBridge thay vì chạy liên tục.

Cấu hình đề xuất:

- CPU: **0.25 vCPU**
- Memory: **512 MB**
- Chạy **1 lần/ngày**

Nhờ đó chỉ phát sinh chi phí trong thời gian crawler hoạt động.

---

### 4. Chỉ tạo Embedding cho dữ liệu mới

Trong bước ETL, chỉ những bài viết chưa tồn tại embedding mới được gửi đến Amazon Bedrock.

Quy trình:

- Kiểm tra URL hoặc SHA256.
- Bỏ qua các bài viết đã xử lý.
- Chỉ embedding các chunk mới.

Cách này giúp giảm đáng kể số lần gọi Bedrock API.

---

### 5. Thiết lập thời gian lưu CloudWatch Logs

Không nên lưu log quá lâu đối với môi trường Development.

```hcl
resource "aws_cloudwatch_log_group" "main" {
  retention_in_days = 7
}
```

Khuyến nghị:

- Development: **3–7 ngày**
- Production: **14–30 ngày**

---

### 6. Chỉ tạo các VPC Endpoint cần thiết

VPC Endpoint giúp tăng tính bảo mật nhưng cũng là một trong những thành phần tốn chi phí nhất.

Chỉ nên giữ các Endpoint thực sự sử dụng:

- Bedrock Runtime
- Amazon S3
- Amazon ECR
- CloudWatch Logs

Nếu không sử dụng dịch vụ nào, nên xóa Endpoint tương ứng để giảm chi phí.

---

### 7. Sử dụng Amazon RDS Proxy

Amazon RDS Proxy giúp quản lý pool kết nối giữa Lambda và Aurora PostgreSQL.

Lợi ích:

- Giảm số lượng kết nối trực tiếp đến Aurora.
- Tăng khả năng xử lý đồng thời.
- Giảm lỗi timeout khi nhiều Lambda chạy cùng lúc.
- Cải thiện hiệu năng của hệ thống.

---

## Theo dõi chi phí

### AWS Budgets

Nên tạo ngân sách để nhận cảnh báo khi chi phí vượt quá ngưỡng mong muốn.

Ví dụ:

- Ngân sách: **100 USD/tháng**
- Cảnh báo khi đạt:
  - 50%
  - 80%
  - 100%

---

### AWS Cost Explorer

Có thể sử dụng AWS Cost Explorer để theo dõi:

- Chi phí theo từng dịch vụ
- Chi phí theo ngày hoặc tháng
- Dự báo chi phí trong tương lai
- Các dịch vụ phát sinh chi phí bất thường

---

## Danh sách kiểm tra tối ưu chi phí

| ✓ | Hạng mục | Trạng thái | Mức ảnh hưởng |
|---|----------|------------|---------------|
| 1 | Aurora Serverless v2 tối thiểu 0.5 ACU | | Cao |
| 2 | Chỉ tạo embedding cho dữ liệu mới | | Cao |
| 3 | Chạy ECS Fargate theo lịch | | Trung bình |
| 4 | Cấu hình bộ nhớ Lambda phù hợp | | Trung bình |
| 5 | Giới hạn thời gian lưu CloudWatch Logs | | Thấp |
| 6 | Chỉ giữ các VPC Endpoint cần thiết | | Cao |
| 7 | Thiết lập AWS Budgets | | Trung bình |
| 8 | Sử dụng Amazon RDS Proxy | | Cao |

---

## Chi phí theo môi trường

| Môi trường | Aurora | RDS Proxy | Bedrock | Compute | Tổng chi phí |
|------------|---------|-----------|----------|----------|-------------:|
| **Development** | ACU thấp | Tùy chọn | Ít sử dụng | Thấp | **~45–60 USD/tháng** |
| **Staging** | ACU trung bình | Có | Trung bình | Trung bình | **~70–85 USD/tháng** |
| **Production** | 0.5–2 ACUs | Có | Theo workload | Đầy đủ | **~95–100 USD/tháng** |

> Nên sử dụng các AWS Account hoặc Resource Tags riêng cho Development, Staging và Production để dễ dàng theo dõi và phân bổ chi phí.

---

**Tiếp theo:** [Dọn dẹp tài nguyên](5.17-Cleanup/)
