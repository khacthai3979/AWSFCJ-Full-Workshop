---
title: "WEEK 3 WORKLOG"
date: "2025-11-24"
weight: 3
chapter: false
pre: " <b> 1.3 </b> "
---

# **WEEK 3 WORKLOG**

### **Week 3 Objectives**

* Thực hành triển khai và quản lý cơ sở dữ liệu trên AWS với Amazon Relational Database Service (Amazon RDS).
* Củng cố kiến thức mạng thông qua khóa học CCNA cơ bản.
* Hiểu và thực hành cơ chế tự động mở rộng quy mô ứng dụng với Amazon EC2 Auto Scaling.
* Làm quen với tính năng giám sát tài nguyên và ứng dụng bằng Amazon CloudWatch.
* Chuẩn bị cho giai đoạn tiếp theo bằng việc học tập và trao đổi trực tiếp trên trường.

---

### **Tasks to be carried out this week**

| Day | Task | Start Date | Completion Date | Reference/Material |
| :--- | :--- | :--- | :--- | :--- |
| 1 (Thứ Hai) | **Thực hành lab Amazon RDS**: Tạo RDS instance, cấu hình VPC, subnet group, security group, kết nối database từ EC2 và làm quen với các tính năng cơ bản | 22/09/2025 | 22/09/2025 | AWS RDS Docs |
| 2 (Thứ Ba) | **Tiếp tục lab RDS & học CCNA cơ bản**: Tối ưu cấu hình RDS, thực hành backup/restore, tiếp tục học khái niệm IP Subnet, Routing | 23/09/2025 | 23/09/2025 | AWS RDS Docs, CCNA Course |
| 3 (Thứ Tư) | **Thực hành EC2 Auto Scaling**: Tạo Auto Scaling Group (ASG), cấu hình Launch Template, thiết lập scaling policies theo CPU Utilization | 24/09/2025 | 24/09/2025 | AWS Auto Scaling Docs |
| 4 (Thứ Năm) | **Tạo bảng theo dõi hệ thống với Amazon CloudWatch**: Tạo Dashboard theo dõi CPU, Network, Disk, thiết lập Alarms cảnh báo hệ thống | 25/09/2025 | 25/09/2025 | AWS CloudWatch Docs |
| 5 (Thứ Sáu) | **Đi lên trường học tập**: Lên trường học tập | 26/09/2025 | 26/09/2025 | Trường/mentor |

---

### **Week 3 Achievements**

Trong tuần thứ ba, các nhiệm vụ thực hành tập trung vào việc triển khai database, auto scaling và giám sát hệ thống đã được hoàn thành. Kết quả đạt được bao gồm:

#### 1. Thực hành triển khai cơ sở dữ liệu với Amazon RDS
- Tạo thành công RDS Instance (PostgreSQL/MySQL)
- Cấu hình RDS Subnet Group và Security Group
- Kết nối RDS từ EC2 Instance và Cloud9
- Thực hành backup database và restore từ snapshot
- Tìm hiểu các tính năng chính của RDS:
  - Multi-AZ Deployment
  - Automated Backups
  - Performance Insights
  - Parameter Group

#### 2. Củng cố kiến thức CCNA cơ bản
- Tiếp tục học về:
  - Subnetting và VLSM
  - Static Routing và Dynamic Routing (OSPF/RIP)
  - LAN vs WAN
  - Routing Table và cách hoạt động
- Hiểu rõ hơn vai trò của networking trong cloud

#### 3. Tự động mở rộng ứng dụng với Amazon EC2 Auto Scaling
- Tạo Auto Scaling Group (ASG) với Launch Template
- Cấu hình Scaling Policy dựa theo CPU Utilization
- Thực hành scale-out và scale-in tự động
- Hiểu luồng scaling:
  - Trigger → Launch new instance → Register with Load Balancer

#### 4. Theo dõi hệ thống với Amazon CloudWatch
- Tạo Dashboard theo dõi:
  - CPU Utilization
  - Network In/Out
  - Memory sử dụng (trên Agent)
- Thiết lập CloudWatch Alarms khi CPU vượt ngưỡng
- Hiểu cơ chế log:
  - CloudWatch Logs
  - Metric Filters

#### 5. Học tập và trao đổi trực tiếp trên trường
- Thảo luận các kiến thức liên quan với mentor
- Nhận góp ý về cách học và roadmap
- Trao đổi kinh nghiệm thực hành với bạn học
- Tập trung củng cố kiến thức networking và cloud

---

**Tổng kết:**  
Tuần thứ ba giúp hiểu sâu hơn về **database trên AWS**, thực hành **tự động mở rộng EC2**, nắm được cách **giám sát hệ thống với CloudWatch**, đồng thời củng cố kiến thức **CCNA**. Đây là nền tảng quan trọng để tiếp tục triển khai hệ thống hoàn chỉnh trong các tuần sau.
