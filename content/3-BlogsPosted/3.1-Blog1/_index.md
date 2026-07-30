---
title: "Blog 1: Overcoming Timeout Limits with AWS Fargate"
date: 2026-06-25
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

During the construction of the NewsRAG Pipeline system, one of the biggest technical challenges the team faced was crawling news data from major online newspapers such as VnExpress, Thanh Nien, and VietnamNet. Initially, the system was designed to run entirely on AWS Lambda. 

### The Problem: AWS Lambda Timeout Limits
AWS Lambda is an excellent service for running code without managing servers (serverless). However, this service has a hard limit: the maximum execution time is only **15 minutes (900 seconds)**. 

When the crawler scans the sitemaps of these newspapers, the number of articles to process often reaches thousands per day. Accessing, downloading HTML content, parsing data, and pushing it to the queue takes a lot of time because the system must always respect the page load speed and the source server's rules to avoid IP blocking (Rate Limiting). Consequently, the Lambda functions frequently timed out midway before they could complete their work loop.

### The Solution: Transitioning to Amazon ECS Fargate
To thoroughly resolve this problem while maintaining the "Serverless" spirit (not having to maintain EC2 servers running 24/7), we decided to restructure the application to use **Amazon ECS Fargate**.
{{< event-image src="images/3-Blogs/fargate.png" alt=" " >}}

The core advantages of applying Fargate to the project include:
* **No execution time limit:** Tasks on Fargate can run as long as needed until the data crawling is complete, without the fear of being interrupted midway.
* **Flexible packaging with Docker:** The team used the **Scrapy framework** for data parsing. The entire source code and dependencies were neatly packaged into a Docker Image and securely stored on **Amazon ECR (Elastic Container Registry)**.
* **Automated Scheduled Tasks:** Combined with Amazon EventBridge, the team configured the Crawler to automatically trigger during off-peak hours: **01:00 and 02:00 AM daily**.
* **Resource optimization:** When scaling is needed, the team simply adjusts the hardware parameters. At the current stage, the crawler operates highly efficiently with a minimal configuration of **0.25 vCPU and 0.5 GB RAM**.

### Results Achieved
The shift to the ECS Fargate architecture brought absolute stability to the data collection flow (Ingestion Pipeline). Currently, the Crawler can run continuously and resiliently for about **30-40 minutes** every night to collect hundreds of the latest articles without encountering any disruption errors. 

Moreover, the cost issue was also completely resolved because Fargate operates on a Pay-as-you-go model; the team only pays for the exact compute minutes that the Crawler actually runs.

> **💡 Key Takeaway:**  
> In cloud architecture design, there is no single AWS service that acts as a "silver bullet." Understanding the limits of each service like Lambda, and thereby flexibly transitioning to more suitable services like ECS Fargate, is a crucial skill to ensure the system's sustainability and scalability.

---
**References:**
1. [AWS Lambda limits - AWS Documentation](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html)
2. [AWS Fargate overview - Serverless compute for containers](https://aws.amazon.com/fargate/)
3. [Scrapy Framework - Web Crawling & Scraping](https://scrapy.org/)
4. [AWS Fargate or AWS Lambda?](https://docs.aws.amazon.com/decision-guides/latest/fargate-or-lambda/fargate-or-lambda.html)
5. [Task Networking in AWS Fargate](https://aws.amazon.com/blogs/compute/task-networking-in-aws-fargate/)