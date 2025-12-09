---
title: "WEEK 8 WORKLOG"
date: "2025-12-29"
weight: 8
chapter: false
pre: " <b> 1.8 </b> "
---

# **WEEK 8 WORKLOG**

### **Week 8 Objectives**

* Nghiên cứu và xây dựng thiết kế kiến trúc hệ thống cho dự án.
* Tham khảo ý kiến từ mentor và điều chỉnh bản thiết kế kiến trúc.
* Xây dựng bản dự thảo kế hoạch triển khai dự án (project plan & timeline).
* Tìm hiểu Terraform để triển khai hạ tầng dưới dạng mã nguồn (Infrastructure as Code).
* Tìm hiểu Docker và containerization để đóng gói ứng dụng.
* Thực hành triển khai ứng dụng lên Amazon Elastic Container Service (Amazon ECS).

---

### **Tasks to be carried out this week**

| Day | Task | Start Date | Completion Date | Reference/Material |
| :--- | :--- | :--- | :--- | :--- |
| 1 (Thứ Hai) | **Nghiên cứu & lập kế hoạch kiến trúc**: Xác định kiến trúc tổng thể, lựa chọn dịch vụ AWS phù hợp cho dự án | 27/10/2025 | 27/10/2025 | AWS Architecture Docs |
| 2 (Thứ Ba) | **Tham khảo ý kiến và chỉnh sửa bản thiết kế kiến trúc**: Thảo luận cùng mentor, tiếp thu góp ý và điều chỉnh bản thiết kế | 28/10/2025 | 28/10/2025 | Mentor |
| 3 (Thứ Tư) | **Dự thảo project plan & timeline**: Xây dựng timeline triển khai dự án theo từng phase và phân bổ thời gian thực hiện | 29/10/2025 | 29/10/2025 | Project Notes |
| 4 (Thứ Năm) | **Tìm hiểu Terraform & triển khai ứng dụng với Docker**: Học khái niệm IaC, tạo terraform module đơn giản, tạo Dockerfile & build image | 30/10/2025 | 30/10/2025 | Terraform Docs |
| 5 (Thứ Sáu) | **Triển khai ứng dụng lên Amazon ECS**: Tạo ECS Cluster, Task Definition, Service, kiểm tra chạy container trên ECS | 31/10/2025 | 31/10/2025 | AWS ECS Docs |

---

### **Week 8 Achievements**

Tuần thứ tám đánh dấu giai đoạn quan trọng của dự án, chuyển từ nghiên cứu sang thiết kế kiến trúc và chuẩn bị triển khai thực tế. Kết quả đạt được bao gồm:

#### 1. Hoàn thiện thiết kế kiến trúc hệ thống
- Xác định kiến trúc phù hợp với dự án:
  - AWS VPC
  - Amazon ECS (Fargate)
  - Amazon RDS
  - Amazon Cognito
  - AWS Bedrock
  - CloudFront + S3
  - CloudWatch Logs & Metrics
- Thiết kế kiến trúc theo hướng **cloud-native, bảo mật và dễ mở rộng**
- Lựa chọn dịch vụ phù hợp với mục tiêu dự án và ngân sách hiện tại

#### 2. Tham khảo ý kiến mentor & điều chỉnh thiết kế
- Trình bày bản thiết kế kiến trúc sơ bộ
- Nhận góp ý về:
  - Best practices on AWS
  - Security layers
  - Networking (VPC Endpoints, ALB, Security Groups)
  - Cost optimization
- Chỉnh sửa lại kiến trúc dựa theo feedback

#### 3. Dự thảo project plan & timeline
- Xác định các phase chính:
  - Research
  - Architecture Design
  - Frontend/Backend Development
- Xây dựng timeline tuần theo milestones
- Định nghĩa deliverables theo từng phase
- Lên kế hoạch test, deploy & optimization

#### 4. Tìm hiểu Terraform & Docker
- Hiểu rõ khái niệm IaC và lợi ích:
  - Reproducible infrastructure
  - Version control
  - Automation
- Viết terraform module đơn giản:
  - VPC
  - Subnet
  - Security Group
- Học Docker:
  - Tạo Dockerfile
  - Build image & chạy container
  - Hiểu khái niệm Layer, Registry

#### 5. Triển khai ứng dụng lên ECS
- Tạo ECS Cluster (Fargate)
- Tạo Task Definition & cấu hình container
- Deploy ứng dụng Flask hoặc Node.js demo
- Kiểm tra Container Logs trên CloudWatch
- Hiểu cơ chế chạy **serverless containers**

---

**Tổng kết:**  
Tuần thứ tám là giai đoạn chuyển từ nghiên cứu sang **thiết kế kiến trúc và triển khai thử nghiệm**, bao gồm việc tạo project plan, hoàn thiện bản kiến trúc, và thực hành với các công nghệ triển khai thực tế như Terraform, Docker và Amazon ECS. Đây là bước nền tảng quan trọng để bước sang giai đoạn phát triển ứng dụng và triển khai hạ tầng hoàn chỉnh trong các tuần tiếp theo.
