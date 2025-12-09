---
title: "WEEK 4 WORKLOG"
date: "2025-12-01"
weight: 4
chapter: false
pre: " <b> 1.4 </b> "
---

# **WEEK 4 WORKLOG**

### **Week 4 Objectives**

* Hiểu và thực hành quản lý tài nguyên trên AWS bằng Tag và Resource Groups.
* Thực hành quản lý truy cập vào tài nguyên EC2 theo Tag với AWS IAM.
* Làm quen và sử dụng AWS CLI trên Amazon EC2 (Windows/Ubuntu).
* Triển khai ứng dụng Wordpress theo mô hình Highly Available trên AWS Cloud.
* Tìm hiểu cách triển khai hạ tầng tự động bằng AWS CloudFormation.
* Tự nghiên cứu thêm các kiến thức mới mở rộng liên quan đến AWS và Cloud.

---

### **Tasks to be carried out this week**

| Day | Task | Start Date | Completion Date | Reference/Material |
| :--- | :--- | :--- | :--- | :--- |
| 1 (Thứ Hai) | **Quản lý tài nguyên với Tag và Resource Groups**: Gắn Tag cho tài nguyên EC2, tạo Resource Group, áp dụng best practices về tagging và quản lý truy cập theo Tag | 29/09/2025 | 29/09/2025 | AWS Tagging Docs |
| 2 (Thứ Ba) | **Quản lý truy cập EC2 theo Resource Tag với IAM**: Tạo IAM Policy dựa theo Tag, thực hành hạn chế quyền truy cập tài nguyên dựa trên Tag | 30/09/2025 | 30/09/2025 | AWS IAM Docs |
| 3 (Thứ Tư) | **Sử dụng AWS CLI trên EC2**: Cài đặt và cấu hình AWS CLI trên EC2 (Windows/Ubuntu), chạy các lệnh CLI quản lý tài nguyên AWS | 01/10/2025 | 01/10/2025 | AWS CLI Docs |
| 4 (Thứ Năm) | **Triển khai Wordpress Highly Available**: Workshop triển khai Wordpress trên AWS theo mô hình HA: EC2 + RDS + ALB + Auto Scaling + S3 | 02/10/2025 | 02/10/2025 | AWS Workshop |
| 5 (Thứ Sáu) | **Khởi tạo hạ tầng dưới dạng Code với CloudFormation**: Tạo CloudFormation Stack, triển khai tài nguyên tự động và tìm hiểu template JSON/YAML | 03/10/2025 | 03/10/2025 | AWS CloudFormation Docs |
| 6 (Thứ Bảy) | **Tự học thêm kiến thức mới**: Tự tìm hiểu thêm về kiến trúc cloud, CI/CD, monitoring, Terraform, và các dịch vụ nâng cao trên AWS | 04/10/2025 | 04/10/2025 | AWS Whitepapers |

---

### **Week 4 Achievements**

Trong tuần thứ tư, các mục tiêu về quản lý tài nguyên, tự động hóa và triển khai ứng dụng thực tế đã được hoàn thành theo kế hoạch. Các kết quả đạt được bao gồm:

#### 1. Quản lý tài nguyên hiệu quả bằng Tag và Resource Groups
- Hiểu rõ ý nghĩa và lợi ích của Tag trong quản lý tài nguyên
- Gắn Tag cho tài nguyên EC2, S3 và VPC resources
- Tạo Resource Group theo Tag phục vụ quản lý tập trung
- Ứng dụng best practices:
  - `Environment`, `Owner`, `Project`, `CostCenter`
- Hiểu cách phân quyền truy cập theo Tag để bảo vệ tài nguyên nhạy cảm

#### 2. Quản lý truy cập EC2 bằng AWS IAM
- Viết IAM Policy dựa trên Resource Tag
- Thực hành giới hạn quyền truy cập EC2 theo Tag
- Hiểu rõ cơ chế điều kiện IAM (`Condition`, `aws:ResourceTag`)
- Nắm được mô hình phân quyền theo nguyên tắc Least Privilege

#### 3. Làm việc với AWS CLI trên EC2
- Cài đặt và cấu hình AWS CLI trên EC2 (Windows & Ubuntu)
- Sử dụng CLI để:
  - Tạo S3 bucket
  - Liệt kê EC2 instances
  - Mô tả cấu hình VPC
  - Tạo Key Pair
- Hiểu lợi ích CLI trong tự động hóa và DevOps

#### 4. Triển khai Wordpress theo mô hình Highly Available
- Hoàn thành workshop triển khai Wordpress trên AWS Cloud:
  - EC2 Instances chạy web application
  - RDS làm database backend
  - Application Load Balancer phân phối tải
  - Auto Scaling Group tự động mở rộng
  - S3 để lưu trữ dữ liệu tĩnh
- Hiểu cấu trúc tổng thể của một ứng dụng HA thực tế

#### 5. Tự động hóa hạ tầng bằng CloudFormation
- Tạo CloudFormation Stack triển khai tài nguyên tự động
- Hiểu định dạng Template (YAML/JSON)
- Tạo và quản lý Stack lifecycle:
  - Create → Update → Delete
- Thực hành deploy mẫu:
  - VPC
  - EC2 instance
  - Security Group

#### 6. Tự học và nghiên cứu thêm kiến thức mới
- Tìm hiểu thêm về:
  - Mô hình CI/CD pipeline
  - Terraform Infrastructure as Code
  - Cloud Monitoring (X-Ray, Log Insights)
  - Kiến trúc Microservices
- Chuẩn bị kiến thức cho các tuần kế tiếp (DevOps & Serverless)

---

**Tổng kết:**  
Tuần thứ tư tập trung vào khả năng **quản lý tài nguyên**, **tự động hóa hạ tầng**, và triển khai ứng dụng thực tế theo mô hình **Highly Available**. Việc thực hành CloudFormation, IAM theo Tag và workshop Wordpress giúp hiểu sâu hơn về kiến trúc cloud và chuẩn bị cho giai đoạn tiếp theo: triển khai hệ thống theo hướng DevOps và IaC.
