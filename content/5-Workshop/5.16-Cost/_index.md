---
title: "Cost Optimization"
date: 2026-07-28
weight: 16
chapter: false
pre: " <b> 5.16 </b> "
---

# Cost Optimization

This section presents the recommended strategies for optimizing the operational cost of the **News RAG Pipeline** on AWS while maintaining system performance, scalability, and reliability.

## Estimated Monthly Cost (ap-southeast-2)

| Service | Configuration | Estimated Cost | Percentage |
|---------|---------------|---------------:|-----------:|
| **Aurora Serverless v2** | PostgreSQL 16, pgvector, 0.5–2 ACUs | ~$44.66 | ~45% |
| **VPC Endpoints** | Bedrock, ECR, CloudWatch Logs, Amazon S3 | ~$28.00 | ~28% |
| **Amazon Bedrock** | Titan Embed Text v2 and foundation models | ~$10.00 | ~10% |
| **Amazon RDS Proxy** | Connection pooling for Aurora PostgreSQL | ~$5.00 | ~5% |
| **AWS Lambda** | Consumer, ETL, and RAG API | ~$3.00 | ~3% |
| **Amazon CloudWatch Logs** | 7-day log retention | ~$5.80 | ~6% |
| **Amazon ECS Fargate** | Scheduled crawler task | ~$1.11 | ~1% |
| **Amazon S3** | Terraform state and log storage | ~$0.80 | ~1% |
| **Amazon API Gateway** | HTTP API (~120 requests/day) | ~$0.10 | ~0% |
| **Amazon ECR** | Docker image storage | ~$0.20 | ~0% |
| **Amazon SQS** | Standard Queue | ~$0.00 | ~0% |
| **Amazon EventBridge** | Scheduled workflow | ~$0.00 | ~0% |
| **Total** | | **~$98.67/month** | **100%** |

> **Workload Assumptions**
>
> - 5–10 public news websites
> - Approximately 500 new articles per day
> - Approximately 3,000 text chunks per day
> - Approximately 3,000 embeddings per day
> - Approximately 120 RAG requests per day

---

## Cost Optimization Strategies

### 1. Configure Aurora Serverless v2 Appropriately

Aurora Serverless v2 represents the largest portion of the infrastructure cost. Configure the minimum and maximum ACU values according to your workload.

```hcl
serverlessv2_scaling_configuration {
  min_capacity = 0.5
  max_capacity = 2
}
```

**Recommendations**

- Development: **0.5–1 ACU**
- Production: **1–2 ACUs**
- Increase `max_capacity` only when higher concurrency is required.
- Monitor the **ServerlessDatabaseCapacity** metric in Amazon CloudWatch for capacity tuning.

---

### 2. Right-Size Lambda Memory

Allocate memory according to each Lambda function's workload to balance execution speed and cost.

| Lambda Function | Recommended Memory |
|-----------------|-------------------:|
| Consumer | 512 MB |
| ETL | 1024 MB |
| RAG API | 1024 MB |

Increasing memory also increases the available CPU, often reducing execution time with minimal impact on overall cost.

---

### 3. Run the Crawler as a Scheduled ECS Fargate Task

Instead of running continuously, the crawler is executed only when triggered by Amazon EventBridge.

Recommended task configuration:

- **CPU:** 0.25 vCPU
- **Memory:** 512 MB
- **Schedule:** Once per day

This ensures that compute charges are incurred only during execution.

---

### 4. Generate Embeddings Only for New Data

The ETL process should invoke Amazon Bedrock only for documents that have not been processed previously.

Recommended workflow:

- Check the article URL or SHA256 hash.
- Skip documents that already exist.
- Generate embeddings only for newly created text chunks.

This significantly reduces Bedrock inference costs.

---

### 5. Configure CloudWatch Log Retention

Avoid storing logs longer than necessary, especially in development environments.

```hcl
resource "aws_cloudwatch_log_group" "main" {
  retention_in_days = 7
}
```

Recommended retention periods:

- **Development:** 3–7 days
- **Production:** 14–30 days

---

### 6. Keep Only Required VPC Endpoints

VPC Endpoints improve security by keeping traffic within the AWS network, but they also contribute significantly to monthly costs.

Only create endpoints that are actually required by the application:

- Amazon Bedrock Runtime
- Amazon S3
- Amazon ECR
- Amazon CloudWatch Logs

Remove unused endpoints to reduce unnecessary expenses.

---

### 7. Use Amazon RDS Proxy

Amazon RDS Proxy manages database connections between AWS Lambda and Aurora PostgreSQL.

Benefits include:

- Reduces the number of direct database connections.
- Improves concurrent request handling.
- Prevents connection exhaustion during traffic spikes.
- Improves overall application stability.

---

## Cost Monitoring

### AWS Budgets

Create a monthly AWS Budget to receive notifications when spending approaches your predefined limit.

Example configuration:

- Monthly budget: **100 USD**
- Notifications at:
  - 50%
  - 80%
  - 100%

---

### AWS Cost Explorer

AWS Cost Explorer can be used to monitor:

- Cost by AWS service
- Daily and monthly spending
- Forecasted future costs
- Unexpected cost increases

---

## Cost Optimization Checklist

| ✓ | Optimization | Status | Impact |
|---|--------------|--------|--------|
| 1 | Configure Aurora Serverless v2 with a minimum of 0.5 ACU | | High |
| 2 | Generate embeddings only for new documents | | High |
| 3 | Schedule ECS Fargate tasks with EventBridge | | Medium |
| 4 | Allocate appropriate Lambda memory | | Medium |
| 5 | Configure CloudWatch log retention | | Low |
| 6 | Remove unused VPC Endpoints | | High |
| 7 | Configure AWS Budgets | | Medium |
| 8 | Use Amazon RDS Proxy | | High |

---

## Estimated Cost by Environment

| Environment | Aurora | RDS Proxy | Bedrock | Compute | Total |
|-------------|---------|-----------|----------|----------|------:|
| **Development** | Low ACU | Optional | Low usage | Low | **~$45–60/month** |
| **Staging** | Medium ACU | Enabled | Moderate | Moderate | **~$70–85/month** |
| **Production** | 0.5–2 ACUs | Enabled | Full workload | Full | **~$95–100/month** |

> Using separate AWS accounts or resource tags for Development, Staging, and Production environments is recommended for better cost allocation and monitoring.

---

**Next:** [Clean Up Resources](5.17-Cleanup/)
