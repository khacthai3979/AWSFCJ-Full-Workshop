---
title : "Workshop"
date :  "2025-10-10" 
weight : 5 
chapter : false
pre: " <b> 5. </b> "
---
# IMPLEMENTING ADVANCED FEATURES OF AWS APPLICATION LOAD BALANCER

### Introduction

Trong bối cảnh AI/ML trở nên phổ biến, người dùng và doanh nghiệp ngày càng quan tâm đến vấn đề bảo mật dữ liệu cá nhân. Với các ứng dụng Computer Vision (CV), một trong những phương pháp bảo vệ thông tin hiệu quả nhất là làm mờ khuôn mặt trong ảnh và video.

Quy trình này gồm hai bước chính:
1. Nhận diện khuôn mặt trong từng khung hình bằng ML/Deep Learning.
2. Che mờ vùng khuôn mặt bằng xử lý ảnh.

Trong workshop này, chúng ta xây dựng ứng dụng serverless tự động phát hiện và làm mờ khuôn mặt trong video, sử dụng:

- Amazon Rekognition Video để nhận diện khuôn mặt.

- AWS Step Functions để điều phối workflow.

- AWS Lambda (Container Image) và OpenCV để xử lý video.

Giải pháp phù hợp cho các hệ thống cần đảm bảo quyền riêng tư, tuân thủ quy định bảo mật dữ liệu hoặc xử lý video quy mô lớn.

![ConnectPrivate](/images/Image_workshop/blur_face_stepfunc.drawio.png) 

## Solution Overview

Kiến trúc tổng thể của hệ thống sử dụng **100% serverless**, đảm bảo:

- **Không cần quản lý máy chủ**
- **Tự động scale theo nhu cầu**
- **Tích hợp API ML có sẵn từ Amazon Rekognition**
- **Không yêu cầu kiến thức Deep Learning chuyên sâu**

---

## Luồng xử lý chính

1. **Upload Video**
   - Người dùng tải video lên Amazon S3.

2. **S3 Event Trigger**
   - S3 phát sinh sự kiện `ObjectCreated`, kích hoạt Lambda **Start Face Detection**.

3. **Face Detection Job**
   - Lambda gọi API **Amazon Rekognition Video** để phân tích khuôn mặt (asynchronous job).

4. **Start Workflow**
   - Lambda khởi chạy **AWS Step Functions State Machine**, truyền vào:
     - S3 Object Key  
     - Video Metadata  
     - Rekognition Job ID  

---

## Step Functions Workflow

### 1. Wait for Completion
- Step Functions gọi Lambda **Wait For Completion** để chờ Amazon Rekognition xử lý.

### 2. Retrieve Detection Results
- Sau khi job hoàn tất, Step Functions gọi Lambda để lấy kết quả:
  - **Bounding Boxes**
  - **Timestamps**

### 3. Video Blurring
- Lambda cuối cùng sử dụng OpenCV để làm mờ khuôn mặt:
  - Fetch Docker image hỗ trợ OpenCV từ **Amazon ECR**
  - Đọc file video từ **S3**
  - Làm mờ khuôn mặt theo `timestamp` và `bounding box`
  - Xuất video đã xử lý lên **Output S3 Bucket**
  - Xóa file tạm trong môi trường Lambda

---

## Output
- Video đã làm mờ khuôn mặt được lưu trữ trong một **S3 bucket đầu ra**, sẵn sàng chia sẻ cho hệ thống phía sau hoặc người dùng cuối.


### Content

1. [Introduction](1-Introduce)
2. [Initalize S3 Bucket](2-S3)
3. [Create IAM Roles & Permissions](3-IAM)
4. [Build Lambda Functions](4-Lambda)
5. [Setup AWS Step Functions](5-StepFunction)
6. [Configure S3 Event Trigger](6-S3EventTrigger)
7. [Testing Workflow](7-Test)
8. [Build ECR Image for OpenCV](8-ECR)
9. [Final Result](9-FinalResult)
10. [Clean Resources](10-CleanResources)

