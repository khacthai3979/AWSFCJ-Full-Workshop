---
title : "Tạo Step Functions State Machine"
date : "2025-10-10"
weight : 2
chapter : false
pre : " <b> 5. </b> "
---

## 4. Tạ
### 4.1 Tạo State Machine

1. Tìm kiếm "Step Functions" trong Console
2. Click **"Create state machine"**

**Choose authoring method:**
- Select: **"Write your workflow in code"**
- **Type:** Standard
- Click **"Next"**

### 4.2 Define State Machine

**Definition:** Paste JSON sau (thay thế ARNs của Lambda functions)

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

**Cách lấy Lambda ARN:**
1. Vào Lambda Console
2. Click vào function
3. Copy ARN ở góc trên bên phải
4. Thay thế `REGION` và `ACCOUNT_ID` trong JSON

**Ví dụ ARN:**
```
arn:aws:lambda:us-east-1:123456789012:function:CheckStatusFunction
```


### 4.3 Configure State Machine

**Specify details:**
- **Name:** `FaceBlurStateMachine`
- **Permissions:** Choose an existing role
  - Select: `StepFunctionsExecutionRole`
- **Logging:** (Optional) Enable logging
  - **Log level:** ALL
  - **Include execution data:** Checked

Click **"Create state machine"**    

### 4.4 Lưu State Machine ARN

1. Sau khi tạo xong, copy ARN của State Machine
2. Ví dụ: `arn:aws:states:us-east-1:123456789012:stateMachine:FaceBlurStateMachine`

### 4.5 Update Lambda 1 Environment Variable

1. Quay lại Lambda Console
2. Click vào `StartFaceDetectFunction`
3. Tab **"Configuration"** → **"Environment variables"** → **"Edit"**
4. Update biến `STATE_MACHINE_ARN`:
   - **Value:** Paste ARN của State Machine vừa tạo
5. Click **"Save"**

**Checkpoint:** State Machine đã được tạo và linked với Lambda 1
