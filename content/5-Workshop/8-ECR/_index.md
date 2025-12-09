---
title : "Add OpenCV to Lambda 4 - Using Container Image"
date : "2025-10-10"
weight : 8
chapter : false
pre : " <b> 5.8 </b> "
---

In the previous section, Lambda 4 only copied video from input to output without blurring. Now we will deploy a Lambda function with a Docker container to **actually blur faces** in videos using OpenCV and MoviePy.

**Prerequisites before starting:**
- Docker Desktop installed and running
- AWS CLI configured with credentials
- Completed steps 1-5 above (S3, IAM, Lambda, Step Functions)

**What we will do:**
1. Create 4 code files (Dockerfile, requirements.txt, app.py, video_processor.py)
2. Create ECR repository to store Docker image
3. Build Docker image on local machine
4. Push image to AWS ECR
5. Update Lambda function to use container image
6. Test and verify results

---

## Step 1: Prepare Code Files

### 1.1 Create Project Directory

Create a new directory in the project

**Windows:**
```powershell
mkdir lambda-blur-docker
cd lambda-blur-docker
```


### 1.2 Create File: Dockerfile

Create a file named `Dockerfile` (without extension) with the following content:

```dockerfile
FROM public.ecr.aws/lambda/python:3.8

RUN yum install -y mesa-libGL

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY video_processor.py .
COPY app.py .

CMD ["app.lambda_function"]
```


### 1.3 Create File: requirements.txt

Create file `requirements.txt` with the following content:

```txt
boto3==1.18.6
botocore==1.21.6
opencv-python-headless==4.5.3.56
numpy==1.22.0
moviepy==1.0.3
imageio==2.9.0
imageio-ffmpeg==0.4.4
```


### 1.4 Create File: video_processor.py

Create file `video_processor.py` with the complete code:

```python
import cv2
import numpy as np
from moviepy.editor import VideoFileClip
import os


def anonymize_face_pixelate(image, blocks=10):
    """
    Apply pixelation blur to face region
    """
    (h, w) = image.shape[:2]
    x_coords = np.linspace(0, w, blocks + 1, dtype="int")
    y_coords = np.linspace(0, h, blocks + 1, dtype="int")
    
    for i in range(1, len(y_coords)):
        for j in range(1, len(x_coords)):
            x1, x2 = x_coords[j - 1], x_coords[j]
            y1, y2 = y_coords[i - 1], y_coords[i]
            roi = image[y1:y2, x1:x2]
            (b, g, r) = [int(x) for x in cv2.mean(roi)[:3]]
            cv2.rectangle(image, (x1, y1), (x2, y2), (b, g, r), -1)
    
    return image


def apply_faces_to_video(timestamps, input_path, output_path, video_metadata):
    """
    Apply blur to detected faces in video
    """
    print(f'Processing video with {len(timestamps)} timestamp groups')
    
    frame_rate = video_metadata["FrameRate"]
    frame_height = video_metadata["FrameHeight"]
    frame_width = video_metadata["FrameWidth"]
    
    print(f'Video: {frame_width}x{frame_height} @ {frame_rate}fps')
    
    # Padding for face boxes
    width_delta = int(frame_width / 250)
    height_delta = int(frame_height / 100)
    
    # Open video
    cap = cv2.VideoCapture(input_path)
    fourcc = cv2.VideoWriter_fourcc('m', 'p', '4', 'v')
    out = cv2.VideoWriter(
        output_path,
        fourcc,
        int(frame_rate),
        (frame_width, frame_height)
    )
    
    frame_counter = 0
    faces_blurred = 0
    
    while cap.isOpened():
        ret, frame = cap.read()
        if not ret:
            break
        
        # Check if current frame has faces to blur
        for timestamp_str, faces in timestamps.items():
            timestamp_ms = int(timestamp_str)
            lower_bound = int(timestamp_ms / 1000 * frame_rate)
            upper_bound = int(timestamp_ms / 1000 * frame_rate + frame_rate / 2) + 1
            
            if lower_bound <= frame_counter <= upper_bound:
                for face in faces:
                    # Calculate face box coordinates
                    x = int(face['Left'] * frame_width) - width_delta
                    y = int(face['Top'] * frame_height) - height_delta
                    w = int(face['Width'] * frame_width) + 2 * width_delta
                    h = int(face['Height'] * frame_height) + 2 * height_delta
                    
                    # Ensure coordinates are within frame
                    x = max(0, x)
                    y = max(0, y)
                    x2 = min(frame_width, x + w)
                    y2 = min(frame_height, y + h)
                    
                    # Extract and blur face region
                    if x < x2 and y < y2:
                        face_region = frame[y:y2, x:x2]
                        if face_region.size > 0:
                            blurred = anonymize_face_pixelate(face_region, blocks=10)
                            frame[y:y2, x:x2] = blurred
                            faces_blurred += 1
        
        out.write(frame)
        frame_counter += 1
        
        if frame_counter % 100 == 0:
            print(f'Processed {frame_counter} frames...')
    
    cap.release()
    out.release()
    
    print(f'Complete! Processed {frame_counter} frames, blurred {faces_blurred} face instances')


def integrate_audio(original_video, processed_video, audio_path='/tmp/audio.mp3'):
    """
    Extract audio from original and add to processed video
    """
    print('Integrating audio...')
    
    try:
        # Extract audio from original
        original_clip = VideoFileClip(original_video)
        
        if original_clip.audio is None:
            print('No audio track found in original video')
            return
        
        original_clip.audio.write_audiofile(audio_path, logger=None)
        
        # Add audio to processed video
        temp_output = '/tmp/final_output.mp4'
        processed_clip = VideoFileClip(processed_video)
        processed_clip = processed_clip.set_audio(VideoFileClip(original_video).audio)
        processed_clip.write_videofile(
            temp_output,
            codec='libx264',
            audio_codec='aac',
            logger=None
        )
        
        # Replace processed video with final version
        os.replace(temp_output, processed_video)
        
        # Cleanup
        if os.path.exists(audio_path):
            os.remove(audio_path)
        
        print('Audio integration complete')
        
    except Exception as e:
        print(f'Audio integration failed: {str(e)}')
        print('Continuing without audio...')
```

### 1.5 Create File: app.py

Create file `app.py` with the following content:

```python
import json
import os
import boto3
from video_processor import apply_faces_to_video, integrate_audio

s3 = boto3.client('s3')
output_bucket = os.environ.get('OUTPUT_BUCKET')


def lambda_function(event, context):
    """
    Lambda handler for blurring faces in video
    """
    try:
        print('='*50)
        print('Starting face blur processing...')
        
        # Extract event data
        bucket = event['s3_object_bucket']
        key = event['s3_object_key']
        timestamps = event.get('timestamps', {})
        response = event.get('response', {})
        video_metadata = response.get('VideoMetadata', {})
        
        print(f'Input: s3://{bucket}/{key}')
        print(f'Output: s3://{output_bucket}/{key}')
        print(f'Faces detected at {len(timestamps)} timestamps')
        
        if not timestamps:
            print('⚠️  No faces detected, copying original video')
        
        # Download video
        filename = key.split('/')[-1]
        local_input = f'/tmp/{filename}'
        local_output = f'/tmp/blurred-{filename}'
        
        print(f'Downloading video...')
        s3.download_file(bucket, key, local_input)
        file_size_mb = os.path.getsize(local_input) / (1024 * 1024)
        print(f'✅ Downloaded: {file_size_mb:.2f} MB')
        
        # Apply blur if faces detected
        if timestamps and video_metadata:
            print('Applying blur to faces...')
            apply_faces_to_video(
                timestamps,
                local_input,
                local_output,
                video_metadata
            )
            print('✅ Blur applied')
            
            # Integrate audio
            print('Integrating audio...')
            integrate_audio(local_input, local_output)
            print('✅ Audio integrated')
        else:
            # No faces, just copy
            import shutil
            shutil.copy(local_input, local_output)
            print('✅ Video copied (no faces to blur)')
        
        # Upload result
        print(f'Uploading to output bucket...')
        s3.upload_file(local_output, output_bucket, key)
        output_size_mb = os.path.getsize(local_output) / (1024 * 1024)
        print(f'✅ Uploaded: {output_size_mb:.2f} MB')
        
        # Cleanup
        os.remove(local_input)
        os.remove(local_output)
        print('✅ Cleanup complete')
        
        print('='*50)
        print('SUCCESS!')
        
        return {
            'statusCode': 200,
            'body': json.dumps({
                'message': 'Video processed successfully',
                'faces_blurred': len(timestamps),
                'output_location': f's3://{output_bucket}/{key}'
            })
        }
        
    except Exception as e:
        print(f'❌ ERROR: {str(e)}')
        import traceback
        print(traceback.format_exc())
        raise e
```

### 1.6 Verify Files

Check that you have all 4 files:

```powershell
# Windows
dir
```

You should see:
```
Dockerfile
requirements.txt
app.py
video_processor.py
```

---

## Step 2: Create ECR Repository

### 1.1 Go to ECR Console

1. AWS Console → Search for **"ECR"** (Elastic Container Registry)
2. Click **"Get Started"** or **"Create repository"**

### 1.2 Configure Repository

**Repository settings:**
- **Visibility:** Private
- **Repository name:** `blur-faces-lambda`
- **Tag immutability:** Disabled
- **Scan on push:** Disabled (optional)
- **Encryption:** AES-256 (default)

Click **"Create repository"**

### 1.3 Save Repository URI

After creation, you will see:
```
Repository URI: 123456789012.dkr.ecr.us-east-1.amazonaws.com/blur-faces-lambda
```

**Save this URI!** You will need it in the next step.

---

## Step 3: Build Docker Image

### 3.1 Check Docker Desktop

**Before building, ensure Docker Desktop is running:**

**Windows:**
1. Open Docker Desktop from Start Menu
2. Wait until you see "Docker Desktop is running"
3. Docker icon in system tray will stop animating


**Verify Docker is working:**
```powershell
docker --version
```

You should see:
```
Docker version 24.0.x, build xxxxx
```

### 3.2 Build Image with Correct Platform

**⚠️ IMPORTANT:** Lambda runs on Linux AMD64, so we must build with this platform!

**Windows PowerShell:**
```powershell
cd C:/AWS/lambda-blur-docker
docker build --platform linux/amd64 -t blur-faces-lambda .
```


**Explanation:**
- `--platform linux/amd64`: Build for Linux AMD64 (Lambda architecture)


**Time:** 5-10 minutes (first time downloading base image and dependencies)

### 3.3 Verify Image

**Check that the image was built successfully:**

**Windows:**
```powershell
docker images | findstr blur-faces-lambda
```

## Step 4: Push Image to AWS ECR

### 4.1 Get AWS Account ID

**Run command to get Account ID:**
```powershell
aws sts get-caller-identity --query Account --output text
```

**Output:**
```
Example: 123456789
```

**Save this number!** This is your AWS Account ID.

### 4.2 Set Variables (For Convenience)

**Windows PowerShell:**
```powershell
$ACCOUNT_ID = "123456789"
$REGION = "ap-southeast-1"
$REPO_NAME = "blur-faces-lambda"
```


**Explanation:**
- `ACCOUNT_ID`: Your AWS Account ID
- `REGION`: Region you are using (ap-southeast-1 = Singapore)
- `REPO_NAME`: ECR repository name (must match the name created in Step 2)

### 4.3 Login to ECR

**Windows PowerShell:**
```powershell
aws ecr get-login-password --region $REGION | docker login --username AWS --password-stdin "$ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com"
```

**Success output:**
```
Login Succeeded
```

**If you encounter errors:**

**Error 1: AWS CLI not configured**
```
Unable to locate credentials
```
→ Run `aws configure` and enter Access Key, Secret Key

**Error 2: Region not found**
```
Could not connect to the endpoint URL
```
→ Check if region name is correct (ap-southeast-1, us-east-1, etc.)

### 4.4 Tag Image for ECR

**Windows PowerShell:**
```powershell
docker tag blur-faces-lambda:latest "$ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com/$REPO_NAME:latest"
```



**Verify tag:**
```powershell
docker images | findstr blur-faces-lambda
```

**You should see 2 lines:**
```
blur-faces-lambda                                                latest    abc123    2 mins ago    1.2GB
208613876063.dkr.ecr.ap-southeast-1.amazonaws.com/blur-faces-lambda   latest    abc123    2 mins ago    1.2GB
```

### 4.5 Push Image to ECR

**Windows PowerShell:**
```powershell
docker push "$ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com/$REPO_NAME:latest"
```

**Time:** 5-10 minutes (uploading ~2GB)



**If you encounter errors:**

**Error 1: Repository not found**
```
repository does not exist or may require authorization
```
→ Check if repository name is correct? Have you created the ECR repository?

**Error 2: Authentication token expired**
```
authorization token has expired
```
→ Login again: Re-run the `aws ecr get-login-password...` command

**Error 3: Image manifest not supported**
```
The image manifest, config or layer media type is not supported
```
→ Rebuild image with `--platform linux/amd64`

### 4.6 Verify on AWS Console

**Check that the image is in ECR:**

1. AWS Console → ECR
2. Click on repository `blur-faces-lambda`
3. Tab **Images**
4. You should see:
   - **Image tag:** latest
   - **Image size:** ~2 GB
   - **Pushed at:** Time just pushed
   - **Image URI:** Example: `123456789.dkr.ecr.ap-southeast-1.amazonaws.com/blur-faces-lambda:latest`

5. **Copy Image URI** - you will need it in the next step!

**Checkpoint:** Image has been successfully pushed to ECR!

---

## Step 5: Create New Lambda Function from Container Image

### 5.1 Delete Lambda Function `BlurFacesFunction`

**If you created `BlurFacesFunction` in Step 3.4 (code-based version):**

1. Lambda Console → Functions
2. Select `BlurFacesFunction`
3. Actions → **Delete**
4. Type "delete" to confirm
5. Click **Delete**

**Reason:** Lambda function cannot be switched from code-based to container-based. Must create new.

### 5.2 Create New Lambda Function

1. Lambda Console → **Create function**

**Basic information:**
- **Function name:** `BlurFacesFunction` must match the old lambda name
- **Package type:** **Container image** 

**Container image:**
- Click **Browse images**
- Select repository: `blur-faces-lambda`
- Select image tag: `latest`
- Click **Select image**

**Architecture:**
- x86_64 (automatically set)

![image](/images/Image_workshop/StepFunc/lambda4-new.png)

**Permissions:**
- **Execution role:** Use an existing role
- Select: `LambdaBlurFacesRole` (created in Step 2.4)

![iamge](/images/Image_workshop/StepFunc/lambda4-role.png)

Click **Create function**

**Time:** 1-2 minutes

**Confirmation:**
- Function created successfully
- Code source displays: "Image: blur-faces-lambda:latest"
- Notification: "Successfully created the function BlurFacesFunction"

### 5.3 Configure Lambda Function

#### 5.3.1 Increase Memory and Timeout

**How to configure:**

1. Tab **Configuration** → **General configuration** → **Edit**

**Settings:**
- **Memory:** 3000 MB (maximum for Lambda)
- **Timeout:** 15 minutes (900 seconds)
- **Ephemeral storage:** 1024 MB (increased from 512 MB default)

![image](/images/Image_workshop/StepFunc/lambda4-cofig.png)

1. Click **Save**

**Explanation:**
- **Memory 3000 MB:** Processing HD video requires lots of RAM
- **Timeout 900s:** 2-3 minute video needs ~5-10 minutes to process
- **Storage 1024 MB:** Store temporary input + output video in /tmp

#### 5.3.2 Set Environment Variables

1. Tab **Configuration** → **Environment variables** → **Edit**
2. Click **Add environment variable**

**Variable:**
- **Key:** `OUTPUT_BUCKET`
- **Value:** `face-blur-output-bucket-****` (replace with your output bucket name)
![image](/images/Image_workshop/StepFunc/lambda4-enviro.png)

3. Click **Save**

**Checkpoint:** Lambda function with Docker container has been created and configured!
