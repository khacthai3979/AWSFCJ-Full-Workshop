---
title : "Test End-to-end System"
date : "2025-10-10"
weight : 9
chapter : false
pre : " <b> 5.9 </b> "
---

## Step 6: End-to-End System Test

### 6.1 Prepare Test Video

**Video requirements:**
- Format: .mp4 or .mov
- Contains at least 1 clear face
- Duration: 10-30 seconds (for quick testing)
- Size: < 100 MB

**Download free test videos:**
- Pexels.com → Search "people walking"
- Pixabay.com → Search "interview"
- Or record a video from your phone

### 6.2 Upload Video to S3

1. S3 Console → Input bucket (`face-blur-input-bucket-**`)
2. Click **Upload**
3. Click **Add files**
4. Select test video
5. Click **Upload**
6. Wait for upload to complete (progress bar = 100%)

![iamge](/images/Image_workshop/StepFunc/testfinal-upload-s3.png)

**Note:** Wait for upload to complete before checking logs!

### 6.3 Monitor Workflow Execution

#### 6.3.1 Check Lambda 1 (Start Face Detect)

1. Lambda Console → `StartFaceDetectFunction`
2. Tab **Monitor** → **View logs in CloudWatch**
3. Click on the latest log stream (most recent time)

**Successful logs:**
```
START RequestId: abc-123-def
Started processing: job-id-xyz123
END RequestId: abc-123-def
```

**If there are errors:**
- Check if S3 trigger is configured
- Check IAM permissions

#### 6.3.2 Check Step Functions

1. Step Functions Console → `FaceBlurStateMachine`
2. Tab **Executions**
3. Click on the latest execution (status: Running or Succeeded)

**Visual Workflow:**
- 🔵 **Blue (Running):** Step is running
- 🟢 **Green (Succeeded):** Step completed
- 🔴 **Red (Failed):** Step failed

**Workflow will go through:**
1. **CheckJobStatus** (repeats multiple times with Wait1Second)
   - Waiting for Rekognition to analyze video
   - Time: 30-60 seconds for a 30-second video
2. **GetTimestampsAndFaces**
   - Retrieve face information and timestamps
   - Time: 5-10 seconds
3. **BlurFacesOnVideo**
   - Process blur with OpenCV
   - Time: 1-5 minutes depending on video
4. **ExecutionSucceeded**
   - Complete!

![image](/images/Image_workshop/StepFunc/testfinale-checking-stepfunc.png) 

**Total time:** 2-7 minutes for a 30-second video

#### 6.3.3 Check Lambda 4 (Blur Faces) - Details

1. Lambda Console → `BlurFacesFunction`
2. Tab **Monitor** → **View logs in CloudWatch**
3. Click on the latest log stream


**If you see errors:**
- Check IAM permissions (S3 GetObject/PutObject)
- Check if memory and timeout are sufficient
- Check if output bucket name is correct

### 6.4 Download and Verify Output Video

#### 6.4.1 Download Video

1. S3 Console → Output bucket (`face-blur-output-bucket-*****`)
2. Find the video file (same name as input)
3. Select file → Click **Download**
4. Wait for download to complete

#### 6.4.2 Verify Results

**Open the video and check:**

**Faces are blurred (pixelated):**
- All faces in the video are blurred
- Blur style: Pixelation (squares)
- Blur follows face movement

![image](/images/Image_workshop/StepFunc/testfinal-dowload-result-s3.png)