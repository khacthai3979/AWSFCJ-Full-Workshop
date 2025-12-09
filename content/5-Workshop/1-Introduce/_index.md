---
title : "Introduction"
date : "2025-10-10"
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---
In this workshop, we will build a project to automatically blur faces in videos by combining Amazon Rekognition and OpenCV. This is a practical application in the Computer Vision field, especially important as businesses increasingly focus on protecting user privacy.

By using AWS Step Functions to orchestrate the workflow and Lambda to process each step, we will call the Amazon Rekognition Video service to detect faces in videos, then use the OpenCV library to blur those faces.

This workshop helps students and mentors understand the process of building a serverless video processing system, while exploring how to integrate AWS AI/ML services into real-world applications.

### Learning Objectives

After completing this workshop, participants will:

- Understand the process of detecting faces in videos using Amazon Rekognition.
- Learn how to use AWS Step Functions to orchestrate multiple Lambda functions.
- Know how to integrate Amazon Rekognition Video with OpenCV to blur faces.
- Become familiar with serverless architecture and automated video processing pipelines.
- Practice deploying an AI/ML solution on AWS.
- Understand the importance of anonymizing image data and protecting privacy.

### Reference Documentation

**AWS Documentation:**
- [Lambda Container Images](https://docs.aws.amazon.com/lambda/latest/dg/images-create.html)
- [Amazon ECR User Guide](https://docs.aws.amazon.com/ecr/)
- [Rekognition Video Analysis](https://docs.aws.amazon.com/rekognition/latest/dg/video.html)

**Libraries:**
- [OpenCV Documentation](https://docs.opencv.org/)
- [MoviePy Documentation](https://zulko.github.io/moviepy/)
- [Boto3 Documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)

**Tools:**
- [Docker Documentation](https://docs.docker.com/)
- [AWS CLI Reference](https://docs.aws.amazon.com/cli/)