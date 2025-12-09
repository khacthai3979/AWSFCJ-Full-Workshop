---
title : "Delete Services"
date :  "2025-10-10"
weight : 10
chapter : false
pre: " <b> 5.10 </b> "
---
### Step 1: Delete S3 Objects

**Must delete objects before deleting buckets!**

#### 1.1 Delete Objects in Input Bucket

    1. S3 Console → Buckets
    2. Click on `face-blur-input-bucket-input` (or your bucket name)
    3. Tab **Objects**
    4. **Select all** (checkbox at the top of the list)
    5. Click **Delete**
    6. Confirm:
    - Type: `permanently delete`
    - Click **Delete objects**

**Verify:** "Successfully deleted X objects"

#### 1.2 Delete Objects in Output Bucket

Repeat the above steps with `face-blur-output-bucket-output`

**Checkpoint:** Both buckets are empty (0 objects)

---

### Step 2: Delete S3 Event Notifications

#### 2.1 Delete Event Notifications

   1. S3 Console → Input bucket
   2. Tab **Properties**
   3. Scroll down to **Event notifications**
   4. You will see 2 notifications:
      - Event notification for `.mp4` files
      - Event notification for `.mov` files
   5. Select each notification → Click **Delete**
   6. Confirm deletion

**Checkpoint:** Event notifications = 0


### Step 3: Delete Lambda Functions

#### 3.1 Delete Lambda 4 (Blur Faces)

    1. Lambda Console → Functions
    2. Select `BlurFacesFunction`
    3. Actions → **Delete**
    4. Confirm:
    - Type: `delete`
    - Click **Delete**

#### 3.2 Delete Lambda 3 (Get Timestamps)

    Repeat with `GetTimestampsFunction`

#### 3.3 Delete Lambda 2 (Check Status)

Repeat with `CheckStatusFunction`

#### 3.4 Delete Lambda 1 (Start Face Detect)

Repeat with `StartFaceDetectFunction`

**Checkpoint:** 0 Lambda functions remaining

---

### Step 4: Delete Step Functions State Machine

1. Step Functions Console → State machines
2. Select `FaceBlurStateMachine`
3. Click **Delete**
4. Confirm:
   - Type: `delete`
   - Click **Delete state machine**

**Time:** ~30 seconds

**Checkpoint:** State machine has been deleted

---

### Step 5: Delete ECR Repository and Docker Images

#### 5.1 Delete Docker Images in ECR

1. ECR Console → Repositories
2. Click on `blur-faces-lambda`
3. Tab **Images**
4. Select all images (checkbox)
5. Click **Delete**
6. Confirm:
   - Type: `delete`
   - Click **Delete**

**Verify:** "Successfully deleted X images"

#### 5.2 Delete ECR Repository

1. ECR Console → Repositories
2. Select `blur-faces-lambda`
3. Click **Delete**
4. Confirm:
   - Type: `delete`
   - Click **Delete**

**Checkpoint:** ECR repository has been deleted

---

### Step 6: Delete S3 Buckets

**⚠️ Buckets must be empty before deletion!**

#### 6.1 Delete Input Bucket

1. S3 Console → Buckets
2. Select `face-blur-input-bucket-input`
3. Click **Delete**
4. Confirm:
   - Type bucket name: `face-blur-input-bucket-input`
   - Click **Delete bucket**

#### 6.2 Delete Output Bucket

Repeat with `face-blur-output-bucket-output`

**Checkpoint:** 0 buckets remaining (or only other buckets)

---

### Step 7: Delete IAM Roles

**Order:** Delete in any order (no dependencies)

#### 7.1 Delete Lambda Roles

1. IAM Console → Roles
2. Search: `Lambda`
3. Select the following roles (can select multiple):
   - `LambdaStartFaceDetectRole`
   - `LambdaCheckStatusRole`
   - `LambdaGetTimestampsRole`
   - `LambdaBlurFacesRole`
4. Click **Delete**
5. Confirm:
   - Type role name
   - Click **Delete**

**Repeat for each role if you cannot delete multiple at once.**

#### 7.2 Delete Step Functions Role

1. IAM Console → Roles
2. Search: `StepFunctions`
3. Select `StepFunctionsExecutionRole`
4. Click **Delete**
5. Confirm deletion

**Checkpoint:** 5 IAM roles have been deleted

---

### Step 8: Delete CloudWatch Log Groups (Optional)

**Note:** Log groups will be automatically deleted after 30 days if no new logs. You can keep them for later review.

#### 8.1 Delete Lambda Log Groups

1. CloudWatch Console → Log groups
2. Search: `/aws/lambda/`
3. Select the log groups:
   - `/aws/lambda/StartFaceDetectFunction`
   - `/aws/lambda/CheckStatusFunction`
   - `/aws/lambda/GetTimestampsFunction`
   - `/aws/lambda/BlurFacesFunction`
4. Actions → **Delete log group(s)**
5. Confirm deletion

#### 8.2 Delete Step Functions Log Groups (If any)

Search: `/aws/states/` and delete similarly

**Checkpoint:** Log groups have been deleted

---

### Step 9: Delete Local Docker Images (Optional)

**To free up disk space on your local machine:**

#### 9.1 List Docker Images

```powershell
# Windows/macOS/Linux
docker images | grep blur-faces-lambda
```

You will see:
```
blur-faces-lambda                                                latest    abc123    1.2GB
208613876063.dkr.ecr.ap-southeast-1.amazonaws.com/blur-faces-lambda   latest    abc123    1.2GB
```

#### 9.2 Delete Images

```powershell
# Delete local image
docker rmi blur-faces-lambda:latest

# Delete ECR tagged image
docker rmi 123456789.dkr.ecr.ap-southeast-1.amazonaws.com/blur-faces-lambda:latest
```

**Or delete all unused images:**
```powershell
docker image prune -a
```

**Verify:**
```powershell
docker images | grep blur-faces-lambda
# No results
```
