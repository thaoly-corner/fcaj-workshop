---
title: "Proposal"
date: 2026-06-15
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
This section summarizes the planned tasks and technical directions to be executed within the scope of the workshop.

# News RAG Pipeline on AWS
## Serverless News Data Pipeline with RAG for Intelligent Information Retrieval

### 1. Executive Summary
**News RAG Pipeline** is an end-to-end solution for building an automated news retrieval and question-answering (Q&A) application. The system proactively collects articles from major Vietnamese news platforms (VnExpress, Thanh Niên, VietnamNet), normalizes data into a Data Warehouse (Star Schema), generates vector embeddings via Amazon Bedrock, and enables natural language user queries using a Retrieval-Augmented Generation (RAG) architecture. Fully operating on AWS Serverless infrastructure integrated with advanced LLM APIs (Groq, Gemini), the project demonstrates practical cloud-native integration for modern AI-driven search and retrieval platforms.

### 2. Problem Statement & Proposed Solution
#### Real-World Challenges
Tracking news across multiple digital publishers is time-consuming due to manual searching and reading. There is a lack of centralized platforms capable of providing direct Q&A on recent events alongside verified source citations. Meanwhile, popular general-purpose AI tools like ChatGPT often lack real-time news context.

#### Proposed Solution
The News RAG Pipeline automates the entire workflow across 4 key steps: (1) Ingestion of news articles via Scrapy SitemapSpider running on ECS Fargate, (2) Messaging through SQS for staging into Aurora PostgreSQL, (3) Lambda ETL execution for HTML cleaning, Star Schema transformation, and vector embedding generation using Amazon Bedrock Titan Embed v2, and (4) Lambda RAG API execution to handle natural language queries, perform vector similarity search on pgvector (HNSW index), and synthesize responses using LLMs. The whole workflow is scheduled and orchestrated automatically via EventBridge Scheduler.

#### Value Proposition & Cost Efficiency
This project offers hands-on experience in MLOps, RAG systems, and AWS Serverless architectures. Key benefits include automated news aggregation eliminating manual research, AI Q&A with precise source citations, and optimized operating costs via serverless pricing models (estimated at ~$21–$26 USD/month).

### 3. System Architecture & Design
The system operates on an AWS Serverless architecture split into two primary pipelines: (1) **Data Pipeline** — EventBridge Scheduler triggers the ECS Fargate crawler daily at 01:00 UTC to ingest articles and enqueue messages into SQS; a Lambda Consumer processes messages, applies SHA256 URL hashing for deduplication, and stores raw data in Aurora PostgreSQL. (2) **ETL + RAG Pipeline** — A second EventBridge schedule triggers the Lambda ETL function at 02:00 UTC to sanitize HTML, chunk text (500-token chunk size), generate 1024-dimensional embeddings via Bedrock Titan Embed v2, and write vectors to Aurora pgvector (HNSW index). When a user submits a query through API Gateway, the Lambda RAG API embeds the query, performs cosine similarity search on pgvector, and routes context to an LLM (Groq/Gemini) to output an answer with source links.

### AWS Services Catalog
- **Amazon ECS Fargate**: Execution environment for the Scrapy SitemapSpider crawler (configured with 0.25 vCPU, 0.5 GB RAM).
- **Amazon SQS Standard**: Decoupled messaging queue replacing Kafka (~$0/month base cost).
- **AWS Lambda** (3 execution functions): Consumer (SQS → Aurora), ETL + Bedrock Embed, and RAG API.
- **Amazon Aurora Serverless v2**: PostgreSQL 15.4+ relational database with native pgvector extension.
- **Amazon Bedrock**: High-performance vector embeddings via Titan Embed Text v2 (1024 dimensions).
- **Amazon API Gateway**: RESTful API frontend endpoint for client integration.
- **Amazon EventBridge Scheduler**: Cron-based automated job scheduling (01:00 and 02:00 UTC).
- **Amazon ECR**: Container registry for hosting Fargate task Docker images.
- **AWS IAM**: Least-privilege access management for Fargate Tasks and Lambda Roles.
- **Amazon CloudWatch**: Centralized log collection and system monitoring (7-day retention).

### Component Breakdown
- **Crawler (Fargate)**: Scrapy SitemapSpider parsing `sitemap_news.xml` across 3 publisher domains, extracting content, and publishing to SQS. Runtime ~30 mins/day.
- **Queue (SQS)**: Standard queue with Dead Letter Queue (DLQ), 14-day message retention, and 3 max retry attempts.
- **Ingestion Handler (Lambda Consumer)**: SQS-triggered function calculating SHA256 URL hashes for deduplication prior to Aurora raw storage.
- **Transformation & Vectorization (Lambda ETL)**: Strips HTML tags, splits text into chunks (500 tokens/chunk, 50-token overlap), calls Bedrock Titan Embed v2, and saves 1024d vectors to pgvector with an HNSW index.
- **RAG Processor (Lambda RAG API)**: Vectorizes user input queries, performs cosine similarity search on pgvector, and streams prompt context to Groq/Gemini for response generation with source citations.
- **User Interface (Next.js + FastAPI)**: Management dashboard with KPI metrics, visual charts (Recharts), interactive AI Chat with model selection, Article Explorer, and Pipeline Monitor.

### 4. Technical Implementation
#### Implementation Phases
The project is structured into 4 distinct phases:
- **Phase 1 – Infrastructure & Containerization (Weeks 1-2)**: Terraform provisioning for VPC, Aurora pgvector, ECS Cluster, ECR, Lambda, EventBridge, IAM, and CloudWatch. Multi-stage Docker build configuration for Fargate Tasks.
- **Phase 2 – Local Development (Weeks 3-6)**: Local Docker Compose setup containing PostgreSQL, Qdrant, and Kafka. Development of Scrapy SitemapSpider, Kafka Consumer, Star Schema ETL pipeline, and SentenceTransformer vectorization.
- **Phase 3 – AWS Production Deployment (Weeks 6-7)**: Deployment of Fargate crawler with EventBridge schedules, SQS-triggered Lambda Consumer, Lambda ETL integrated with Bedrock Titan Embed v2, Lambda RAG API behind API Gateway, and Next.js frontend hosting.
- **Phase 4 – Evaluation & Optimization (Weeks 7-8)**: CloudWatch dashboard and alarm setup, Locust load testing, and cost optimization.

#### Core Technical Requirements
- **Data Pipeline**: Scrapy (SitemapSpider), Kafka (Local) / SQS (AWS Cloud), PostgreSQL with pgvector.
- **ETL Pipeline**: Regex HTML sanitization, 500-token text chunking, embedding generation using SentenceTransformer (Local) / Bedrock Titan v2 (AWS).
- **RAG Engine**: pgvector HNSW similarity search (cosine distance), Groq API integration (Qwen3-8B, Llama 3.1), Gemini 2.0 Flash fallback mechanism, structured prompt templates with source citations.
- **Infrastructure Management**: Infrastructure as Code via Terraform, multi-stage Docker builds, Lambda deployment packages, and EventBridge Cron scheduling.

### 5. Timeline & Milestones
**Project Roadmap**
- **Preparation Phase (Week 0)**: Research, core AWS fundamentals learning, and detailed technical planning.
- **Execution Phase (Weeks 1-3)**:
  - *Week 1*: Infrastructure provisioning, local dev setup, and web crawler completion.
  - *Week 2*: ETL pipeline construction, Star Schema data modeling, vectorization, and RAG API development.
  - *Week 3*: Full production rollout on AWS, quality evaluation, monitoring setup, and cost tuning.
- **Post-Launch Phase**: System maintenance and feature extensions (semantic chunking, hybrid search, topic alert notifications).

### 6. Budget Estimation
Estimated monthly infrastructure costs (Referenced from [AWS Pricing Calculator](https://calculator.aws/#/)):

| Service | Estimated Monthly Cost |
| :--- | :--- |
| Aurora Serverless v2 (2 ACU) | ~$15 - $20 |
| ECS Fargate Crawler (0.25 vCPU, 0.5 GB, 30 mins/day) | ~$0.50 |
| AWS Lambda (3 functions) | ~$2 - $3 |
| SQS Standard | ~$0.00 |
| API Gateway | ~$0.30 |
| Bedrock Titan Embed | ~$0.50 |
| CloudWatch Logs (7-day retention) | ~$1 - $2 |
| **Total Estimated Cost** | **~$21 - $26 / month** |

### 7. Risk Management
#### Risk Matrix & Impact Analysis
- **Crawler IP Blocking**: Medium Impact, Medium Probability (Mitigation: Set appropriate request headers, comply with `robots.txt`, configure `DOWNLOAD_DELAY`).
- **Bedrock Throttling**: Medium Impact, Low Probability (Mitigation: Implement retry logic with Exponential Backoff).
- **LLM API Outage (Groq/Gemini)**: High Impact, Low Probability (Mitigation: Multi-model fallback system).
- **Unexpected Cost Overruns**: Medium Impact, Low Probability (Mitigation: Set AWS Budget alerts, scale resources conservatively).

#### Risk Mitigation Strategies
- **Crawler**: Enforce `robots.txt` compliance, maintain `DOWNLOAD_DELAY >= 1s`, enable AutoThrottle.
- **Throttling Handling**: Apply Exponential Backoff on Bedrock API calls with `max_attempts=3`.
- **LLM Fallback Chain**: Order of fallback: Groq Qwen3 → Llama 3.1 → Gemini 2.0 Flash.
- **Cost Controls**: Configure AWS Budget alerts at the 80% threshold, run crawler on Fargate Spot, right-size Lambda memory allocation.

#### Contingency Plans
- If crawler is blocked by publishers: Switch to alternative news RSS/Sitemaps or enable manual data ingestion.
- If Bedrock becomes unavailable: Fall back to locally hosted SentenceTransformer embeddings.
- If cost exceeds threshold: Scale down Aurora Serverless minimum capacity to 1 ACU.

### 8. Expected Outcomes
#### Technical Improvements
Full automation of the news aggregation lifecycle, removing manual research overhead. Intelligent Q&A capabilities backed by traceable source citations. Elastic serverless infrastructure capable of handling growing article ingestion volumes.

#### Long-Term Value
- Establishes a standard RAG architecture template for future NLP/AI initiatives.
- Reusable pipeline modules applicable to other domains (tech blogs, research paper indexing).
- Practical expertise in production-grade AWS Serverless ecosystem deployment.
- High-quality news repository dataset (~5,000 articles) suitable for downstream analytics.