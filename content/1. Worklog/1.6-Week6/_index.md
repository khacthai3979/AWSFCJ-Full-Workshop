---
title: "WEEK 6 WORKLOG"
date: "2025-11-10"
weight: 6
chapter: false
pre: " <b> 1.6 </b> "
---

# **WEEK 6 WORKLOG**

### **Week 6 Objectives**

* Understand deeply about **Infrastructure as Code (IaC)** by using **AWS CloudFormation**.
* Master YAML syntax and structure of a CloudFormation Template (including \Parameters\, \Resources\, \Outputs\).
* Write templates to automate creation of individual resources (S3 Bucket) and complex network infrastructure (VPC, Subnet, IGW, Route Table).
* Expand templates to automatically deploy complete web server (EC2, Security Group) into the created network infrastructure.
* Master stack lifecycle management process (create, update, delete) using AWS Console and AWS CLI.

---

### **Tasks to be carried out this week**

| Day | Task | Start Date | Completion Date | Reference/Material |
| :--- | :--- | :--- | :--- | :--- |
| 1 (Monday) | **Understand CloudFormation & YAML**: Research sections (Parameters, Resources, Outputs) and YAML syntax. Write simple template creating S3 Bucket. | 13/10/2025 | 13/10/2025 | |
| 2 (Tuesday) | **Deploy Stack & Parameters**: Deploy S3 stack. Add \Parameters\ (example: customize Bucket name) to template and learn how to update stack. | 14/10/2025 | 14/10/2025 | |
| 3 (Wednesday) | **Write Network template**: Write new template creating network infrastructure (VPC, Public Subnet, IGW, Route Table). Use \Outputs\ to display IDs. | 15/10/2025 | 15/10/2025 | |
| 4 (Thursday) | **Write EC2 template**: Expand network template, add \Resources\ for Security Group (SSH, HTTP) and EC2 instance (specify AMI, Type). | 16/10/2025 | 16/10/2025 | |
| 5 (Friday) | **Deploy & Clean up**: Deploy complete stack (VPC + Subnet + EC2). Check SSH/HTTP. Learn how to delete stack (\ws cloudformation delete-stack\). | 17/10/2025 | 17/10/2025 | |

---

### **Week 6 Achievements**

* Master YAML syntax and structure of **AWS CloudFormation** template (including \Parameters\, \Resources\, \Outputs\).
* Successfully write CloudFormation template to automate creation of individual resources (like **S3 Bucket**).
* Skillfully use \Parameters\ to customize resources when deploying (example: S3 Bucket name, AMI ID, Instance Type), helping increase template flexibility and reusability.
* Successfully write complex template connecting multiple resources to build complete network infrastructure, including:
  * **VPC** and **Public Subnet**.
  * **Internet Gateway (IGW)** and **Route Table** (with associations).
* Expand template to automatically deploy complete web server, including:
  * **Security Group** (allow SSH port 22 and HTTP port 80).
  * **EC2 Instance** (use \!Ref\ to link with created Subnet and Security Group).
* Master stack lifecycle management: deploy (\create-stack\), update (\update-stack\) and delete (\delete-stack\) using both AWS Console and **AWS CLI**.
* Fix common CloudFormation errors, such as YAML syntax errors (indentation), S3 Bucket names (globally unique), AMI ID (by region), and errors when deleting stack (due to dependent resources).

---

**Summary:**
Week 6 is an important phase in learning Infrastructure as Code using AWS CloudFormation. By mastering template writing and stack management, students gain ability to automate infrastructure deployment and prepare for advanced topics like Terraform, CI/CD, and managing complex cloud architectures.
