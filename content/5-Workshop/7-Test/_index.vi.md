---
title : "Test hệ thống"
date : "2025-10-10"
weight : 7
chapter : false
pre : " <b> 5.7 </b> "
---


### 6.1 Chuẩn bị Test Video

**Tìm video test:**
- Video ngắn (< 30 giây) để test nhanh
- Format: .mp4 hoặc .mov
- Có ít nhất 1 khuôn mặt
- Kích thước < 100MB

**Download sample video:**
- Pexels.com (free videos)
- Pixabay.com
- Hoặc dùng video từ điện thoại

### 6.2 Upload Video

1. Vào S3 Console
2. Click vào `face-blur-input-bucket-[your-name]`
3. Click **"Upload"**
4. Click **"Add files"**
5. Chọn video test
6. Click **"Upload"**
![image](static/images/Image_workshop/StepFunc/s3-upload.png)

### 6.3 Theo dõi Execution

**Step 1: Check Lambda Logs**

   1. Vào Lambda Console
   2. Click `StartFaceDetectFunction`
   3. Tab **"Monitor"** → **"View logs in CloudWatch"**
   4. Click vào log stream mới nhất
   5. Xem logs:
      ```
      START RequestId: xxx
      Started processing: job-id-xxx
      END RequestId: xxx
      ```

**Step 2: Check Step Functions**

   1. Vào Step Functions Console
   2. Click vào `FaceBlurStateMachine`
   3. Tab **"Executions"**
   4. Click vào execution mới nhất (status: Running)
   5. Xem visual workflow:
      - 🔵 Blue = Running
      - 🟢 Green = Succeeded
      - 🔴 Red = Failed

**Step 3: Monitor Progress**

   Workflow sẽ trải qua các states:
   1. **CheckJobStatus** (lặp lại nhiều lần với Wait)
   2. **GetTimestampsAndFaces**
   3. **BlurFacesOnVideo**
   4. **ExecutionSucceeded**

![image](/images/Image_workshop/StepFunc/check-step-run.png)

   **Thời gian ước tính:**
   - Video 30 giây: 2-5 phút
   - Video 2 phút: 5-10 phút

### 6.4 Check Output

   1. Vào S3 Console
   2. Click vào `face-blur-output-bucket-[your-name]`
   3. Tìm file video (cùng tên với input)
   4. Click **"Download"**
   5. Xem video đã được xử lý


