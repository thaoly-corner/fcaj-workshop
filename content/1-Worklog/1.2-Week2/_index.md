---
title: "Worklog - Week 2"
date: 2026-06-08
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives:
* Understand core AWS networking architectures (VPC, Subnet, Route Table, Internet Gateway, NAT Gateway).
* Master network security concepts through Security Groups and Network ACLs.
* Complete Lab 03: Hands-on provisioning of a custom Virtual Private Cloud (VPC) and deploying virtual servers (EC2).
* **Advanced:** Provision a Relational Database service (Amazon RDS/Aurora) within the private network and establish a secure connection from EC2 to the Database (2-tier architecture).

### Tasks Completed During the Week:
| Day | Task | Start Date | Completion Date | Reference Material |
| :---: | :--- | :---: | :---: | :--- |
| 2 | - Research theoretical concepts of AWS VPC and VPC Security. <br> - Lab 03 Practice: Subnets, Route tables, and IGW concepts. | 06/08/2026 | 06/08/2026 | [First Cloud Journey Bootcamp - 2025](https://youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i) |
| 3 | - Configure NAT Gateway, Security Groups, and Network ACLs. <br> - Provision VPC, Subnets, and IGW via AWS Console. | 06/09/2026 | 06/09/2026 | [First Cloud Journey Bootcamp - 2025](https://youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i) |
| 4 | - Configure Route Tables, Security Groups, and launch EC2 Instances within the created Subnets. <br> - Test connectivity using EC2 Instance Connect. | 06/10/2026 | 06/10/2026 | [First Cloud Journey Bootcamp - 2025](https://youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i) |
| 5 | - Explore Amazon RDS/Aurora PostgreSQL. <br> - **Hands-on:** Create a DB Subnet Group and provision an RDS Instance inside a Private Subnet. | 06/11/2026 | 06/11/2026 | [Amazon RDS User Guide](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html) |
| 6 | - **Hands-on:** Fine-tune Inbound Rules of the Database Security Group to allow only EC2 access on port 5432. <br> - SSH into EC2 and test database connectivity (`psql`) to RDS. | 06/12/2026 | 06/12/2026 | [AWS VPC Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) |

### Week 2 Achievements:

* Successfully designed and built a custom cloud networking environment (VPC) featuring Public Subnets, Private Subnets, IGW, and NAT Gateway.

* Deployed EC2 virtual servers and gained a solid understanding of managing outbound and inbound traffic routing securely.

* Mastered and implemented a standard 2-tier architecture: Positioning the Web/App Server (EC2) in the outer tier and the Database (RDS) in an isolated inner tier (Private Subnet).

* Applied Security Groups effectively to establish firewall rules, completely isolating the database from the Internet and granting access exclusively to internal EC2 instances.