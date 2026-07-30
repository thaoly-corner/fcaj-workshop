---
title: "Blogs & Practical Experiences"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

> **Note:**  
> According to the initial plan of the internship program, these technical sharing articles were expected to be published publicly on the Facebook community group. However, due to the specific nature of the NewsRAG project—continuously scraping and processing massive amounts of data from major online news publishers—the team decided to package this knowledge as **Internal Blogs** within this report. This is not just a project diary, but also the most authentic summary of the technical challenges, optimization mindset, and hands-on experiences that our entire team has accumulated throughout the system building process.

---

Below are 3 blog posts highlighting the most significant technical milestones in building and optimizing the NewsRAG system:

### [Blog 1 - OVERCOMING AWS LAMBDA TIMEOUT LIMITS WITH ECS FARGATE](3.1-Blog1/)
* **The Challenge:** During operations, the Crawler system encountered a bottleneck due to AWS Lambda's maximum execution timeout of 15 minutes.
* **Solution & Takeaways:** This article recounts the team's agile restructuring decision to transition to the **Amazon ECS Fargate** container service. This architectural shift not only helped the crawler bypass time constraints but also ensured a continuous, stable, and resilient data ingestion pipeline.

### [Blog 2 - SYSTEM COST OPTIMIZATION: FROM APACHE KAFKA TO AMAZON SQS](3.2-Blog2/)
* **The Challenge:** Maintaining a 24/7 server for a message broker like Apache Kafka created a substantial infrastructure cost burden, especially during the development phase.
* **Solution & Takeaways:** This article provides an in-depth analysis of architectural trade-offs. By boldly removing Kafka and switching to the Serverless **Amazon SQS** service, the team slashed queue maintenance costs to **nearly $0**, while fully preserving the performance and scalability of the data streaming pipeline.

### [Blog 3 - REALIZING A CLOSED-LOOP RAG SYSTEM WITH AMAZON BEDROCK AND AURORA PGVECTOR](3.3-Blog3/)
* **The Challenge:** How to build a RAG architecture that is not only powerful but also ensures absolute security, preventing data leakage to external third-party services?
* **Solution & Takeaways:** This article emphasizes a highly secure system design mindset. Instead of using third-party Vector databases, the team successfully integrated **Amazon Bedrock** with **Aurora Serverless v2 (supporting pgvector)**. The result is a fully closed-loop RAG workflow operating entirely within the AWS boundary, meeting stringent enterprise-grade architecture standards.s