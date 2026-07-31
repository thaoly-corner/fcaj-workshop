---
title: "Workshop Overview"
date: 2026-07-28
weight: 1
chapter: false
pre: "<b>5.1</b>"
---

# News RAG Pipeline on AWS

This workshop guides you through building a complete **News Retrieval-Augmented Generation (News RAG) Pipeline** on AWS using a **serverless architecture**. The pipeline automates the entire workflow—from collecting news articles and processing data, to generating vector embeddings, storing them in a data warehouse, and serving intelligent question-answering through Large Language Models (LLMs).

By the end of this workshop, you will be able to deploy a production-ready RAG system on AWS that is scalable, cost-efficient, and fully automated.

## Architecture Overview

{{< event-image src="images/5-Workshop/5.1-Workshop-overview/RAG-Pipeline.png" alt="News RAG Pipeline Architecture" >}}

The pipeline consists of the following stages:

1. **Crawler** collects news articles from public news websites.
2. **Amazon SQS** receives and distributes incoming messages.
4. **ETL Lambda** cleans the content, splits it into chunks, and generates embeddings using Amazon Bedrock.
5. **Aurora PostgreSQL + pgvector** stores both structured data and vector embeddings.
6. **RAG API** performs semantic retrieval and generates responses using an LLM.
7. **Frontend Dashboard** provides search, chat, and pipeline monitoring capabilities.

### Data Ingestion Flow

{{< event-image src="images/5-Workshop/5.1-Workshop-overview/Ingestion.png" alt="Data Ingestion Flow" >}}

The ingestion process automates data collection and vectorization:
*   **Trigger:** Amazon EventBridge triggers the Amazon ECS Fargate Crawler on a predefined schedule.
*   **Execution:** The Crawler retrieves its container image from Amazon ECR via an Interface VPC Endpoint. It accesses the internet through a NAT Gateway to scrape public news articles.
*   **Messaging:** Raw article data is sent as messages to an Amazon SQS queue via an Interface VPC Endpoint.
*   **Processing:** The SQS queue triggers the AWS Lambda (ETL + Embedding) function. 
*   **Embedding & Storage:** The Lambda function requests vector embeddings from Amazon Bedrock via an Interface VPC Endpoint and stores the resulting vectors in Amazon Aurora PostgreSQL utilizing an RDS Proxy for connection management.

### RAG Query Flow

{{< event-image src="images/5-Workshop/5.1-Workshop-overview/RAG-Query.png" alt="RAG Query Flow" >}}

The retrieval and generation process manages user interactions and answers queries:
*   **User Request:** A user submits a query through the application interface, which is routed through Amazon Route 53, AWS WAF, Amazon CloudFront, and finally Amazon API Gateway.
*   **Invocation:** API Gateway invokes the AWS Lambda (RAG API) function.
*   **Query Embedding:** The Lambda function sends the query to Amazon Bedrock (via PrivateLink) to generate a query vector.
*   **Similarity Search:** The query vector is used to perform a similarity search against the Amazon Aurora PostgreSQL database (routed through RDS Proxy) to retrieve the top *n* relevant text chunks.
*   **Answer Generation:** The retrieved chunks are reranked and sent back to Amazon Bedrock alongside the original query to generate a contextually accurate answer.
*   **Response:** The final answer and associated source documents are returned to the user.

---

---

# Learning Objectives

After completing this workshop, you will be able to:

- Provision AWS infrastructure using **Terraform**.
- Package and deploy a **Scrapy crawler** on Amazon ECS Fargate.
- Design a **Star Schema** data warehouse on Aurora PostgreSQL.
- Build an event-driven data pipeline using **Amazon SQS** and **AWS Lambda**.
- Generate vector embeddings using **Amazon Bedrock Titan Embeddings v2**.
- Build a Retrieval-Augmented Generation (RAG) system with **pgvector** and LLMs.
- Monitor, test, and optimize the complete pipeline.

---

# Workshop Modules

| Module | Description |
|---------|-------------|
| **5.1** | Workshop Overview | 
| **5.2** | Prerequisites |
| **5.3** | Infrastructure Deployment with Terraform | 
| **5.4** | Building the ECS Fargate Crawler | 
| **5.5** | Amazon SQS & Lambda Consumer | 
| **5.6** | ETL, Chunking & Embedding | 
| **5.7** | Building the RAG API | 
| **5.8** | Frontend Dashboard Integration | 
| **5.9** | Testing & Monitoring | 
| **5.10** | Resource Cleanup | 

---

# Prerequisites

Before starting this workshop, make sure you have the following:

### AWS Services

- AWS Account
- Permissions for:
  - Amazon VPC
  - Amazon ECS
  - Amazon ECR
  - Amazon Aurora PostgreSQL
  - AWS Lambda
  - Amazon SQS
  - Amazon EventBridge
  - Amazon API Gateway
  - Amazon Bedrock
  - Amazon CloudWatch
  - AWS IAM

### Required Tools

- AWS CLI
- Terraform (>= 1.5)
- Docker & Docker Compose
- Python 3.10+
- Git
- Visual Studio Code (recommended)

---

# Estimated Monthly Cost

The following estimates are based on the **ap-southeast-2 (Sydney)** AWS Region.

| AWS Service | Purpose | Estimated Cost |
|-------------|---------|---------------:|
| Amazon ECS Fargate | Run the Scrapy crawler | ~$1.11 |
| Aurora Serverless v2 | Data Warehouse + pgvector | ~$44.66 |
| Amazon Bedrock | Embedding & LLM inference | ~$10.00 |
| AWS Lambda | ETL & RAG API | ~$3.00 |
| VPC Endpoints + NAT Gateway | Private networking | ~$68.00 |
| Amazon S3 | Logs and Terraform state | ~$0.80 |
| Amazon SQS + EventBridge | Queue & Scheduler | ~$0.00 |
| Amazon CloudWatch | Logging & Monitoring | ~$5.80 |
| **Total** | | **~$133.37/month** |

> **Note**
>
> This is an estimated cost for a demonstration environment. Actual costs may be significantly lower if you use AWS Free Tier resources, reduce the workload, or terminate resources after completing the workshop.

### Workload Assumptions

| Item | Value |
|------|-------|
| News websites | 5–10 public news sites |
| New articles | ~500 articles/day |
| Average article length | ~1,200 words |
| Generated chunks | ~3,000 chunks/day |
| Generated embeddings | ~3,000 vectors/day |
| RAG requests | ~120 requests/day |

---

# Project Structure

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

# Architecture Migration: v1 → v2

This workshop is based on an upgraded architecture from the original thesis implementation. The new version focuses on a **simpler, fully serverless, and AWS-native architecture** that is easier to deploy, maintain, and scale.

| Component | v1 | v2 |
|-----------|----|----|
| Crawler | Lambda + Scrapy | ECS Fargate + SitemapSpider |
| Message Queue | Apache Kafka | Amazon SQS |
| Embedding | Local BGE model | Amazon Bedrock Titan Embeddings v2 |
| Vector Database | Qdrant | Aurora PostgreSQL + pgvector |
| Vectorization | Dedicated Fargate task | Integrated into ETL Lambda |
| Query Embedding | Local embedding model | Amazon Bedrock |

### Why migrate to v2?

- Eliminate self-managed infrastructure such as Kafka and Qdrant.
- Reduce the number of services to operate.
- Leverage AWS managed serverless services.
- Simplify infrastructure deployment using Terraform.
- Build a fully AWS-native architecture.
- Improve scalability, maintainability, and operational efficiency.

> **Important**
>
> Both the ETL pipeline and the RAG API **must use the same embedding model** (`amazon.titan-embed-text-v2:0`).
>
> Using different embedding models results in different vector spaces, causing semantic retrieval to become inaccurate.

---

**Next:** [Prerequisites](5.2-Prerequisites/)
