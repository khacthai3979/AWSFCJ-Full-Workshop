---
title : "Create IAM Roles"
date : "2025-10-10"
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

### 2.1 Role for Lambda - Start Face Detect

1. Search for "IAM" in the Console
2. Sidebar: Click **"Roles"** → **"Create role"**

  **Step 1: Select trusted entity**
  - **Trusted entity type:** AWS service
  - **Use case:** Lambda
  - Click **"Next"**

![lambda](/images/Image_workshop/IAM/4-usecase-chon-lambda.png)

  **Step 2: Add permissions** 
  Search and select the following policies:
  - `AWSLambdaBasicExecutionRole` (for CloudWatch Logs)
  - Click **"Next"**


  **Step 3: Name, review, and create**
  - **Role name:** `LambdaStartFaceDetectRole`
  - **Description:** Role for Start Face Detect Lambda
  - Click **"Create role"**

  **Step 4: Add inline policies**
  1. Find the newly created role in the list
  2. Click on the role name
  3. Tab **"Permissions"** → **"Add permissions"** → **"Create inline policy"**
  4. Click the **"JSON"** tab
  5. Paste the following policy:
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

1. Click **"Review policy"**
2. **Policy name:** `StartFaceDetectPolicy`
3. Click **"Create policy"**

![rolestartfacedetec](/images/Image_workshop/IAM/11-StartFaceDetectPolicy.png)

**Review Role for Lambda - Start Face Detect**

![reviewpolicy](/images/Image_workshop/IAM/review-startface.png)

**Save:** ARN of the role (e.g., `arn:aws:iam::123456789012:role/LambdaStartFaceDetectRole`)


### 2.2 Role for Lambda - Check Status

Repeat the role creation process:

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

![checkstatus](/images/Image_workshop/IAM/policy-status.png)

**Review Role for Lambda - Check Status**

![reviewstatus](/images/Image_workshop/IAM/review-status.png)

### 2.3 Role for Lambda - Get Timestamps

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
![checkgettimes](/images/Image_workshop/IAM/policy-gettimes.png)

**Review Role for Lambda - Get Timestamps**
![reviewgettimes](/images/Image_workshop/IAM/review-gettimes.png)

### 2.4 Role for Lambda - Blur Faces

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
![checkblur](/images/Image_workshop/IAM/policy-blur.png)

**Review Role for Lambda - Blur Faces**
![reviewblur](/images/Image_workshop/IAM/review-blur.png)

### 2.5 Role for Step Functions

1. IAM → Roles → **"Create role"**

**Step 1: Select trusted entity**
- **Trusted entity type:** AWS service
- **Use case:** Step Functions
- Click **"Next"**

**Step 2: Add permissions**
- No need to select any policy
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

**Step 5: Add another inline policy**

```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"logs:CreateLogDelivery",
				"logs:GetLogDelivery",
				"logs:UpdateLogDelivery",
				"logs:DeleteLogDelivery",
				"logs:ListLogDeliveries",
				"logs:PutResourcePolicy",
				"logs:DescribeResourcePolicies",
				"logs:DescribeLogGroups"
			],
			"Resource": "*"
		}
	]
}
```

**Policy name:** `StepFunctionsExecutionRole`

**Review Role for Step Functions**
![reviewstepfunc](/images/Image_workshop/IAM/review-step.png)

**Checkpoint:** You have 5 IAM Roles
