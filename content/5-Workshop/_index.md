---
title : "Workshop"
date :  "2025-10-10" 
weight : 5 
chapter : false
pre: " <b> 5. </b> "
---
# IMPLEMENTING ADVANCED FEATURES OF AWS APPLICATION LOAD BALANCER

### Introduction

As AI/ML grows increasingly popular, users and businesses are becoming more concerned about the protection of personal data. In Computer Vision (CV) applications, one of the most effective approaches to protect sensitive information is to blur faces in images and videos.

This process involves two main steps:
1. Detecting faces in each frame using ML/Deep Learning.
2. Masking the detected face region using image processing.

In this workshop, we build a fully serverless application that automatically detects and blurs faces in videos using:

- Amazon Rekognition Video for face detection  
- AWS Step Functions to orchestrate the workflow  
- AWS Lambda (Container Image) and OpenCV for video processing  

This solution is suitable for systems that require privacy protection, compliance with data protection regulations, or large-scale video processing.

![ConnectPrivate](/images/Image_workshop/blur_face_stepfunc.drawio.png) 

## Solution Overview

The overall architecture is built with **100% serverless services**, ensuring:

- **No infrastructure management**
- **Automatic scaling based on demand**
- **Built-in ML APIs from Amazon Rekognition**
- **No deep learning expertise required**

---

## Processing Flow

1. **Upload Video**
   - The user uploads a video file to Amazon S3.

2. **S3 Event Trigger**
   - When the object is created, S3 emits an `ObjectCreated` event that triggers the **Start Face Detection** Lambda function.

3. **Face Detection Job**
   - The Lambda function asynchronously invokes the **Amazon Rekognition Video** API to perform face detection.

4. **Start Workflow**
   - The Lambda function starts the **AWS Step Functions State Machine**, passing:
     - S3 Object Key  
     - Video Metadata  
     - Rekognition Job ID  

---

## Step Functions Workflow

### 1. Wait for Completion
- Step Functions invokes the **Wait for Completion** Lambda function to wait for Rekognition to finish processing.

### 2. Retrieve Detection Results
- After the job completes, Step Functions calls another Lambda function to fetch:
  - **Bounding Boxes**
  - **Timestamps**

### 3. Video Blurring
- The final Lambda function uses OpenCV to blur the detected faces:
  - Pull the Docker image with OpenCV support from **Amazon ECR**
  - Read the input video from **S3**
  - Apply blur to faces based on `timestamp` and `bounding box`
  - Upload the processed video to the **output S3 bucket**
  - Remove temporary files inside the Lambda environment

---

## Output

- The blurred video is stored in the **output S3 bucket** and is ready to be shared with downstream systems or end users.

---

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
