---
title: "Worklog - Week 4"
date: 2026-06-22
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:
* Begin migrating the RAG architecture from local to the AWS Serverless environment.
* Finalize Amazon Bedrock API authentication (`amazon.titan-embed-text-v2:0`) in preparation for pipeline integration.
* Pre-configure core modules (Database, network settings) to integrate immediately once the Amazon Bedrock API is unblocked.

### Tasks Completed During the Week:
| Day | Task | Start Date | Completion Date | Reference Material |
| :---: | :--- | :---: | :---: | :--- |
| 2 | - Initialized Lambda and wrote test scripts to invoke Titan Bedrock. <br> - **Issue:** Encountered AccessDenied/Model Access Exception. <br> - **Action:** Identified root cause (Foundation Model limits) and opened an AWS Support Ticket immediately. | 06/22/2026 | 06/22/2026 | [AWS Support Center](https://docs.aws.amazon.com/awssupport/latest/user/getting-started.html) |
| 3 | - While waiting for a response, experimented with various local fixes (switching regions, checking IAM Roles, adding `bedrock:*` policies, verifying billing) but remained blocked by AWS system controls. | 06/23/2026 | 06/23/2026 | [AWS IAM Documentation](https://docs.aws.amazon.com/iam/) |
| 4 | - Received initial response from Support Team requesting additional details. Communicated back and forth, providing internship use-case details to prove safety compliance. <br> - Used the downtime to draft Aurora connection logic via `psycopg2`. | 06/24/2026 | 06/24/2026 | N/A |
| 5 | - Continued waiting for AWS specialized teams to verify the account. <br> - Drafted the `retrieve()` function utilizing the HNSW algorithm to prepare for vector search workflows. | 06/25/2026 | 06/25/2026 | [pgvector HNSW index tuning](https://github.com/pgvector/pgvector#hnsw) |
| 6 | - Ticket remained in "Pending AWS" status. <br> - Reviewed all VPC and Security Group configurations for Lambda. | 06/26/2026 | 06/26/2026 | N/A |
| 7 | - Ticket status unchanged. <br> - Researched Amazon API Gateway documentation (Lambda Proxy Integration) in preparation for next week. | 06/27/2026 | 06/27/2026 | [API Gateway Docs](https://docs.aws.amazon.com/apigateway/) |

### Week 4 Achievements:

* Gained real-world cloud engineering experience: Navigating strict risk management and account verification policies within the AWS ecosystem.

* Enhanced persistence and communication skills when dealing with AWS Support throughout the week: Responding rapidly and providing clear, continuous English descriptions to track support tickets closely.

* Optimized blocked time by pre-configuring database infrastructure code (Aurora) and researching API Gateway in advance, preventing any project downtime.