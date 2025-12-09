---
title: "WEEK 10 WORKLOG"
date: "2025-12-09"
weight: 10
chapter: false
pre: " <b> 1.10 </b> "
---

# **WEEK 10 WORKLOG**

### **Week 10 Objectives**
* Triển khai hạ tầng AWS bằng Terraform.
* Deploy frontend và backend lên AWS.
* Thiết lập auto-scaling, logging và CDN.
* Kiểm thử authentication end-to-end.

---

### **Tasks to be carried out this week**

| Day | Task | Start Date | Completion Date |
| :--- | :--- | :--- | :--- |
| 1 (Thứ Hai) | Viết Terraform cho VPC, subnets, security groups | 10/11/2025 | 10/11/2025 |
| 2 (Thứ Ba) | Deploy frontend lên S3 và CloudFront | 11/11/2025 | 11/11/2025 |
| 3 (Thứ Tư) | Containerize Django backend với Docker | 12/11/2025 | 12/11/2025 |
| 4 (Thứ Năm) | Setup ECS Fargate, API Gateway, ALB | 13/11/2025 | 13/11/2025 |
| 5 (Thứ Sáu) | Cấu hình VPC Endpoints, CORS | 14/11/2025 | 14/11/2025 |
| 6 (Thứ Bảy) | Thiết lập CloudWatch Logs & test E2E | 15/11/2025 | 15/11/2025 |

---

### **Week 10 Achievements**

#### 1. Hoàn thiện triển khai hạ tầng AWS
- VPC, Subnets, Security Groups được provision bằng Terraform
- Thiết lập AWS Cognito và App Client

#### 2. Deploy ứng dụng production
- Frontend deploy trên S3 + CloudFront CDN
- Backend chạy trên ECS Fargate với Auto-scaling

#### 3. Thiết lập Logging & Monitoring
- CloudWatch Logs cho toàn bộ service
- Test end-to-end authentication flow thành công

---

Tuần 10 hoàn thành mục tiêu đưa ứng dụng lên môi trường cloud production-ready.
