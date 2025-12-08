---
title: "Báo cáo sự kiện – AWS Well-Architected Security Pillar"
date: "2025-11-29"
chapter: false
weight: 6
pre: "<b>4.6.</b>"
---

# Báo cáo sự kiện: “AWS Well-Architected Security Pillar Workshop”

## Thông tin sự kiện

- **Chủ đề:** AWS Well-Architected Security Pillar  
- **Ngày:** Thứ Bảy, 29/11/2025  
- **Thời gian:** 08:30 AM – 12:00 PM  
- **Địa điểm:** Văn phòng AWS Vietnam  

---

## Mục tiêu sự kiện

- Hiểu vai trò của **Security Pillar** trong AWS Well-Architected Framework  
- Nắm vững nguyên tắc bảo mật hiện đại theo tiêu chuẩn AWS  
- Áp dụng các kỹ thuật bảo mật theo **5 trụ cột bảo mật**  
- Tìm hiểu chiến lược phòng vệ đa lớp trong môi trường cloud  
- Học từ case study và thực tế triển khai tại doanh nghiệp Việt Nam  

---

# 8:30 – 8:50 AM | Mở đầu & Nền tảng bảo mật

### Vai trò của Security Pillar
Buổi workshop mở đầu với phần giới thiệu về vị trí của trụ cột bảo mật trong **AWS Well-Architected Framework** — đây là 1 trong 6 trụ cột kiến trúc cloud hiện đại của AWS.

Bảo mật không phải là “bước cuối cùng”, mà cần được tích hợp xuyên suốt từ thiết kế kiến trúc đến vận hành hệ thống.

### Nguyên tắc cốt lõi

- **Least Privilege (quyền tối thiểu)**  
- **Zero Trust**  
- **Defense in Depth (phòng thủ đa lớp)**  

Các nguyên tắc này đảm bảo hệ thống vẫn an toàn khi một lớp bảo vệ bị phá vỡ.

### Mô hình chia sẻ trách nhiệm

- AWS chịu trách nhiệm: **Hạ tầng toàn cầu, hypervisor, dịch vụ managed**  
- Khách hàng chịu trách nhiệm: **Identity, dữ liệu, network, workload**  

Mô hình giúp doanh nghiệp hiểu rõ phạm vi trách nhiệm khi triển khai trên cloud.

### Các mối đe dọa phổ biến tại Việt Nam

- Cấu hình IAM sai dẫn đến lộ thông tin  
- S3 public exposure  
- Thiếu logging & detection  
- Không có multi-account governance  
- Ransomware & credential compromise  

---

# Pillar 1 — Identity & Access Management (8:50 – 9:30 AM)

### Kiến trúc IAM hiện đại

- **IAM Users, Roles, Policies**
  - Tránh long-term credentials  
  - Ưu tiên **STS session** & role assumption  

- **IAM Identity Center**
  - SSO tập trung cho multi-account  
  - Quản lý lifecycle người dùng đơn giản  

- **SCP & Permission Boundaries**
  - Kiểm soát quyền trong môi trường multi-account  
  - Ngăn privilege escalation  

- **Bảo vệ truy cập**
  - MFA bắt buộc  
  - Credential rotation  
  - IAM Access Analyzer  

### Mini Demo
- Kiểm tra policy IAM  
- Simulation truy cập để phát hiện over-permission  

---

# Pillar 2 — Detection (9:30 – 9:55 AM)

### Giám sát & phát hiện liên tục

- **AWS CloudTrail (Org-level)**
  - Audit tập trung toàn hệ thống  

- **Amazon GuardDuty**
  - Phát hiện mối đe dọa & hành vi bất thường  

- **AWS Security Hub**
  - Tổng hợp cảnh báo  
  - Đánh giá theo CIS, PCI  

### Logging ở mọi tầng

- **VPC Flow Logs**  
- **ALB Logs**  
- **S3 Access Logs**  

### Automation & Alerting

- EventBridge rules  
- Tự động hóa phản hồi sự cố  

### Detection-as-Code
- Phát hiện mối đe dọa như code  
- Deploy rule giống IaC  

---

# 9:55 – 10:10 AM | Coffee Break

Thảo luận và đặt câu hỏi cùng chuyên gia AWS.

---

# Pillar 3 — Infrastructure Protection (10:10 – 10:40 AM)

### Bảo vệ mạng & workload

- **Phân tách VPC**
  - Cách ly từng workload  
  - Private vs Public subnet  

- **Security Groups vs NACLs**
  - SG: Stateful — cấp tài nguyên  
  - NACL: Stateless — cấp subnet  

### Các lớp bảo vệ

- **AWS WAF**  
- **AWS Shield Advanced**  
- **AWS Network Firewall**  

### Bảo mật ở lớp workload

- Harden EC2  
- Bảo mật cơ bản ECS/EKS  
- Tách security trên runtime  

---

# Pillar 4 — Data Protection (10:40 – 11:10 AM)

### Encryption, Keys & Secrets

- **AWS KMS**
  - Key policy  
  - Key grant  
  - Rotation tự động  

- **Mã hóa dữ liệu**
  - At-rest & In-transit  
  - S3, EBS, RDS, DynamoDB  

- **Quản lý secrets**
  - Secrets Manager  
  - SSM Parameter Store  
  - Pattern rotation  

### Phân loại dữ liệu

- Phân loại data theo sensitivity  
- Áp dụng guardrail dựa trên tag  
- Tạo boundary hạn chế quyền truy cập  

---

# Pillar 5 — Incident Response (11:10 – 11:40 AM)

### Chu trình IR theo AWS

1. Chuẩn bị  
2. Phát hiện & phân tích  
3. Cô lập  
4. Loại bỏ mối đe dọa  
5. Khôi phục  
6. Học sau sự cố  

### Playbooks thực tế

- Lộ IAM key  
- S3 public exposure  
- EC2 nhiễm malware  

### Kỹ thuật tự động hoá

- Lambda tự động response  
- Step Functions workflow  
- Quarantine EC2  

---

# 11:40 AM – 12:00 PM | Tổng kết & Q&A

### Tổng hợp 5 trụ cột

- Identity & Access Management  
- Detection  
- Infrastructure Protection  
- Data Protection  
- Incident Response  

### Những sai lầm phổ biến

- Lạm dụng quyền IAM  
- Thiếu log & detection pipeline  
- Không thực thi encryption  
- Không có multi-account governance  

### Lộ trình học Security

- **AWS Security Specialty**  
- **AWS Solutions Architect Professional**  

---

# Bài học rút ra

- Bảo mật cần được triển khai **từ bước thiết kế**  
- Multi-account là nền tảng để mở rộng  
- Identity là lớp bảo vệ quan trọng nhất  
- Logging & Detection là xương sống bảo mật cloud  
- Mã hóa & rotation nên được tự động hóa  
- Incident Response cần có playbook rõ ràng  

---

# Ứng dụng vào công việc thực tế

- Chuẩn hóa IAM theo best practice AWS  
- Xây dựng pipeline logging & detection  
- Triển khai WAF + Shield Advanced bảo vệ ứng dụng web  
- Tự động rotation secret bằng KMS & Secrets Manager  
- Xây dựng playbook và workflow tự động hóa IR  



