---
title: "Blog 3: Optimizing the RAG Architecture"
date: 2026-07-27
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Realizing a Closed-Loop RAG System with Amazon Bedrock and Aurora pgvector

**Retrieval-Augmented Generation (RAG)** is a method that combines the power of Large Language Models (LLMs) with internal enterprise knowledge bases. For a RAG system to operate effectively and accurately, the "heart" of the architecture lies in two core components: the **Embedding Model** (converting text to vectors) and the **Vector Database** (storing and retrieving vectors). 

Below is the team's journey of building this "heart" entirely within the AWS ecosystem, prioritizing maximum performance and absolute security.

### 1. Amazon Bedrock: Flexible and Cost-Effective Embedding Power
Instead of self-hosting open-source models like HuggingFace SentenceTransformer on EC2 (which incurs high GPU maintenance costs) or forcing them to run on Lambda (often leading to slow processing and cold-start issues), the team chose the **Amazon Bedrock** service utilizing the `amazon.titan-embed-text-v2:0` model.

* **Optimal performance and cost:** Bedrock's API responds with ultra-low latency, processing embeddings for thousands of text chunks in a flash. Notably, the pay-per-token pricing mechanism makes this an extremely economical solution for agile projects.
* **Ensuring absolute consistency:** A vital principle of a RAG system is that the data stored in the Database and the user's Query must be mapped into the vector space by the **same model**. By invoking the Bedrock API in both the ETL data pipeline and the RAG API flow, the system consistently maintains the highest accuracy in semantic matching.

### 2. Amazon Aurora Serverless v2 + pgvector: "All-in-One" Storage
Instead of relying on independent third-party Vector databases (such as Qdrant, Pinecone, or Milvus), the team decided to leverage **Amazon Aurora PostgreSQL** combined with the **`pgvector`** extension.

* **Combining Vector and Relational Data:** PostgreSQL allows the team to store metadata (like author, publication date, URL) in a traditional relational structure alongside a `vector` data column containing the embeddings. This design excellently resolves Hybrid Search scenarios. For example: *"Find articles related to economic fluctuations (Vector Search) but only filter those published in the last 7 days (SQL Filter)"*.
* **Speed optimization with HNSW index:** To handle search queries across hundreds of thousands of news data chunks, the team configured the **HNSW (Hierarchical Navigable Small World)** index on the vector column. Consequently, cosine similarity search queries return results almost instantly (real-time).
* **Auto-scaling:** Leveraging the architecture of Aurora Serverless v2, the database can automatically adjust compute resources (scaling up/down from 0 to 2 ACU) based on the actual system load. This ensures a smooth Q&A experience during peak hours without wasting resources during idle times.

### Architectural Vision and Conclusion
The combined architecture of Amazon Bedrock and Aurora pgvector delivers a fully closed-loop (End-to-End) RAG pipeline directly on AWS. Sensitive news data never has to leave the internal network (VPC) to call external APIs, ensuring absolute security and minimizing network latency. 

This is the foundational technical highlight that the team is most proud of in the entire architectural design process.

---
**References:**
1. [Amazon Titan Text Embeddings models](https://docs.aws.amazon.com/bedrock/latest/userguide/titan-embedding-models.html)
2. [Running pgvector in production on Amazon Aurora PostgreSQL](https://aws.amazon.com/blogs/database/running-pgvector-in-production-on-amazon-aurora-postgresql/)
3. [pgvector: Open-source vector similarity search for Postgres](https://github.com/pgvector/pgvector)