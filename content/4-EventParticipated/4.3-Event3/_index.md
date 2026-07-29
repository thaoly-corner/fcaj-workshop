---
title: "Agentic AI Build Week Hackathon"
date: 2026-07-25
weight: 1
chapter: false
pre: " <b> 4.3. </b> "
---

# Report: Agent AI Build Week Hackathon – Innovative AI Solutions & Team Experiences

### Event Overview

**Agent AI Build Week Hackathon** is an explosive technology playground focusing on actualizing practical Artificial Intelligence (AI) applications. The event spans multiple practical fields: from optimizing commercial service experiences, enterprise strategy analysis, crowd security monitoring, to automated financial compliance. 

Through insights shared from 4 projects, the event distilled core technological values:
* **Mindset Innovation:** Capturing today's rapid software development speed, large-scale automation, and changing the mindset of solving industry problems.
* **Practical Modeling:** A seamless combination of Generative AI, Sub-agents, Computer Vision (YOLO), and Serverless architecture on AWS.
* **Human-in-the-Loop & Cost Optimization:** Maintaining human supervision in AI workflows and mastering the challenge of optimizing Cloud infrastructure costs.


### 1. Outstanding AI Solutions at the Hackathon

#### AI-Powered Conversational Food Ordering Agent - One Team
The team thoroughly resolved the barriers of traditional food ordering—where customers often lose patience due to having to download apps, create accounts, and browse complex menus. At the same time, this solution completely fixes the "hallucination" errors that caused AI to order wrong items, which major chains like McDonald's have faced.
* **Barrier-free Multi-channel Solution:** Deployed AI Chatbots directly on familiar platforms like Zalo/WhatsApp. Customers can order food using natural language.
* **Agent Core Architecture & Decision Intelligence (DI):** Utilized a centralized AI Engine to remember conversation context (session memory) and order history. The DI cluster automatically plans the next steps, applies discount codes, and **mandates a confirmation step** to prevent incorrect orders or unintended large quantities.
* **Demo:** Management dashboard for error tracing...
* **Overcoming API Limits (Tiny Fish):** Collected real-time practical menu data from KFC's website to handle the lack of an official API.
* **Cost Optimization:** By using Agent Core's memory instead of continuously calling traditional Lambda APIs, the team reduced costs by 60%. Operating costs are impressively at **~$0.006/order** (approx. $88/month for infrastructure) with a latency of only 3–4 seconds.

#### AI-Driven Business Strategy Analyzer - Signal Scout
The project focuses on the "pain point" of businesses when analyzing competitor strategies through scattered public data sources (financial reports, organizational structures, news).
* **Topic Selection Philosophy:** Focused on application and business operations. Understanding the market clearly: "What problem does your software solve?", "Who is the target user?". From there, choose the appropriate topic and approach.
* **Serverless Architecture & Sub-agents:** Utilizes AWS Lambda and Agent Core to coordinate tasks. *Sub-agents* handle specialized tasks: web scraping, noise filtering, and structured storage in S3/DynamoDB.
* **Cost Optimization:** Uses AWS services to replace TinyFish and Amplify to optimize costs.
![Architecture model image of the AI-Driven Business Strategy Analyzer project](images/4-Event/ss.png)

#### Solution Architect Professional Native App - Plan V

* **Background & Practical Pain Points:** Customers or superiors frequently demand system architecture designs and cost estimates extremely urgently within 2-3 days, sometimes even making sudden phone calls at night demanding them immediately. Solution Architects (SA) have to start from a blank page, manually drawing each icon on Draw.io, writing infrastructure-as-code (IaC) manually, and spending a lot of time parsing complex business requirements. Popular tools like ChatGPT or Gemini often produce messy icons, disjointed arrows, and fail to comply with internal corporate technical standards when drawing diagrams.
* **Natural Language & Document Processing:** Users can input requirements using natural language (free text) or upload internal company documents and policies.
* **Agent Architecture & High Engineering:** Focuses on optimizing high-level engineering for memory management, workflow, and real-time context, displaying a step-by-step streaming report for users to track the Agent's progress.
* **Automatic Diagram Generation on Draw.io:** Completely automates the creation of intuitive architectural diagrams with precise layouts on Draw.io, allowing users to freely adjust manually as desired.
* **Automatic Pricing & IaC Generation:** Automatically calculates cost estimates and exports Terraform source code complying with standard Terraform module usage to optimize reusability.
* **Auto-deployment:** Capable of automatically running deployment code to provision the entire drawn infrastructure on AWS if the enterprise has urgent needs.
##### System Workflow
* Users access via a web interface and send requests (free text or attached documents).
* Requests are forwarded to backend Sub-agents for analysis and cross-referencing with enterprise policies.
* The system proceeds to draw diagrams on Draw.io, aggregate real-time reports, generate pricing tables, and create IaC code (Terraform).
* Stores related databases and returns the complete result to the user.
![Architecture model image of the Solution Architect Professional Native App project](images/4-Event/PlanV.png)

#### Real-Time Crowd Flow Monitoring System - 3KA
The system solves congestion problems in crowded areas (airports, supermarkets), helping minimize security risks and customer frustration.
* **Real-time Processing & AI Agent:** Utilizes the **YOLO** model to detect and track movement flows via video livestreams (combining WebSocket and AWS Fargate). AI Agents operate autonomously to monitor and send notifications directly to operational staff.
* **Zone-wise Tracking:** Flexibly defines monitoring zones. The system automatically counts people and displays intuitive color-coded warning levels on the Dashboard.
* **Technical Lessons:** Despite encountering some networking limitations during the demo, the project successfully proved the feasibility of integrating Deep AI with Cloud services for automation while retaining human decision-making power.
![Architecture model image of the Real-Time Crowd Flow Monitoring System project](images/4-Event/3KA.png)

#### Adaptive AML Workflow Engine - Six Pillars Team
The project tackles Anti-Money Laundering (AML). In reality, 90–95% of current transaction alerts are false-positives, causing analysts to waste excessive time and easily burn out.
* **Automated Triage Process:** AI Agents handle KYC lookups, transaction analysis, and automatically aggregate evidence dossiers.
* **Impressive Performance:** Shortens the multi-step processing time from **~3 hours/case** down to a neat summary report for specialists to approve at the final step.
* **Auditability:** The project's standout feature lies in its detailed logging system, allowing full traceability of AI origins and reasoning, meeting the strict transparency requirements of financial institutions.
![Architecture model image of the Adaptive AML Workflow Engine project](images/4-Event/aml.png)

---

### 3. Lessons Learned & Core Mindset Through the Hackathon

The event concluded with valuable takeaways from the teams:
* **Scope Management:** In a short timeframe, defining a compact enough problem and completing a smoothly running Demo is far more important than sketching out a massive yet error-ridden architecture. Feasibility and value communication are the key factors to persuade the jury.
* **Human-in-the-Loop Mindset:** AI excels at automation and acceleration (Copilot), but human judgment and expertise remain the ultimate safety barrier and cannot be completely replaced.
* **Value of Iterative Feedback:** Multi-dimensional feedback from the jury—including technical, business, and user experience (UX) perspectives—is the catalyst to sharpen the product.

---
#### Some photos from attending the event
![Group photo attending the event](images/4-Event/team.png)
> **In summary**, the sharing sessions during the event not only provided an explosive, thrilling technology playground brimming with creative inspiration, but also offered sharp, practical perspectives on the art of packaging a complete AI product. Through this, I not only firmly consolidated my foundation in Serverless architecture, Sub-agents, and cloud cost optimization art, but also clearly defined a roadmap to elevate my own capabilities. This is undoubtedly a fantastic launching pad, helping me ready to face and master new technology waves changing day by day.
---