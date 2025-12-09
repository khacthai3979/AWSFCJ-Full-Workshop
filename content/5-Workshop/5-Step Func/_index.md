---
title : "Create Step Functions State Machine"
date : "2025-10-10"
weight : 5
chapter : false
pre : " <b> 5.5 </b> "
---


### 4.1 Create State Machine

1. Search for "Step Functions" in the Console
2. Click **"Create state machine"**

**Choose authoring method:**
- Select: **"Write your workflow in code"**
- **Type:** Standard
- Click **"Next"**

### 4.2 Define State Machine

**Definition:** Paste the following JSON (replace Lambda function ARNs)

```json
{
  "Comment": "Face Blur Video Processing Workflow",
  "StartAt": "CheckJobStatus",
  "States": {
    "CheckJobStatus": {
      "Type": "Task",
      "Resource": "arn:aws:states:::lambda:invoke",
      "Parameters": {
        "FunctionName": "arn:aws:lambda:REGION:ACCOUNT_ID:function:CheckStatusFunction",
        "Payload": {
          "job_id.$": "$.body.job_id",
          "s3_object_bucket.$": "$.body.s3_object_bucket",
          "s3_object_key.$": "$.body.s3_object_key"
        }
      },
      "ResultPath": "$.checkResult",
      "ResultSelector": {
        "body.$": "$.Payload.body"
      },
      "Next": "JobFinished"
    },
    "JobFinished": {
      "Type": "Choice",
      "Choices": [
        {
          "Variable": "$.checkResult.body.job_status",
          "StringEquals": "IN_PROGRESS",
          "Next": "Wait1Second"
        },
        {
          "Variable": "$.checkResult.body.job_status",
          "StringEquals": "SUCCEEDED",
          "Next": "GetTimestampsAndFaces"
        }
      ],
      "Default": "ExecutionFailed"
    },
    "Wait1Second": {
      "Type": "Wait",
      "Seconds": 1,
      "Next": "CheckJobStatus"
    },
    "GetTimestampsAndFaces": {
      "Type": "Task",
      "Resource": "arn:aws:states:::lambda:invoke",
      "Parameters": {
        "FunctionName": "arn:aws:lambda:REGION:ACCOUNT_ID:function:GetTimestampsFunction",
        "Payload": {
          "job_id.$": "$.checkResult.body.job_id",
          "s3_object_bucket.$": "$.checkResult.body.s3_object_bucket",
          "s3_object_key.$": "$.checkResult.body.s3_object_key"
        }
      },
      "ResultPath": "$.timestampResult",
      "ResultSelector": {
        "body.$": "$.Payload.body"
      },
      "Next": "BlurFacesOnVideo"
    },
    "BlurFacesOnVideo": {
      "Type": "Task",
      "Resource": "arn:aws:states:::lambda:invoke",
      "Parameters": {
        "FunctionName": "arn:aws:lambda:REGION:ACCOUNT_ID:function:BlurFacesFunction",
        "Payload": {
          "job_id.$": "$.timestampResult.body.job_id",
          "response.$": "$.timestampResult.body.response",
          "s3_object_bucket.$": "$.timestampResult.body.s3_object_bucket",
          "s3_object_key.$": "$.timestampResult.body.s3_object_key",
          "timestamps.$": "$.timestampResult.body.timestamps"
        }
      },
      "Next": "ExecutionSucceeded"
    },
    "ExecutionSucceeded": {
      "Type": "Succeed"
    },
    "ExecutionFailed": {
      "Type": "Fail",
      "Cause": "Face Detection Failed",
      "Error": "JobStatusFailed"
    }
  }
}
```

**How to get Lambda ARN:**
1. Go to Lambda Console
2. Click on the function
3. Copy ARN from the top right corner
4. Replace `REGION` and `ACCOUNT_ID` in the JSON

```
arn:aws:lambda:us-east-1:123456789012:function:CheckStatusFunction
```

**:**
![imagestate](/images/Image_workshop/StepFunc/4-config.png)

### 4.3 Configure State Machine

**Specify details:**
- **Name:** `FaceBlurStateMachine`
- **Permissions:** Choose an existing role
  - Select: `StepFunctionsExecutionRole`
![image](/images/Image_workshop/StepFunc/6-config-2.png)
- **Logging:** (Optional) Enable logging
  - **Log level:** ALL
  - **Include execution data:** Checked

Click **"Create state machine"**    



### 4.4 Save State Machine ARN

1. After creation, copy the State Machine ARN
2. Example: `arn:aws:states:us-east-1:123456789012:stateMachine:FaceBlurStateMachine`

### 4.5 Update Lambda 1 Environment Variable

1. Return to Lambda Console
2. Click on `StartFaceDetectFunction`
3. Tab **"Configuration"** → **"Environment variables"** → **"Edit"**
4. Update the `STATE_MACHINE_ARN` variable:
   - **Value:** Paste the ARN of the newly created State Machine
5. Click **"Save"**

![image](/images/Image_workshop/StepFunc/7-enviroment-var.png)

**Checkpoint:** State Machine has been created and linked with Lambda 1
