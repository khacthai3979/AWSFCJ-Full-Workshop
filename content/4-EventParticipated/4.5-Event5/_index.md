---
title: "Event Report – DevOps on AWS"
date: "2025-11-17"
chapter: false
weight: 5
pre: "<b>4.5.</b>"
---

# Event Report: “DevOps on AWS Workshop”

## Event Details

- **Event:** DevOps on AWS  
- **Date:** Monday, November 17, 2025  
- **Time:** 8:30 AM – 5:00 PM  
- **Location:** AWS Vietnam Office  

---

## Event Objectives

- Provide hands-on knowledge of DevOps practices on AWS  
- Guide participants through building an end-to-end **CI/CD Pipeline**  
- Introduce **Infrastructure as Code (IaC)** using CloudFormation & AWS CDK  
- Deploy containerized workloads using **ECR, ECS, EKS, and App Runner**  
- Implement **Monitoring & Observability** with CloudWatch and X-Ray  
- Share DevOps best practices and real-world case studies  

---

# Morning Session (8:30 AM – 12:00 PM)

## 8:30 – 9:00 AM | Welcome & DevOps Mindset

The workshop started with registration, introduction, and an overview session to connect the previous **AI/ML session** with today’s DevOps topic — showing how AI and DevOps are closely integrated in modern cloud-native systems.

### DevOps Culture & Principles

- DevOps is a combination of **People – Process – Tools**  
- Collaboration between Developers & Operations  
- Goal: **Deliver faster, safer, and more reliable systems**  
- Core mindset: **Automation, Continuous Improvement, Ownership**

### Key Metrics (DORA Metrics)

- **Deployment Frequency**  
- **Lead Time for Changes**  
- **Mean Time To Recovery (MTTR)**  
- **Change Failure Rate**

DORA Metrics are essential for measuring the **velocity and reliability** of modern software delivery.

---

## 9:00 – 10:30 AM | AWS DevOps Services – CI/CD Pipeline

This session covered the core DevOps services on AWS and demonstrated how to implement a full CI/CD pipeline.

### Source Control

- **AWS CodeCommit**  
- Git strategies comparison: **GitFlow vs Trunk-Based Development**  
- When to use release branches, hotfix, and feature branch workflows  

### Build & Test

- **AWS CodeBuild**
  - Buildspec file  
  - Unit test & integration testing  
  - Artifact caching & environment configuration  

### Deployment

Deployment strategies using **AWS CodeDeploy**:

- **Blue/Green Deployment**
  - Zero downtime  
  - Immediate rollback  

- **Canary Deployment**
  - Gradual rollout (e.g., 10% traffic)  
  - Metric-based validation  

- **Rolling Updates**
  - Replace instances in a controlled manner  

### Pipeline Orchestration

- **AWS CodePipeline**
  - Automates Source → Build → Test → Deploy  
  - Integrates with Lambda, SNS, CloudWatch Events  

### Live Demo: CI/CD Walkthrough

Hands-on demonstration showing:

- Commit to CodeCommit  
- Automatic trigger → CodeBuild  
- Build & test artifacts  
- Blue/Green deployment through CodeDeploy  
- Visualized pipeline in CodePipeline  

---

## 10:30 – 10:45 AM | Break

Informal networking with AWS Solutions Architects and fellow participants.

---

## 10:45 AM – 12:00 PM | Infrastructure as Code (IaC)

### AWS CloudFormation

- Templates & Stacks  
- Drift detection  
- Cross-stack references  
- StackSets for multi-account deployments  

### AWS CDK (Cloud Development Kit)

- CDK Constructs Levels 1/2/3  
- Reusable patterns  
- Multi-language support (TypeScript, Python)  
- Infrastructure deployment through code  

### Demo: Deploying IaC

- Deploying a VPC, ECS service using **CloudFormation**  
- Re-implementing using **AWS CDK** with reusable constructs  

### Discussion: Choosing IaC Tools

| CloudFormation | AWS CDK |
|----------------|---------|
| Declarative     | Imperative |
| YAML/JSON       | Programming language |
| Strong governance | Fast development |
| Stable          | Flexible & reusable |

**Conclusion:**  
- Use **CloudFormation** for governance & enterprise control  
- Use **CDK** for developer velocity & patterns  

---

# Lunch Break (12:00 – 1:00 PM)

Self-arranged lunch and networking.

---

# Afternoon Session (1:00 PM – 5:00 PM)

## 1:00 – 2:30 PM | Container Services on AWS

### Docker Fundamentals

- Microservices architecture  
- Layered container image patterns  
- Containerizing applications  

### Amazon ECR

- Secure image storage  
- Image scanning & vulnerability detection  
- Lifecycle policies for cost optimization  

### Amazon ECS vs EKS

**Amazon ECS**
- Fully managed by AWS  
- Simple operations  
- Fargate serverless support  

**Amazon EKS**
- Managed Kubernetes  
- High flexibility  
- Rich ecosystem tooling  

### AWS App Runner

- Simplified container deployment  
- No infrastructure management  
- Ideal for startups & internal business apps  

### Demo & Case Study

Live demo showing:

- Building Docker images  
- Pushing to ECR  
- Deploying on ECS/EKS  
- Auto scaling & service mesh examples  

---

## 2:30 – 2:45 PM | Break

---

## 2:45 – 4:00 PM | Monitoring & Observability

### AWS CloudWatch

- Metrics & Logs  
- Alarm-Based Monitoring  
- Log Insights Query  
- Dashboards for real-time visibility  

### AWS X-Ray

- Distributed tracing  
- Latency and bottleneck analysis  
- Service map visualization  

### Demo: Full-Stack Observability

- Instrument services for metrics and tracing  
- Create alarms based on SLOs  
- Visualize end-to-end request flow  

### Best Practices

- Dashboards per service/domain  
- Clear on-call process  
- Alert fatigue prevention  
- Post-incident reviews  

---

## 4:00 – 4:45 PM | DevOps Best Practices & Case Studies

### Deployment Strategies

- Feature flags  
- A/B testing  
- Progressive delivery  

### Automated Testing in CI/CD

- Unit testing  
- Integration testing  
- Canary checks  

### Incident Management

- Blameless postmortems  
- On-call rotation  
- Incident response playbooks  

### Case Studies

- Startup cloud-native transformations  
- Large enterprise modernization  

---

## 4:45 – 5:00 PM | Q&A & Wrap-up

### DevOps Career Pathways

- DevOps Engineer  
- Site Reliability Engineer (SRE)  
- Cloud Engineer  
- Platform Engineer  

### AWS Certification Roadmap

1. AWS Cloud Practitioner  
2. AWS Associate (DevOps, Developer, SysOps)  
3. AWS DevOps Professional  

---

# Key Takeaways

- DevOps is a **culture and mindset** before being a toolset  
- CI/CD increases **deployment frequency** and reduces MTTR  
- IaC ensures **repeatability and consistency**  
- Containers and observability are foundational to modern cloud-native platforms  
- DevOps + SRE drive **reliability and velocity**  

---

# Applications to Real Projects

- Build production-ready CI/CD using CodePipeline  
- Implement IaC with CDK for faster delivery  
- Deploy microservices on ECS/EKS  
- Operational monitoring using CloudWatch & X-Ray  



