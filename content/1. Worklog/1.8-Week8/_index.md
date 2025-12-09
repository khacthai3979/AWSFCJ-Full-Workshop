---
title: "WEEK 8 WORKLOG"
date: "2025-12-29"
weight: 8
chapter: false
pre: " <b> 1.8 </b> "
---

# **WEEK 8 WORKLOG**

### **Week 8 Objectives**

* Research and build system architecture design for the project.
* Consult with mentor and adjust architecture design version.
* Build project implementation plan draft (project plan & timeline).
* Learn Terraform to deploy infrastructure as source code (Infrastructure as Code).
* Learn Docker and containerization to package applications.
* Practice deploying applications to Amazon Elastic Container Service (Amazon ECS).

---

### **Tasks to be carried out this week**

| Day | Task | Start Date | Completion Date | Reference/Material |
| :--- | :--- | :--- | :--- | :--- |
| 1 (Monday) | **Research & plan architecture**: Determine overall architecture, select appropriate AWS services for the project | 27/10/2025 | 27/10/2025 | AWS Architecture Docs |
| 2 (Tuesday) | **Consult and revise architecture design**: Discuss with mentor, receive feedback and adjust design version | 28/10/2025 | 28/10/2025 | Mentor |
| 3 (Wednesday) | **Draft project plan & timeline**: Build project implementation timeline by phases and allocate execution time | 29/10/2025 | 29/10/2025 | Project Notes |
| 4 (Thursday) | **Learn Terraform & deploy with Docker**: Learn IaC concepts, create simple terraform module, create Dockerfile & build image | 30/10/2025 | 30/10/2025 | Terraform Docs |
| 5 (Friday) | **Deploy application to Amazon ECS**: Create ECS Cluster, Task Definition, Service, check container running on ECS | 31/10/2025 | 31/10/2025 | AWS ECS Docs |

---

### **Week 8 Achievements**

Week 8 marks an important project phase, transitioning from research to architecture design and preparing for real implementation. Achievements include:

#### 1. Complete system architecture design

* Determine suitable architecture with project:
  * AWS VPC
  * Amazon ECS (Fargate)
  * Amazon RDS
  * Amazon Cognito
  * AWS Bedrock
  * CloudFront + S3
  * CloudWatch Logs & Metrics
* Design architecture toward **cloud-native, secure and scalable**
* Select appropriate services for project objectives and current budget

#### 2. Consult mentor & adjust design

* Present initial architecture design version
* Receive feedback on:
  * Best practices on AWS
  * Security layers
  * Networking (VPC Endpoints, ALB, Security Groups)
  * Cost optimization
* Revise architecture based on feedback

#### 3. Draft project plan & timeline

* Identify main phases:
  * Research
  * Architecture Design
  * Frontend/Backend Development
* Build weekly timeline according to milestones
* Define deliverables for each phase
* Plan testing, deploy & optimization

#### 4. Learn Terraform & Docker

* Understand IaC concepts and benefits:
  * Reproducible infrastructure
  * Version control
  * Automation
* Write simple terraform module:
  * VPC
  * Subnet
  * Security Group
* Learn Docker:
  * Create Dockerfile
  * Build image & run container
  * Understand concepts of Layer, Registry

#### 5. Deploy application to ECS

* Create ECS Cluster (Fargate)
* Create Task Definition & configure container
* Deploy demo Flask or Node.js application
* Check Container Logs on CloudWatch
* Understand **serverless containers** mechanism

---

**Summary:**
Week 8 is the phase transitioning from research to **architecture design and experimental deployment**, including creating project plan, finalizing architecture, and practicing with real deployment technologies like Terraform, Docker and Amazon ECS. This is an important foundation to enter application development and complete infrastructure deployment phases in following weeks.
