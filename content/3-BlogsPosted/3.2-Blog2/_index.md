---
title: "Blog 2: Cost Optimization with SQS"
date: 2026-07-04
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

A crucial criterion when designing systems on Cloud platforms is **Cost Optimization**. In the first version (v1) developed in the local environment, the NewsRAG project used **Apache Kafka** as the message broker to route data from the Crawler to the Database. However, upon migrating the system to AWS, the cost issue quickly became a major barrier.

### The Cost Problem with Kafka
Apache Kafka is an incredibly powerful stream processing tool. But to deploy Kafka on AWS, the team faced two costly options:
1. **Amazon MSK (Managed Streaming for Apache Kafka):** A fully managed service but highly expensive, potentially costing tens or hundreds of USD per month.
2. **Self-hosting Kafka on EC2:** Requires maintaining EC2 servers running 24/7, incurring fixed costs (at least ~$10-15/month) plus the effort of system administration, security updates, and maintaining the ZooKeeper cluster.

Meanwhile, the NewsRAG data pipeline is characterized by **Batch processing**: The Crawler only runs 1-2 times a day at night, pushing about 500 - 700 new news articles. Maintaining a Kafka cluster running continuously 24/7 just to serve such bursty traffic is extremely wasteful for the project's budget.

### The Alternative: Amazon SQS (Simple Queue Service)
After analyzing the architectural trade-offs, the team decided to replace the entire queue architecture with **Amazon SQS**. This is considered the perfect choice for the project due to 3 core reasons:

* **Fully Serverless & Pay-as-you-go:** SQS requires no server management and charges solely based on the number of requests. With the system's daily news volume, the number of API requests easily falls within the **Free Tier** (the first 1 million requests per month are free). As a result, the cost for the queue component is reduced to **$0**.
* **Native integration with AWS Lambda:** SQS acts as a perfect "Event Source" to automatically trigger Lambda functions (Lambda Consumer). When the Crawler pushes articles into SQS, SQS automatically invokes Lambda to parse and save them to the Database, without the team having to write complex polling loop code.
* **Dead-Letter Queue (DLQ) mechanism:** SQS provides built-in DLQs. If an article has a formatting error and Lambda cannot save it to the database after multiple retries, that message is automatically pushed to the DLQ. This helps us easily track and analyze errors later, ensuring no piece of data is lost.

### Results Achieved
By boldly removing Kafka and transitioning to an SQS combined with Lambda architecture, the system not only became lighter and easier to maintain but also saved a substantial operational budget.

> **Key Takeaway:**  
> In system building, choosing technology is not about picking the "fanciest" or "most complex" one, but selecting the tool that is **"most suitable"** for the current scale, business problem, and budget.

---
**References:**
1. [Amazon SQS pricing & Free Tier - AWS Documentation](https://aws.amazon.com/sqs/pricing/)
2. [Using AWS Lambda with Amazon SQS](https://docs.aws.amazon.com/lambda/latest/dg/with-sqs.html)
3. [Amazon SQS Dead-Letter Queues (DLQ)](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html)