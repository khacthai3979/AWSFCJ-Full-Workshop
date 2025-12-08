---
title : "Xóa các dịch vụ"
date :  "2025-10-10"
weight : 2
chapter : false
pre: " <b> 10. </b> "
---
### Bước 1: Xóa S3 Objects

**Phải xóa objects trước khi xóa buckets!**

#### 1.1 Xóa Objects trong Input Bucket

    1. S3 Console → Buckets
    2. Click vào `face-blur-input-bucket-input` (hoặc tên bucket của bạn)
    3. Tab **Objects**
    4. **Select all** (checkbox ở đầu danh sách)
    5. Click **Delete**
    6. Confirm:
    - Type: `permanently delete`
    - Click **Delete objects**

**Verify:** "Successfully deleted X objects"

#### 1.2 Xóa Objects trong Output Bucket

Lặp lại các bước trên với `face-blur-output-bucket-output`

**Checkpoint:** Cả 2 buckets đều empty (0 objects)

---

### Bước 2: Xóa S3 Event Notifications

#### 2.1 Xóa Event Notifications

   1. S3 Console → Input bucket
   2. Tab **Properties**
   3. Scroll xuống **Event notifications**
   4. Bạn sẽ thấy 2 notifications:
      - Event notification cho `.mp4` files
      - Event notification cho `.mov` files
   5. Select từng notification → Click **Delete**
   6. Confirm deletion

**Checkpoint:** Event notifications = 0


### Bước 3: Xóa Lambda Functions

#### 3.1 Xóa Lambda 4 (Blur Faces)

    1. Lambda Console → Functions
    2. Select `BlurFacesFunction`
    3. Actions → **Delete**
    4. Confirm:
    - Type: `delete`
    - Click **Delete**

#### 3.2 Xóa Lambda 3 (Get Timestamps)

    Lặp lại với `GetTimestampsFunction`

#### 3.3 Xóa Lambda 2 (Check Status)

Lặp lại với `CheckStatusFunction`

#### 3.4 Xóa Lambda 1 (Start Face Detect)

Lặp lại với `StartFaceDetectFunction`

**Checkpoint:** 0 Lambda functions còn lại

---

### Bước 4: Xóa Step Functions State Machine

1. Step Functions Console → State machines
2. Select `FaceBlurStateMachine`
3. Click **Delete**
4. Confirm:
   - Type: `delete`
   - Click **Delete state machine**

**Thời gian:** ~30 giây

**Checkpoint:** State machine đã bị xóa

---

### Bước 5: Xóa ECR Repository và Docker Images

#### 5.1 Xóa Docker Images trong ECR

1. ECR Console → Repositories
2. Click vào `blur-faces-lambda`
3. Tab **Images**
4. Select all images (checkbox)
5. Click **Delete**
6. Confirm:
   - Type: `delete`
   - Click **Delete**

**Verify:** "Successfully deleted X images"

#### 5.2 Xóa ECR Repository

1. ECR Console → Repositories
2. Select `blur-faces-lambda`
3. Click **Delete**
4. Confirm:
   - Type: `delete`
   - Click **Delete**

**Checkpoint:** ECR repository đã bị xóa

---

### Bước 6: Xóa S3 Buckets

**⚠️ Buckets phải empty trước khi xóa!**

#### 6.1 Xóa Input Bucket

1. S3 Console → Buckets
2. Select `face-blur-input-bucket-input`
3. Click **Delete**
4. Confirm:
   - Type bucket name: `face-blur-input-bucket-input`
   - Click **Delete bucket**

#### 6.2 Xóa Output Bucket

Lặp lại với `face-blur-output-bucket-output`

**Checkpoint:** 0 buckets còn lại (hoặc chỉ còn buckets khác)

---

### Bước 7: Xóa IAM Roles

**Thứ tự:** Xóa theo thứ tự bất kỳ (không có dependencies)

#### 7.1 Xóa Lambda Roles

1. IAM Console → Roles
2. Search: `Lambda`
3. Select các roles sau (có thể select nhiều):
   - `LambdaStartFaceDetectRole`
   - `LambdaCheckStatusRole`
   - `LambdaGetTimestampsRole`
   - `LambdaBlurFacesRole`
4. Click **Delete**
5. Confirm:
   - Type role name
   - Click **Delete**

**Lặp lại cho từng role nếu không thể xóa nhiều cùng lúc.**

#### 7.2 Xóa Step Functions Role

1. IAM Console → Roles
2. Search: `StepFunctions`
3. Select `StepFunctionsExecutionRole`
4. Click **Delete**
5. Confirm deletion

**Checkpoint:** 5 IAM roles đã bị xóa

---

### Bước 8: Xóa CloudWatch Log Groups (Optional)

**Lưu ý:** Log groups sẽ tự động bị xóa sau 30 ngày nếu không có logs mới. Bạn có thể giữ lại để review sau.

#### 8.1 Xóa Lambda Log Groups

1. CloudWatch Console → Log groups
2. Search: `/aws/lambda/`
3. Select các log groups:
   - `/aws/lambda/StartFaceDetectFunction`
   - `/aws/lambda/CheckStatusFunction`
   - `/aws/lambda/GetTimestampsFunction`
   - `/aws/lambda/BlurFacesFunction`
4. Actions → **Delete log group(s)**
5. Confirm deletion

#### 8.2 Xóa Step Functions Log Groups (Nếu có)

Search: `/aws/states/` và xóa tương tự

**Checkpoint:** Log groups đã bị xóa

---

### Bước 9: Xóa Docker Images Local (Optional)

**Để giải phóng disk space trên máy local:**

#### 9.1 List Docker Images

```powershell
# Windows/macOS/Linux
docker images | grep blur-faces-lambda
```

Bạn sẽ thấy:
```
blur-faces-lambda                                                latest    abc123    1.2GB
208613876063.dkr.ecr.ap-southeast-1.amazonaws.com/blur-faces-lambda   latest    abc123    1.2GB
```

#### 9.2 Xóa Images

```powershell
# Xóa local image
docker rmi blur-faces-lambda:latest

# Xóa ECR tagged image
docker rmi 123456789.dkr.ecr.ap-southeast-1.amazonaws.com/blur-faces-lambda:latest
```

**Hoặc xóa tất cả unused images:**
```powershell
docker image prune -a
```

**Verify:**
```powershell
docker images | grep blur-faces-lambda
# Không có kết quả
```