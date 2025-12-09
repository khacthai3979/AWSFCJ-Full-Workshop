---
title : "Test the System"
date : "2025-10-10"
weight : 7
chapter : false
pre : " <b> 5.7 </b> "
---


### 6.1 Prepare Test Video

**Find test video:**
- Short video (< 30 seconds) for quick testing
- Format: .mp4 or .mov
- Contains at least 1 face
- Size < 100MB

**Download sample video:**
- Pexels.com (free videos)
- Pixabay.com
- Or use a video from your phone

### 6.2 Upload Video

1. Go to S3 Console
2. Click on `face-blur-input-bucket-[your-name]`
3. Click **"Upload"**
4. Click **"Add files"**
5. Select test video
6. Click **"Upload"**
![image](static/images/Image_workshop/StepFunc/s3-upload.png)

### 6.3 Monitor Execution

**Step 1: Check Lambda Logs**

   1. Go to Lambda Console
   2. Click `StartFaceDetectFunction`
   3. Tab **"Monitor"** → **"View logs in CloudWatch"**
   4. Click on the latest log stream
   5. View logs:
      ```
      START RequestId: xxx
      Started processing: job-id-xxx
      END RequestId: xxx
      ```

**Step 2: Check Step Functions**

   1. Go to Step Functions Console
   2. Click on `FaceBlurStateMachine`
   3. Tab **"Executions"**
   4. Click on the latest execution (status: Running)
   5. View visual workflow:
      - 🔵 Blue = Running
      - 🟢 Green = Succeeded
      - 🔴 Red = Failed

**Step 3: Monitor Progress**

   The workflow will go through these states:
   1. **CheckJobStatus** (repeats multiple times with Wait)
   2. **GetTimestampsAndFaces**
   3. **BlurFacesOnVideo**
   4. **ExecutionSucceeded**

![image](/images/Image_workshop/StepFunc/check-step-run.png)

   **Estimated time:**
   - 30 second video: 2-5 minutes
   - 2 minute video: 5-10 minutes

### 6.4 Check Output

   1. Go to S3 Console
   2. Click on `face-blur-output-bucket-[your-name]`
   3. Find the video file (same name as input)
   4. Click **"Download"**
   5. View the processed video
