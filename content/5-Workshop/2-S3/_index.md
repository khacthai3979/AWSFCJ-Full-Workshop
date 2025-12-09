---
title : "Initialize S3 Bucket"
date : "2025-10-10"
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---

### 1.1 Create Input Bucket

1. Log in to AWS Console
2. Search for "S3" in the search bar
3. Click **"Create bucket"**

    #### **Configuration:**
      - **Bucket name:** `face-blur-input-bucket-[your-name]`
        - Example: `face-blur-input-bucket-john123`
        - Must be globally unique
     - **AWS Region:** Select region (e.g., ap-southeast-1)
     - **Block Public Access:** Keep default (Block all)
     - **Bucket Versioning:** Disabled (or Enabled if desired)
     - **Encryption:** Enable (Server-side encryption with Amazon S3 managed keys)

4. Click **"Create bucket"**
![image]()

### 1.2 Create Output Bucket

Repeat the above steps with:
- **Bucket name:** `face-blur-output-bucket-[your-name]`
- Other configurations same as Input Bucket

**Checkpoint:** You have 2 S3 buckets
