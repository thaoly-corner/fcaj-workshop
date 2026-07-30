---
title: "Worklog - Week 5"
date: 2026-06-29
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:
* **Smooth Kickoff:** Successfully tested the Amazon Bedrock API (`amazon.titan-embed-text-v2:0`) locally following successful limit removal.
* **Database Infrastructure:** Proceeded with creating an Aurora Serverless DB and configuring it on AWS, including the `pgvector` extension.
* **Vectorize Pipeline Migration:** Moved local preprocessing and chunking logic (from Week 3) to AWS Lambda, integrating Bedrock API calls.
* **Public RAG API:** Deployed the Lambda RAG API integrated with API Gateway, completing the Retrieval (HNSW Search) and Generation loop.

### Tasks Completed During the Week:
| Day | Task | Start Date | Completion Date | Reference Material |
| :---: | :--- | :---: | :---: | :--- |
| 2 | - Tested Bedrock API calls locally using `boto3` to ensure model stability. <br> - Proceeded with creating the Aurora Serverless DB and configuring AWS settings, enabling the `pgvector` extension. | 06/29/2026 | 06/29/2026 | [Boto3 Bedrock Runtime](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/bedrock-runtime.html) |
| 3 | - Migrated Vectorize logic to AWS Lambda: Transitioned HTML cleaning and text chunking algorithms to the Serverless environment. | 06/30/2026 | 06/30/2026 | [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/) |
| 4 | - Integrated Bedrock Titan Embed into Lambda: Processed text chunks into 1024-dimensional vector embeddings. <br> - Implemented exception handling logic for Bedrock API requests. | 07/01/2026 | 07/01/2026 | [Amazon Bedrock Docs](https://docs.aws.amazon.com/bedrock/) |
| 5 | - Configured the `psycopg2` library for Lambda to connect and directly insert data into the newly created Aurora database. | 07/02/2026 | 07/02/2026 | [psycopg2 Docs](https://www.psycopg.org/docs/) |
| 6 | - Migrated the RAG API function to Lambda: Received user queries -> Embed -> Performed HNSW Top-K retrieval via pgvector -> Constructed prompts for LLM response generation. | 07/03/2026 | 07/03/2026 | [pgvector HNSW index tuning](https://github.com/pgvector/pgvector#hnsw) |
| 7 | - Resolved minor bugs and exceptions encountered during pipeline integration. <br> - Drafted and finalized progress notes for Week 5. | 07/04/2026 | 07/04/2026 | N/A |

### Week 5 Achievements:

* Cleared major infrastructure hurdles: Successfully completed local testing and effectively leveraged AWS Foundation Models via the Amazon Bedrock API.

* Successfully provisioned and configured an Aurora Serverless database instance on AWS, activating the `pgvector` extension for secure vector storage.

* Migrated data processing code smoothly from local environments to AWS Lambda, optimizing the Vectorize pipeline for a Serverless architecture.