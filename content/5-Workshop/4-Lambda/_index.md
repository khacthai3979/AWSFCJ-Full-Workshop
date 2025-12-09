---
title : "Initialize Lambda Functions"
date : "2025-10-10"
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---


### 3.1 Create Lambda 1: Start Face Detect

  1. Search for "Lambda" in the Console
  2. Click **"Create function"**

  **Basic information:**
  - **Function name:** `StartFaceDetectFunction`
  - **Runtime:** Python 3.9
  - **Architecture:** x86_64
  - **Execution role:** Use an existing role
    - Select: `LambdaStartFaceDetectRole`
  - **"Create function"**

  **Code:**
  1. In the Code source section, delete the default code
  2. Paste the following code:

```python
import json
import logging
import os
import urllib.parse
import boto3

logger = logging.getLogger()
logger.setLevel(logging.INFO)

reko = boto3.client('rekognition')
s3 = boto3.client('s3')
sfn = boto3.client('stepfunctions')

def lambda_handler(event, context):
    state_machine_arn = os.environ.get('STATE_MACHINE_ARN')
    
    for record in event['Records']:
        try:
            bucket = record['s3']['bucket']['name']
            key = urllib.parse.unquote_plus(record['s3']['object']['key'])
            size = int(record['s3']['object']['size'])
            filename = key.split('/')[-1]
            
            # Check format
            if not filename.lower().endswith(('.mp4', '.mov')):
                logger.error('Unsupported file type')
                continue
            
            # Check size (10GB limit)
            if size >= 10 * 1024 * 1024 * 1024:
                logger.error('File too large')
                continue
            
            # Start Rekognition
            response = reko.start_face_detection(
                Video={'S3Object': {'Bucket': bucket, 'Name': key}}
            )
            job_id = response['JobId']
            
            # Start Step Functions
            input_data = json.dumps({
                "body": {
                    "job_id": job_id,
                    "s3_object_bucket": bucket,
                    "s3_object_key": key
                }
            })
            
            sfn.start_execution(
                stateMachineArn=state_machine_arn,
                input=input_data
            )
            
            logger.info(f'Started processing: {job_id}')
            
        except Exception as e:
            logger.error(f'Error: {str(e)}')
            continue
    
    return {
        'statusCode': 200,
        'body': json.dumps('Processing started')
    }
```

3. Click **"Deploy"**

**Configuration:**
1. Tab **"Configuration"** → **"General configuration"** → **"Edit"**
   - **Timeout:** 10 minutes (600 seconds)
   - **Memory:** 512 MB
   - Click **"Save"**

2. Tab **"Configuration"** → **"Environment variables"** → **"Edit"**
   - Click **"Add environment variable"**
   - **Key:** `STATE_MACHINE_ARN`
   - **Value:** `PLACEHOLDER` (will be updated after creating Step Functions)
   - Click **"Save"**

**Save:** ARN of the Lambda function


### 3.2 Create Lambda 2: Check Status

**Create function:**
- **Function name:** `CheckStatusFunction`
- **Runtime:** Python 3.9
- **Execution role:** `LambdaCheckStatusRole`

**Code:**
```python
import json
import boto3

reko = boto3.client('rekognition')

def lambda_handler(event, context):
    job_id = event['job_id']
    
    response = reko.get_face_detection(
        JobId=job_id,
        MaxResults=100
    )
    
    return {
        'statusCode': 200,
        'body': {
            'job_id': job_id,
            'job_status': response['JobStatus'],
            's3_object_bucket': event['s3_object_bucket'],
            's3_object_key': event['s3_object_key']
        }
    }
```

**Configuration:**
- **Timeout:** 10 minutes
- **Memory:** 512 MB

Click **"Deploy"**

### 3.3 Create Lambda 3: Get Timestamps

**Create function:**
- **Function name:** `GetTimestampsFunction`
- **Runtime:** Python 3.9
- **Execution role:** `LambdaGetTimestampsRole`

**Code:**
```python
import json
import boto3

reko = boto3.client('rekognition')

def lambda_handler(event, context):
    job_id = event['job_id']
    
    final_timestamps = {}
    next_token = ""
    first_round = True
    
    while True:
        if first_round:
            response = reko.get_face_detection(
                JobId=job_id,
                MaxResults=100
            )
            first_round = False
        else:
            response = reko.get_face_detection(
                JobId=job_id,
                MaxResults=100,
                NextToken=next_token
            )
        
        # Process faces
        for face in response.get('Faces', []):
            f = face['Face']['BoundingBox']
            t = str(face['Timestamp'])
            
            if t not in final_timestamps:
                final_timestamps[t] = []
            final_timestamps[t].append(f)
        
        # Check for more pages
        if 'NextToken' in response:
            next_token = response['NextToken']
        else:
            break
    
    return {
        'statusCode': 200,
        'body': {
            'job_id': job_id,
            'response': response,
            's3_object_bucket': event['s3_object_bucket'],
            's3_object_key': event['s3_object_key'],
            'timestamps': final_timestamps
        }
    }
```

**Configuration:**
- **Timeout:** 10 minutes
- **Memory:** 512 MB

Click **"Deploy"**


### 3.4 Create Lambda 4: Blur Faces (Simplified Version)

**⚠️ Note:** This Lambda function requires OpenCV and MoviePy. There are 2 approaches:

**Approach 1: Use Lambda Layer (Simpler)**

1. Download pre-built layer for OpenCV:
   - Search for "opencv lambda layer" on Google
   - Or use: https://github.com/iandow/opencv_aws_lambda

2. Upload layer to Lambda

**Approach 2: Simplified version without OpenCV (For testing)**

**Create function:**
- **Function name:** `BlurFacesFunction`
- **Runtime:** Python 3.9
- **Execution role:** `LambdaBlurFacesRole`

**Code (Simplified - for testing workflow only):**
```python
import json
import os
import boto3

s3 = boto3.client('s3')
output_bucket = os.environ.get('OUTPUT_BUCKET')

def lambda_handler(event, context):
    try:
        bucket = event['s3_object_bucket']
        key = event['s3_object_key']
        timestamps = event.get('timestamps', {})
        
        print(f'Processing video: {key}')
        print(f'Found {len(timestamps)} timestamps with faces')
        
        # Download video
        local_input = f'/tmp/{key.split("/")[-1]}'
        s3.download_file(bucket, key, local_input)
        
        # TODO: Apply blur with OpenCV
        # For now, just copy the file
        local_output = f'/tmp/blurred-{key.split("/")[-1]}'
        
        # Simulate processing
        import shutil
        shutil.copy(local_input, local_output)
        
        # Upload to output bucket
        s3.upload_file(local_output, output_bucket, key)
        
        return {
            'statusCode': 200,
            'body': json.dumps('Video processed successfully')
        }
        
    except Exception as e:
        print(f'Error: {str(e)}')
        raise e
```

**Configuration:**
- **Timeout:** 10 minutes
- **Memory:** 2048 MB (requires more RAM for video processing)

**Environment variables:**
- **Key:** `OUTPUT_BUCKET`
- **Value:** `face-blur-output-bucket-[your-name]`

Click **"Deploy"**

**Checkpoint:** You have 4 Lambda functions
