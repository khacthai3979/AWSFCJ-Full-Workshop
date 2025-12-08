---
title: "Event Report – AWS Well-Architected Security Pillar"
date: "2025-11-29"
chapter: false
weight: 6
pre: "<b>4.6.</b>"
---

# Event Report: “AWS Well-Architected Security Pillar Workshop”

## Event Details
- **Topic:** AWS Well-Architected Security Pillar  
- **Date:** Saturday, November 29, 2025  
- **Time:** 08:30 AM – 12:00 PM  
- **Location:** AWS Vietnam Office  

---

## Event Objectives
- Understand the role of the **Security Pillar** in the AWS Well-Architected Framework  
- Explore modern security best practices based on AWS guidance  
- Implement security controls across the 5 security pillars  
- Apply practical defense strategies to protect workloads on AWS  
- Learn from real-world security use cases in Vietnam  

---

# 8:30 – 8:50 AM | Opening & Security Foundation

### Role of Security Pillar in the Well-Architected Framework
The session introduced the importance of the Security Pillar as one of the six pillars in the AWS Well-Architected Framework.  
Security is integrated across all stages of cloud architecture — from identity to data protection and incident response.

### Core Principles
- **Least Privilege**  
- **Zero Trust**  
- **Defense in Depth**  

These principles ensure that workloads remain secure even when individual layers fail.

### Shared Responsibility Model
- What AWS Secures: **Global infrastructure, virtualization layer, managed services**  
- What Customers Secure: **Identity, data, network configuration, OS, workloads**

This model helps clarify responsibility boundaries in cloud environments.

### Top Threats in Vietnam Cloud Landscape
- Misconfigured IAM access  
- Public data exposure on S3  
- Lack of logging and detection  
- Weak multi-account governance  
- Malware & compromised credentials  

---

# Pillar 1 — Identity & Access Management (8:50 – 9:30 AM)

### Modern IAM Architecture

- **IAM Users, Roles, Managed Policies**  
  - Avoid long-term credentials  
  - Use short-lived tokens & role assumption  

- **IAM Identity Center**
  - Centralized SSO with permission sets  
  - Simplified user lifecycle management  

- **SCP & Permission Boundaries**
  - Governance for multi-account environments  
  - Prevent privilege escalation  

- **Security Enhancements**
  - MFA enforcement  
  - Credential rotation  
  - IAM Access Analyzer  

### Mini Demo
- Validate IAM policy  
- Perform **access simulation** to prevent over-permissioned roles  

---

# Pillar 2 — Detection (9:30 – 9:55 AM)

### Detection & Continuous Monitoring

- **CloudTrail (Organization-level logging)**
  - Unified auditing across multiple accounts  

- **Amazon GuardDuty**
  - Threat detection & anomaly detection  

- **AWS Security Hub**
  - Aggregated security findings  
  - CIS, PCI compliance checks  

### Logging Across All Layers
- **VPC Flow Logs**  
- **ALB Logs**  
- **S3 Access Logs**  

### Alerting & Automation
- EventBridge rules  
- Automatic remediation workflows  

### Detection-as-Code
- Security detection codified  
- Rules deployed like infrastructure  

---

# 9:55 – 10:10 AM | Coffee Break

Break.

---

# Pillar 3 — Infrastructure Protection (10:10 – 10:40 AM)

### Network & Workload Security

- **VPC Segmentation**
  - Isolate workloads using subnets & routing  
  - Private vs Public placement strategies  

- **Security Groups vs NACLs**
  - Security Groups: Stateful, resource-level  
  - NACLs: Stateless, subnet-level  

### Protection Layers
- **AWS WAF**  
- **AWS Shield Advanced**  
- **AWS Network Firewall**  

### Workload-Level Protection
- EC2 security hardening  
- ECS/EKS cluster security basics  
- Runtime isolation strategies  

---

# Pillar 4 — Data Protection (10:40 – 11:10 AM)

### Encryption, Keys & Secrets

- **AWS KMS**
  - Key policies  
  - Key grants  
  - Automatic rotation  

- **Encryption At-Rest & In-Transit**
  - S3, EBS, RDS, DynamoDB  
  - TLS termination  

- **Secret Management**
  - **Secrets Manager**  
  - **SSM Parameter Store**  
  - Rotation strategies for secrets  

### Data Classification & Guardrails
- Identify sensitive data  
- Restrict data access using tags  
- Enforce access policies  

---

# Pillar 5 — Incident Response (11:10 – 11:40 AM)

### Incident Response Lifecycle (AWS Model)

1. **Prepare**  
2. **Detect & Analyze**  
3. **Contain**  
4. **Eradicate**  
5. **Recover**  
6. **Post-Incident Lessons Learned**  

### Incident Response Playbooks

- Compromised IAM key  
- Public S3 bucket exposure  
- Malware detection on EC2  
- Snapshot + isolation strategy  
- Evidence collection  

### Automation Techniques

- Lambda-based remediation  
- Step Functions orchestration  
- Automatic quarantine flows  

---

# 11:40 AM – 12:00 PM | Wrap-Up & Q&A

### Summary of 5 Pillars
- Identity & Access Management  
- Detection  
- Infrastructure Protection  
- Data Protection  
- Incident Response  

### Common Pitfalls in Vietnam
- Over-permissioned IAM roles  
- Lack of detection pipelines  
- Missing encryption enforcement  
- No centralized governance  

### Security Learning Roadmap

- **AWS Security Specialty**  
- **AWS Solutions Architect Professional**  

---

# Key Takeaways

- Security must be integrated from day one, not added later  
- Multi-account governance is essential for scale  
- Identity is the new perimeter  
- Logging and detection are the backbone of cloud security  
- Encryption & secret rotation should be automated  
- Incident response must be pre-planned with automation  

---

# Application to Real Projects

- Apply IAM best practices in multi-account environments  
- Build detection pipelines using CloudTrail + GuardDuty  
- Protect workloads using WAF & Shield Advanced  
- Automate secret rotation with KMS & Secrets Manager  
- Create IR playbooks and remediation automation  


