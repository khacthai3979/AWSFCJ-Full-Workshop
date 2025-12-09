---
title : "Cấu hình S3 Event Trigger"
date : "2025-10-10"
weight : 6
chapter : false
pre : " <b> 5.6 </b> "
---


### 5.1 Add Trigger cho Lambda 1

1. Vào Lambda Console
2. Click vào `StartFaceDetectFunction`
3. Click **"Add trigger"**

**Configure trigger:**
- **Source:** S3
- **Bucket:** Select `face-blur-input-bucket-[your-name]`
- **Event type:** All object create events, PUT, POST, Multipart Upload Completed 
- **Prefix:** (leave empty)
- **Suffix:** `.mp4`
- **Acknowledge:** Check the box
- Click **"Add"**
![image](/images/Image_workshop/StepFunc/9.1---add-trigger.png)

### 5.2 Add Second Trigger cho .mov files

1. Click **"Add trigger"** again
2. Same configuration nhưng:
   - **Suffix:** `.mov`
3. Click **"Add"**

### 5.3 Verify Triggers

1. Tab **"Configuration"** → **"Triggers"**
2. Confirm có 2 triggers từ S3 bucket

![image](/images/Image_workshop/StepFunc/11--sec-trigger-.mov.png)
**Checkpoint:** S3 triggers đã được cấu hình

![image](/images/Image_workshop/StepFunc/review-trigger.png)