---
title: "WEEK 4 WORKLOG"
date: "2025-12-01"
weight: 4
chapter: false
pre: " <b> 1.4 </b> "
---

# **WEEK 4 WORKLOG**

### **Week 4 Objectives**

* Understand and practice managing AWS resources with Tags and Resource Groups.
* Practice managing access to EC2 resources by Tag with AWS IAM.
* Get familiar with and use AWS CLI on Amazon EC2 (Windows/Ubuntu).
* Deploy Wordpress application in Highly Available model on AWS Cloud.
* Learn how to automatically deploy infrastructure with AWS CloudFormation.
* Self-research additional new knowledge related to AWS and Cloud.

---

### **Tasks to be carried out this week**

| Day | Task | Start Date | Completion Date | Reference/Material |
| :--- | :--- | :--- | :--- | :--- |
| 1 (Monday) | **Manage resources with Tags and Resource Groups**: Tag EC2 resources, create Resource Group, apply tagging best practices and manage access by Tag | 29/09/2025 | 29/09/2025 | AWS Tagging Docs |
| 2 (Tuesday) | **Manage EC2 access by Resource Tag with IAM**: Create IAM Policy based on Tag, practice limiting resource access based on Tag | 30/09/2025 | 30/09/2025 | AWS IAM Docs |
| 3 (Wednesday) | **Use AWS CLI on EC2**: Install and configure AWS CLI on EC2 (Windows/Ubuntu), run CLI commands to manage AWS resources | 01/10/2025 | 01/10/2025 | AWS CLI Docs |
| 4 (Thursday) | **Deploy Highly Available Wordpress**: Workshop deploying Wordpress on AWS in HA model: EC2 + RDS + ALB + Auto Scaling + S3 | 02/10/2025 | 02/10/2025 | AWS Workshop |
| 5 (Friday) | **Initialize infrastructure as Code with CloudFormation**: Create CloudFormation Stack, automatically deploy resources and understand template JSON/YAML | 03/10/2025 | 03/10/2025 | AWS CloudFormation Docs |
| 6 (Saturday) | **Self-learn additional knowledge**: Self-research cloud architecture, CI/CD, monitoring, Terraform, and advanced AWS services | 04/10/2025 | 04/10/2025 | AWS Whitepapers |

---

### **Week 4 Achievements**

In week 4, objectives about resource management, automation and real-world application deployment were completed as planned. Achievements include:

#### 1. Manage resources efficiently with Tags and Resource Groups

* Clearly understand the meaning and benefits of Tags in resource management
* Tag EC2, S3 and VPC resources
* Create Resource Group by Tag for centralized management
* Apply best practices:
  * \Environment\, \Owner\, \Project\, \CostCenter\
* Understand how to grant access permissions by Tag to protect sensitive resources

#### 2. Manage EC2 access with AWS IAM

* Write IAM Policy based on Resource Tag
* Practice limiting EC2 access by Tag
* Clearly understand IAM condition mechanism (\Condition\, \ws:ResourceTag\)
* Master access control model based on Least Privilege principle

#### 3. Work with AWS CLI on EC2

* Install and configure AWS CLI on EC2 (Windows & Ubuntu)
* Use CLI to:
  * Create S3 bucket
  * List EC2 instances
  * Describe VPC configuration
  * Create Key Pair
* Understand CLI benefits in automation and DevOps

#### 4. Deploy Wordpress in Highly Available model

* Complete workshop deploying Wordpress on AWS Cloud:
  * EC2 Instances running web application
  * RDS as database backend
  * Application Load Balancer for load distribution
  * Auto Scaling Group for automatic scaling
  * S3 for storing static data
* Understand overall architecture of real-world HA application

#### 5. Automate infrastructure with CloudFormation

* Create CloudFormation Stack for automatic resource deployment
* Understand Template format (YAML/JSON)
* Create and manage Stack lifecycle:
  * Create → Update → Delete
* Practice deploying sample templates:
  * VPC
  * EC2 instance
  * Security Group

#### 6. Self-learn additional new knowledge

* Learn more about:
  * CI/CD pipeline model
  * Terraform Infrastructure as Code
  * Cloud Monitoring (X-Ray, Log Insights)
  * Microservices Architecture
* Prepare knowledge for next weeks (DevOps & Serverless)

---

**Summary:**
Week 4 focuses on **resource management capabilities**, **infrastructure automation**, and deploying real-world applications in **Highly Available model**. Practicing CloudFormation, IAM by Tag and Wordpress workshop helps understand cloud architecture more deeply and prepare for the next phase: deploying systems in DevOps and IaC direction.
