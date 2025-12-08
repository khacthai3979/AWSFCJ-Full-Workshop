---
title: "Event Report – DevOps on AWS"
date: "2025-11-17"
weight: 5
chapter: false
pre: "<b>4.5.</b>"
---

# Summary Report: “DevOps on AWS Workshop”

## Event Details

- **Event:** DevOps on AWS  
- **Date:** Monday, November 17, 2025  
- **Time:** 8:30 AM – 5:00 PM  
- **Location:** AWS Vietnam Office  

---

## Event Objectives

- Trang bị kiến thức thực hành về DevOps trên AWS  
- Hướng dẫn xây dựng **CI/CD Pipeline** từ source → build → deploy  
- Giới thiệu **Infrastructure as Code (IaC)** với CloudFormation và CDK  
- Triển khai container với **ECR, ECS, EKS và App Runner**  
- Thực hành **Monitoring & Observability** với CloudWatch và X-Ray  
- Chia sẻ DevOps best practices & case studies thực tế  

---

# Morning Session (8:30 AM – 12:00 PM)

## 8:30 – 9:00 AM | Welcome & DevOps Mindset

Buổi sáng bắt đầu với hoạt động chào mừng và tổng kết lại nội dung **AI/ML session** trước đó, kết nối giữa AI & DevOps trong vận hành hệ thống hiện đại.

Nội dung chia sẻ:

### DevOps Culture & Principles

- DevOps là sự kết hợp giữa **People – Process – Tools**  
- Collaboration giữa Developer & Operations  
- Mục tiêu: **Deliver faster, safer, and more reliable**  
- Tư duy: **Automation, Continuous Improvement, Ownership**

### Key Metrics (DORA Metrics)

- **Deployment Frequency**  
- **Lead Time for Changes**  
- **MTTR (Mean Time To Recovery)**  
- **Change Failure Rate**

Việc đo lường bằng DORA giúp doanh nghiệp đánh giá khả năng **delivery & reliability** của hệ thống.

---

## 9:00 – 10:30 AM | AWS DevOps Services – CI/CD Pipeline

Session tập trung vào kiến trúc DevOps trên AWS:

### Source Control

- **AWS CodeCommit**  
- So sánh chiến lược: **GitFlow vs Trunk-Based Development**  
- Khi nào dùng release branch, hotfix, feature branch  

### Build & Test

- **AWS CodeBuild**
  - Build spec  
  - Unit test, integration test  
  - Artifacts & caching  

### Deployment

Sử dụng **AWS CodeDeploy** cho nhiều chiến lược deployment:

- **Blue/Green Deployment**
  - Không downtime  
  - Rollback nhanh  

- **Canary Deployment**
  - Release cho 10% traffic  
  - Theo dõi metrics  

- **Rolling Update**
  - Thay thế từng phần  

### Pipeline Orchestration

- **AWS CodePipeline**
  - Tự động hóa toàn bộ flow: Source → Build → Test → Deploy  
  - Integration với Lambda, CloudWatch, SNS  

### Live Demo: CI/CD Walkthrough

Demo thực tế:

- Push code → CodeCommit  
- Trigger → CodeBuild test  
- Build artifact → CodeDeploy  
- Deploy Blue/Green đến ECS  

---

## 10:30 – 10:45 AM | Break

Networking cùng AWS Solution Architects và các dev tham gia event.

---

## 10:45 AM – 12:00 PM | Infrastructure as Code (IaC)

### AWS CloudFormation

- Templates & Stacks  
- Stack drift detection  
- Cross-stack reference  
- StackSets cho multi-account  

### AWS CDK (Cloud Development Kit)

- CDK **Constructs 1/2/3**  
- Reusable patterns  
- Multi-language support (TypeScript, Python…)  
- Dev pipelines dùng CDK  

### Demo: Deploying IaC

- Deploy stack bằng **YAML CloudFormation**  
- Tạo VPC, EC2, ECS bằng **CDK**  

### Discussion: Choosing IaC Tools

| CloudFormation | CDK |
|----------------|-----|
| YAML/JSON      | Code (TS/Python) |
| Static         | Dynamic |
| Enterprise-ready | Fast development |

**Kết luận:**  
- CloudFormation tốt cho **governance**  
- CDK tốt cho **developer productivity**  

---

# Lunch Break (12:00 – 1:00 PM)

- Tự sắp xếp bữa trưa và networking nhóm nhỏ.

---

# Afternoon Session (1:00 – 5:00 PM)

## 1:00 – 2:30 PM | Container Services on AWS

### Docker Fundamentals

- Microservices architecture  
- Container packaging  
- Context & layers  

### Amazon ECR

- Image storage  
- Image scanning & security  
- Lifecycle policy tối ưu chi phí  

### Amazon ECS vs EKS

**ECS:**
- Native AWS  
- Easier to operate  
- Fargate serverless mode  

**EKS:**
- Managed Kubernetes  
- High flexibility  
- Compatible with K8s ecosystem  

### AWS App Runner

- Tự động deploy container  
- Không cần quản lý infrastructure  
- Ideal cho startup, internal apps  

### Demo & Case Study

Triển khai microservices:

- Container build → ECR  
- Deploy ECS/EKS  
- Auto scaling & service mesh  

---

## 2:30 – 2:45 PM | Break

---

## 2:45 – 4:00 PM | Monitoring & Observability

### AWS CloudWatch

- Metrics  
- Logs  
- Alarms  
- Dashboards  
- Log Insights Query  

### AWS X-Ray

- Distributed tracing  
- Latency analysis  
- Bottleneck detection  
- Performance insights  

### Demo: Full-Stack Observability Setup

- Instrumentation  
- Tracing requests end-to-end  
- Alert on MTTR  

### Best Practices

- Dashboard theo service  
- On-call process  
- Alerting: SLO/SLA/SLI  

---

## 4:00 – 4:45 PM | DevOps Best Practices & Case Studies

### Deployment Strategies

- Feature Flags  
- A/B Testing  
- Progressive delivery  

### Automated Testing in CI/CD

- Unit tests  
- Integration tests  
- Canary checks  

### Incident Management

- On-call rotation  
- Postmortem  
- Blameless culture  

### Case Studies

- Startup DevOps transformation  
- Enterprise modernization  

---

## 4:45 – 5:00 PM | Q&A & Wrap-up

### DevOps Career Pathways

- DevOps Engineer  
- Site Reliability Engineering (SRE)  
- Cloud Engineer  
- Platform Engineer  

### AWS Certification Roadmap

1. AWS Cloud Practitioner  
2. AWS Associate (DevOps, Developer, SysOps)  
3. AWS DevOps Professional  

---

# Key Takeaways

- DevOps là **văn hóa** trước khi là công cụ  
- Automation giúp tăng **deployment frequency**  
- CI/CD giúp **giảm MTTR**  
- IaC giúp cải thiện **consistency & traceability**  
- Containers + Observability là nền tảng **cloud-native**  
- SRE & DevOps thúc đẩy **reliability** và **velocity**  

---

# Application to Real Projects

- Triển khai CI/CD dùng CodePipeline  
- Viết IaC bằng CDK  
- Triển khai microservices trên ECS/EKS  
- Theo dõi end-to-end bằng CloudWatch + X-Ray  



