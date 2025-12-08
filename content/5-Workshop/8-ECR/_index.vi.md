---
title : "Thêm OpenCV cho Lambda 4 - Sử dụng Container Image"
date : "2025-10-10"
weight : 2
chapter : false
pre : " <b> 8. </b> "
---

Ở phần trước, Lambda 4 chỉ copy video từ input sang output mà không blur. Bây giờ chúng ta sẽ deploy Lambda function với Docker container để **thực sự blur faces** trong video bằng OpenCV và MoviePy.

**Yêu cầu trước khi bắt đầu:**
- Docker Desktop đã cài đặt và đang chạy
- AWS CLI đã cấu hình với credentials
- Đã hoàn thành các bước 1-5 ở trên (S3, IAM, Lambda, Step Functions)

**Những gì sẽ làm:**
1. Tạo 4 files code (Dockerfile, requirements.txt, app.py, video_processor.py)
2. Tạo ECR repository để lưu Docker image
3. Build Docker image trên máy local
4. Push image lên AWS ECR
5. Update Lambda function để dùng container image
6. Test và verify kết quả

---

## Bước 1: Chuẩn bị Files Code

### 1.1 Tạo Thư mục Project

**Windows:**
```powershell
mkdir C:\AWS\lambda-blur-docker
cd C:\AWS\lambda-blur-docker
```


### 1.2 Tạo File: Dockerfile

Tạo file tên `Dockerfile` (không có extension) với nội dung:

```dockerfile
FROM public.ecr.aws/lambda/python:3.8

RUN yum install -y mesa-libGL

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY video_processor.py .
COPY app.py .

CMD ["app.lambda_function"]
```

**Giải thích:**
- `FROM`: Base image từ AWS Lambda Python 3.8
- `RUN yum install`: Cài OpenGL library cho OpenCV
- `COPY requirements.txt`: Copy file dependencies
- `RUN pip install`: Cài các Python packages
- `COPY app.py`: Copy code xử lý
- `CMD`: Entry point khi Lambda chạy

### 1.3 Tạo File: requirements.txt

Tạo file `requirements.txt` với nội dung:

```txt
boto3==1.18.6
botocore==1.21.6
opencv-python-headless==4.5.3.56
numpy==1.22.0
moviepy==1.0.3
imageio==2.9.0
imageio-ffmpeg==0.4.4
```

**Giải thích:**
- `boto3`: AWS SDK để tương tác với S3
- `opencv-python-headless`: Xử lý video và blur faces (không cần GUI)
- `numpy`: Tính toán array cho OpenCV
- `moviepy`: Xử lý audio và video
- `imageio`: Đọc/ghi video files

### 1.4 Tạo File: video_processor.py

Tạo file `video_processor.py` với nội dung đầy đủ từ code đã có:

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

### 1.5 Tạo File: app.py

Tạo file `app.py` với nội dung:

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

Kiểm tra bạn có đủ 4 files:

```powershell
# Windows
dir
```

Bạn sẽ thấy:
```
Dockerfile
requirements.txt
app.py
video_processor.py
```

---

## Bước 2: Tạo ECR Repository

### 1.1 Vào ECR Console

1. AWS Console → Tìm kiếm **"ECR"** (Elastic Container Registry)
2. Click **"Get Started"** hoặc **"Create repository"**

### 1.2 Cấu hình Repository

**Repository settings:**
- **Visibility:** Private
- **Repository name:** `blur-faces-lambda`
- **Tag immutability:** Disabled
- **Scan on push:** Disabled (optional)
- **Encryption:** AES-256 (default)

Click **"Create repository"**

### 1.3 Lưu Repository URI

Sau khi tạo xong, bạn sẽ thấy:
```
Repository URI: 123456789012.dkr.ecr.us-east-1.amazonaws.com/blur-faces-lambda
```

**Lưu lại URI này!** Bạn sẽ cần nó ở bước sau.

---

## Bước 3: Build Docker Image

### 3.1 Kiểm tra Docker Desktop

**Trước khi build, đảm bảo Docker Desktop đang chạy:**

**Windows:**
1. Mở Docker Desktop từ Start Menu
2. Đợi cho đến khi thấy "Docker Desktop is running"
3. Icon Docker ở system tray sẽ không còn animation


**Verify Docker hoạt động:**
```powershell
docker --version
```

Bạn sẽ thấy:
```
Docker version 24.0.x, build xxxxx
```

### 3.2 Build Image với Platform Đúng

**⚠️ QUAN TRỌNG:** Lambda chạy trên Linux AMD64, nên phải build với platform này!

**Windows PowerShell:**
```powershell
cd C:\AWS\lambda-blur-docker
docker build --platform linux/amd64 -t blur-faces-lambda .
```


**Giải thích:**
- `--platform linux/amd64`: Build cho Linux AMD64 (Lambda architecture)


**Thời gian:** 5-10 phút (lần đầu download base image và dependencies)

### 3.3 Verify Image

**Kiểm tra image đã build thành công:**

**Windows:**
```powershell
docker images | findstr blur-faces-lambda
```

## Bước 4: Push Image lên AWS ECR

### 4.1 Lấy AWS Account ID

**Chạy lệnh để lấy Account ID:**
```powershell
aws sts get-caller-identity --query Account --output text
```

**Output:**
```
VD: 123456789
```

**Lưu lại số này!** Đây là AWS Account ID của bạn.

### 4.2 Set Variables (Để dễ dùng)

**Windows PowerShell:**
```powershell
$ACCOUNT_ID = "123456789"
$REGION = "ap-southeast-1"
$REPO_NAME = "blur-faces-lambda"
```


**Giải thích:**
- `ACCOUNT_ID`: AWS Account ID của bạn
- `REGION`: Region bạn đang dùng (ap-southeast-1 = Singapore)
- `REPO_NAME`: Tên ECR repository (phải trùng với tên đã tạo ở Bước 2)

### 4.3 Login vào ECR

**Windows PowerShell:**
```powershell
aws ecr get-login-password --region $REGION | docker login --username AWS --password-stdin "$ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com"
```

**Output thành công:**
```
Login Succeeded
```

**Nếu gặp lỗi:**

**Lỗi 1: AWS CLI not configured**
```
Unable to locate credentials
```
→ Chạy `aws configure` và nhập Access Key, Secret Key

**Lỗi 2: Region not found**
```
Could not connect to the endpoint URL
```
→ Check region name đúng chưa (ap-southeast-1, us-east-1, etc.)

### 4.4 Tag Image cho ECR

**Windows PowerShell:**
```powershell
docker tag blur-faces-lambda:latest "$ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com/$REPO_NAME:latest"
```



**Verify tag:**
```powershell
docker images | findstr blur-faces-lambda
```

**Bạn sẽ thấy 2 dòng:**
```
blur-faces-lambda                                                latest    abc123    2 mins ago    1.2GB
208613876063.dkr.ecr.ap-southeast-1.amazonaws.com/blur-faces-lambda   latest    abc123    2 mins ago    1.2GB
```

### 4.5 Push Image lên ECR

**Windows PowerShell:**
```powershell
docker push "$ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com/$REPO_NAME:latest"
```

**Thời gian:** 5-10 phút (upload ~2GB)



**Nếu gặp lỗi:**

**Lỗi 1: Repository not found**
```
repository does not exist or may require authorization
```
→ Check tên repository đúng chưa? Đã tạo ECR repository chưa?

**Lỗi 2: Authentication token expired**
```
authorization token has expired
```
→ Login lại: Chạy lại lệnh `aws ecr get-login-password...`

**Lỗi 3: Image manifest not supported**
```
The image manifest, config or layer media type is not supported
```
→ Rebuild image với `--platform linux/amd64`

### 4.6 Verify trên AWS Console

**Kiểm tra image đã lên ECR:**

1. AWS Console → ECR
2. Click vào repository `blur-faces-lambda`
3. Tab **Images**
4. Bạn sẽ thấy:
   - **Image tag:** latest
   - **Image size:** ~1.2 GB
   - **Pushed at:** Thời gian vừa push
   - **Image URI:** `208613876063.dkr.ecr.ap-southeast-1.amazonaws.com/blur-faces-lambda:latest`

5. **Copy Image URI** - bạn sẽ cần nó ở bước tiếp theo!

**Checkpoint:** Image đã được push lên ECR thành công!

---

## Bước 5: Tạo Lambda Function Mới từ Container Image

### 5.1 Xóa Lambda Function `BlurFacesFunction`

**Nếu bạn đã tạo `BlurFacesFunction` ở Bước 3.4 (version code-based):**

1. Lambda Console → Functions
2. Select `BlurFacesFunction`
3. Actions → **Delete**
4. Type "delete" để confirm
5. Click **Delete**

**Lý do:** Lambda function không thể chuyển từ code-based sang container-based. Phải tạo mới.

### 5.2 Tạo Lambda Function Mới

1. Lambda Console → **Create function**

**Basic information:**
- **Function name:** `BlurFacesFunction` tên phải giống với tên lambda cũ
- **Package type:**  **Container image** 

**Container image:**
- Click **Browse images**
- Select repository: `blur-faces-lambda`
- Select image tag: `latest`
- Click **Select image**

**Architecture:**
- x86_64 (đã được set tự động)

**Permissions:**
- **Execution role:** Use an existing role
- Select: `LambdaBlurFacesRole` (đã tạo ở Bước 2.4)

Click **Create function**

**Thời gian:** 1-2 phút

**Xác nhận:**
- Function được tạo thành công
- Code source hiển thị: "Image: blur-faces-lambda:latest"
- Có thông báo: "Successfully created the function BlurFacesFunction"

### 5.3 Cấu hình Lambda Function

#### 5.3.1 Tăng Memory và Timeout

**Cách cấu hình:**

1. Tab **Configuration** → **General configuration** → **Edit**

**Settings:**
- **Memory:** 3008 MB (maximum cho Lambda)
- **Timeout:** 15 minutes (900 seconds)
- **Ephemeral storage:** 1024 MB (tăng từ 512 MB default)

2. Click **Save**

**Giải thích:**
- **Memory 3008 MB:** Xử lý video HD cần nhiều RAM
- **Timeout 900s:** Video 2-3 phút cần ~5-10 phút để xử lý
- **Storage 1024 MB:** Lưu video input + output tạm trong /tmp

#### 5.3.2 Set Environment Variables

1. Tab **Configuration** → **Environment variables** → **Edit**
2. Click **Add environment variable**

**Variable:**
- **Key:** `OUTPUT_BUCKET`
- **Value:** `face-blur-output-bucket-****` (thay bằng tên output bucket của bạn)

3. Click **Save**

**✅ Checkpoint:** Lambda function với Docker container đã được tạo và cấu hình!
