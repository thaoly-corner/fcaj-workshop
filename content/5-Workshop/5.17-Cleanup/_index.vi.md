---
title: "Dọn dẹp tài nguyên"
date: 2024-01-01
weight: 17
chapter: false
pre: " <b> 5.17 </b> "
---

# Dọn dẹp tài nguyên

Phần này cung cấp hướng dẫn từng bước để xóa toàn bộ tài nguyên AWS được tạo trong workshop để tránh chi phí phát sinh.

## ⚠️ Trước khi bắt đầu

**Sao lưu dữ liệu quan trọng!** Quy trình này **không thể đảo ngược** và sẽ xóa vĩnh viễn:
- CSDL Aurora PostgreSQL (toàn bộ bài viết, chunks, embeddings)
- Ảnh Docker trên ECR
- CloudWatch Logs
- Đối tượng S3 (nếu có)
- Các Lambda function và các version của chúng

## Phương án 1: Terraform Destroy (Khuyến nghị)

Xóa toàn bộ tài nguyên do Terraform quản lý theo đúng thứ tự phụ thuộc.

```bash
# 1. Di chuyển đến thư mục gốc dự án
cd AWS-Projects

# 2. Xem trước những gì sẽ bị xóa
terraform plan -destroy

# 3. Thực hiện xóa (nhập 'yes' khi được nhắc)
terraform destroy

# 4. Xác minh tất cả tài nguyên đã bị xóa
terraform show
```

**Kết quả mong đợi:**
```
Plan: 0 to add, 0 to change, 38 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

aws_lambda_function.rag_api: Destroying... [id=newsrag-rag-api]
aws_lambda_function.etl: Destroying... [id=newsrag-etl]
aws_lambda_function.consumer: Destroying... [id=newsrag-consumer]
aws_ecs_task_definition.crawler: Destroying... [id=newsrag-crawler]
aws_ecs_task_definition.etl: Destroying... [id=newsrag-etl]
aws_cloudwatch_event_target.crawler_target: Destroying... [id=newsrag-crawler-target]
aws_cloudwatch_event_target.etl_target: Destroying... [id=newsrag-etl-target]
aws_cloudwatch_event_rule.crawler_schedule: Destroying... [id=newsrag-crawler-rule]
aws_cloudwatch_event_rule.etl_schedule: Destroying... [id=newsrag-etl-rule]
aws_ecr_repository.api: Destroying... [id=newsrag-api]
aws_ecs_cluster.cluster: Destroying... [id=newsrag-cluster]
aws_db_instance.main: Destroying... [id=newsrag-postgres-1]
aws_rds_cluster.main: Destroying... [id=newsrag-postgres]
...
Destroy complete! Resources: 38 destroyed.
```

## Phương án 2: Dọn dẹp thủ công (Nếu mất Terraform state)

Nếu không có Terraform state, hãy dọn dẹp thủ công theo thứ tự sau:

### 1. Xóa Lambda Functions
```bash
aws lambda delete-function --function-name newsrag-consumer --region ap-southeast-2
aws lambda delete-function --function-name newsrag-etl --region ap-southeast-2
aws lambda delete-function --function-name newsrag-rag-api --region ap-southeast-2
```

### 2. Xóa EventBridge Rules & Targets
```bash
# Xóa targets trước
aws events remove-targets --rule newsrag-crawler-rule --ids crawler-target --region ap-southeast-2
aws events remove-targets --rule newsrag-etl-rule --ids etl-target --region ap-southeast-2
aws events remove-targets --rule newsrag-vectorize-rule --ids vectorize-target --region ap-southeast-2

# Xóa rules
aws events delete-rule --name newsrag-crawler-rule --region ap-southeast-2
aws events delete-rule --name newsrag-etl-rule --region ap-southeast-2
aws events delete-rule --name newsrag-vectorize-rule --region ap-southeast-2
```

### 3. Xóa tài nguyên ECS
```bash
# Hủy đăng ký task definitions
aws ecs deregister-task-definition --task-definition newsrag-crawler --region ap-southeast-2
aws ecs deregister-task-definition --task-definition newsrag-etl --region ap-southeast-2
aws ecs deregister-task-definition --task-definition newsrag-vectorize --region ap-southeast-2

# Xóa cluster
aws ecs delete-cluster --cluster newsrag-cluster --region ap-southeast-2
```

### 4. Xóa ECR Repository
```bash
# Xóa tất cả ảnh trước
aws ecr batch-delete-image \
  --repository-name newsrag-api \
  --image-ids $(aws ecr list-images --repository-name newsrag-api --query 'imageIds[*]' --output json) \
  --region ap-southeast-2

# Xóa repository
aws ecr delete-repository --repository-name newsrag-api --force --region ap-southeast-2
```

### 5. Xóa cụm RDS Aurora
```bash
# Xóa instance trước
aws rds delete-db-instance \
  --db-instance-identifier newsrag-postgres-1 \
  --skip-final-snapshot \
  --region ap-southeast-2

# Đợi instance xóa xong, sau đó xóa cluster
aws rds delete-db-cluster \
  --db-cluster-identifier newsrag-postgres \
  --skip-final-snapshot \
  --region ap-southeast-2

# Xóa subnet group
aws rds delete-db-subnet-group --db-subnet-group-name newsrag-db-subnet --region ap-southeast-2
```

### 6. Xóa tài nguyên VPC
```bash
# Lấy VPC ID
VPC_ID=$(aws ec2 describe-vpcs --filters "Name=tag:Name,Values=newsrag-vpc" --query "Vpcs[0].VpcId" --output text)

# Xóa security groups
aws ec2 delete-security-group --group-id $(aws ec2 describe-security-groups --filters "Name=group-name,Values=newsrag-ecs-sg" --query "SecurityGroups[0].GroupId" --output text) --region ap-southeast-2
aws ec2 delete-security-group --group-id $(aws ec2 describe-security-groups --filters "Name=group-name,Values=newsrag-rds-sg" --query "SecurityGroups[0].GroupId" --output text) --region ap-southeast-2
aws ec2 delete-security-group --group-id $(aws ec2 describe-security-groups --filters "Name=group-name,Values=newsrag-lambda-sg" --query "SecurityGroups[0].GroupId" --output text) --region ap-southeast-2

# Xóa subnets
aws ec2 delete-subnet --subnet-id $(aws ec2 describe-subnets --filters "Name=vpc-id,Values=$VPC_ID" "Name=availability-zone,Values=ap-southeast-2a" --query "Subnets[0].SubnetId" --output text) --region ap-southeast-2
aws ec2 delete-subnet --subnet-id $(aws ec2 describe-subnets --filters "Name=vpc-id,Values=$VPC_ID" "Name=availability-zone,Values=ap-southeast-2b" --query "Subnets[0].SubnetId" --output text) --region ap-southeast-2

# Tách và xóa IGW
IGW_ID=$(aws ec2 describe-internet-gateways --filters "Name=attachment.vpc-id,Values=$VPC_ID" --query "InternetGateways[0].InternetGatewayId" --output text)
aws ec2 detach-internet-gateway --internet-gateway-id $IGW_ID --vpc-id $VPC_ID --region ap-southeast-2
aws ec2 delete-internet-gateway --internet-gateway-id $IGW_ID --region ap-southeast-2

# Xóa VPC
aws ec2 delete-vpc --vpc-id $VPC_ID --region ap-southeast-2
```

### 7. Xóa IAM Roles & Policies
```bash
# Tách policies trước
aws iam detach-role-policy --role-name newsrag-ecs-execution-role --policy-arn arn:aws:iam::aws:policy/service-role/AmazonECSTaskExecutionRolePolicy --region ap-southeast-2
aws iam detach-role-policy --role-name newsrag-ecs-task-role --policy-arn arn:aws:iam::123456789012:policy/newsrag-ecs-task-permissions --region ap-southeast-2
aws iam detach-role-policy --role-name newsrag-lambda-role --policy-arn arn:aws:iam::123456789012:policy/newsrag-lambda-permissions --region ap-southeast-2

# Xóa inline policies
aws iam delete-role-policy --role-name newsrag-ecs-task-role --policy-name newsrag-ecs-task-permissions --region ap-southeast-2
aws iam delete-role-policy --role-name newsrag-lambda-role --policy-name newsrag-lambda-permissions --region ap-southeast-2

# Xóa roles
aws iam delete-role --role-name newsrag-ecs-execution-role --region ap-southeast-2
aws iam delete-role --role-name newsrag-ecs-task-role --region ap-southeast-2
aws iam delete-role --role-name newsrag-lambda-role --region ap-southeast-2
```

### 8. Xóa CloudWatch Log Groups
```bash
aws logs delete-log-group --log-group-name /ecs/newsrag-project --region ap-southeast-2
aws logs delete-log-group --log-group-name /aws/lambda/newsrag-consumer --region ap-southeast-2
aws logs delete-log-group --log-group-name /aws/lambda/newsrag-etl --region ap-southeast-2
aws logs delete-log-group --log-group-name /aws/lambda/newsrag-rag-api --region ap-southeast-2
```

### 9. Xóa SQS Queues
```bash
aws sqs delete-queue --queue-url https://sqs.ap-southeast-2.amazonaws.com/123456789012/newsrag-news-raw --region ap-southeast-2
aws sqs delete-queue --queue-url https://sqs.ap-southeast-2.amazonaws.com/123456789012/newsrag-news-raw-dlq --region ap-southeast-2
```

### 10. Xóa API Gateway
```bash
API_ID=$(aws apigateway get-rest-apis --query "items[?name=='newsrag-api'].id" --output text)
aws apigateway delete-rest-api --rest-api-id $API_ID --region ap-southeast-2
```

### 11. Xóa Secrets Manager Secrets
```bash
aws secretsmanager delete-secret --secret-id newsrag/db-credentials --force-delete-without-recovery --region ap-southeast-2
```

### 12. Xóa SNS Topic (Alarms)
```bash
aws sns delete-topic --topic-arn arn:aws:sns:ap-southeast-2:123456789012:newsrag-alerts --region ap-southeast-2
```

## Xác minh dọn dẹp hoàn tất

```bash
# Kiểm tra các tài nguyên còn lại có tag "newsrag"
aws resourcegroupstaggingapi get-resources \
  --tag-filters Key=Name,Values=newsrag* \
  --region ap-southeast-2

# Kiểm tra từng dịch vụ
aws lambda list-functions --query "Functions[?contains(FunctionName, 'newsrag')].FunctionName" --region ap-southeast-2
aws ecs list-clusters --query "clusterArns[?contains(@, 'newsrag')]" --region ap-southeast-2
aws rds describe-db-clusters --query "DBClusters[?contains(DBClusterIdentifier, 'newsrag')].DBClusterIdentifier" --region ap-southeast-2
aws ecr describe-repositories --query "repositories[?contains(repositoryName, 'newsrag')].repositoryName" --region ap-southeast-2
aws sqs list-queues --queue-name-prefix newsrag --region ap-southeast-2
aws apigateway get-rest-apis --query "items[?contains(name, 'newsrag')].id" --region ap-southeast-2
```

Tất cả phải trả về kết quả rỗng `[]`.

## Dọn dẹp file cục bộ

```bash
# Xóa Terraform state (nếu không dùng remote backend)
rm -rf .terraform terraform.tfstate terraform.tfstate.backup

# Xóa các package deployment
rm -f consumer.zip etl.zip rag.zip

# Xóa Docker images
docker rmi news-crawler:latest
docker rmi $(docker images -q -f dangling=true)

# Xóa Python virtual environment
rm -rf venv

# Xóa file .env (tùy chọn, giữ lại cho lần sau)
# rm -f .env terraform.tfvars
```

## Kiểm tra chi phí

Sau khi dọn dẹp, kiểm tra không còn chi phí phát sinh:

1. **AWS Billing Dashboard** → Kiểm tra "Cost Explorer" cho tháng hiện tại
2. **Thiết lập Budget Alert** cho $1/tháng để phát hiện tài nguyên bị sót:
   ```bash
   aws budgets create-budget --account-id $(aws sts get-caller-identity --query Account --output text) --budget file://budget.json
   ```

## Sự cố thường gặp

| Sự cố | Giải pháp |
|-------|----------|
| `DependencyViolation` khi xóa VPC | Xóa ENIs trước: `aws ec2 describe-network-interfaces --filters Name=vpc-id,Values=$VPC_ID` sau đó `aws ec2 delete-network-interface` |
| `DBInstanceNotFound` | Instance đã bị xóa, chuyển sang xóa cluster |
| `ResourceInUseException` (Lambda) | Đợi 1-2 phút để EventBridge targets được tách |
| `InvalidParameterException` (RDS) | Đảm bảo dùng `skip-final-snapshot` cho môi trường dev |

---

## Workshop Hoàn Thành! 🎉

Bạn đã xây dựng và triển khai thành công **News RAG Pipeline trên AWS sẵn sàng cho production** với:

- ✅ **Infrastructure as Code** (Terraform)
- ✅ **Kiến trúc Serverless** (Fargate, Lambda, Aurora Serverless v2)
- ✅ **Pipeline_event-driven** (EventBridge → SQS → Lambda → Bedrock → pgvector)
- ✅ **RAG API** (Vector search + LLM generation)
- ✅ **Giám sát & Cảnh báo** (CloudWatch, Logs, Metrics)
- ✅ **Tối ưu chi phí** (~$21-26/tháng)
- ✅ **Sẵn sàng CI/CD** (GitHub Actions)

### Các bước tiếp theo cho Production

1. **Thêm xác thực** - API Gateway Authorizers (Cognito/JWT)
2. **Domain tùy chỉnh** - Route 53 + ACM certificate cho API Gateway
3. **WAF** - Bảo vệ API khỏi lạm dụng
4. **Aurora Multi-AZ** - Bật `storage_encrypted`, `deletion_protection`
5. **RDS Proxy** - Pooling kết nối khi mở rộng
6. **Secrets Rotation** - Tự động xoay vòng credentials
7. **Blue/Green Deploy** - Circuit deployment cho ECS
8. **Chaos Engineering** - Kiểm tra kịch bản lỗi

---

**Cảm ơn bạn đã hoàn thành workshop!**

*Repository: https://github.com/your-org/AWS-Projects*