---
title : "Tạo IAM Roles"
date : "2025-10-10"
weight : 2
chapter : false
pre : " <b> 3. </b> "
---

## Tạo Role cho Labda
### Role cho Lambda - Start Face Detect

1. Tìm kiếm "IAM" trong Console
2. Sidebar: Click **"Roles"** → **"Create role"**

  **Step 1: Select trusted entity**
  - **Trusted entity type:** AWS service
  - **Use case:** Lambda
  - Click **"Next"**
  ![image](static\images\Image_workshop\IAM\3-chon aws service.png)

  **Step 2: Add permissions** 
  Tìm và chọn các policies sau:
  - `AWSLambdaBasicExecutionRole` (cho CloudWatch Logs)
  - Click **"Next"**

  **Step 3: Name, review, and create**
  - **Role name:** `LambdaStartFaceDetectRole`
  - **Description:** Role for Start Face Detect Lambda
  - Click **"Create role"**

  **Step 4: Add inline policies**
  1. Tìm role vừa tạo trong danh sách
  2. Click vào role name
  3. Tab **"Permissions"** → **"Add permissions"** → **"Create inline policy"**
  4. Click tab **"JSON"**
  5. Paste policy sau:
```json
    {
    "Version": "2012-10-17",
    "Statement": [
        {
        "Effect": "Allow",
        "Action": [
            "s3:GetObject",
            "s3:PutObject"
        ],
        "Resource": [
            "arn:aws:s3:::face-blur-input-bucket-[your-name]/*"
        ]
        },
        {
        "Effect": "Allow",
        "Action": [
            "rekognition:StartFaceDetection"
        ],
        "Resource": "*"
        },
        {
        "Effect": "Allow",
        "Action": [
            "states:StartExecution"
        ],
        "Resource": "*"
        }
    ]
    }
```

6. Click **"Review policy"**
7. **Policy name:** `StartFaceDetectPolicy`
8. Click **"Create policy"**

**Lưu lại:** ARN của role (ví dụ: `arn:aws:iam::123456789012:role/LambdaStartFaceDetectRole`)


### Role cho Lambda - Check Status

Lặp lại quy trình tạo role:

**Basic info:**
- **Role name:** `LambdaCheckStatusRole`
- **Trusted entity:** Lambda
- **Managed policy:** `AWSLambdaBasicExecutionRole`

**Inline policy JSON:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "rekognition:GetFaceDetection"
      ],
      "Resource": "*"
    }
  ]
}
```

**Policy name:** `CheckStatusPolicy`

### Role cho Lambda - Get Timestamps

**Basic info:**
- **Role name:** `LambdaGetTimestampsRole`
- **Trusted entity:** Lambda
- **Managed policy:** `AWSLambdaBasicExecutionRole`

**Inline policy JSON:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "rekognition:GetFaceDetection"
      ],
      "Resource": "*"
    }
  ]
}
```

**Policy name:** `GetTimestampsPolicy`

### Role cho Lambda - Blur Faces

**Basic info:**
- **Role name:** `LambdaBlurFacesRole`
- **Trusted entity:** Lambda
- **Managed policy:** `AWSLambdaBasicExecutionRole`

**Inline policy JSON:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": [
        "arn:aws:s3:::face-blur-input-bucket-[your-name]/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:s3:::face-blur-output-bucket-[your-name]/*"
      ]
    }
  ]
}
```

**Policy name:** `BlurFacesPolicy`

### Role cho Step Functions

1. IAM → Roles → **"Create role"**

**Step 1: Select trusted entity**
- **Trusted entity type:** AWS service
- **Use case:** Step Functions
- Click **"Next"**

**Step 2: Add permissions**
- Không cần chọn policy nào
- Click **"Next"**

**Step 3: Name and create**
- **Role name:** `StepFunctionsExecutionRole`
- Click **"Create role"**

**Step 4: Add inline policy**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "lambda:InvokeFunction"
      ],
      "Resource": "*"
    }
  ]
}
```

**Policy name:** `StepFunctionsInvokePolicy`

**Checkpoint:** Bạn có 5 IAM Roles