---
title : "Khởi tạo S3 Bucket "
date : "2025-10-10"
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---

### 1.1 Tạo Input Bucket

1. Đăng nhập AWS Console
2. Tìm kiếm "S3" trong thanh tìm kiếm
3. Click **"Create bucket"**

    #### **Cấu hình:**
      - **Bucket name:** `face-blur-input-bucket-[your-name]`
        - Ví dụ: `face-blur-input-bucket-john123`
        - Phải unique globally
     - **AWS Region:** Chọn region (ví dụ: ap-southeast-1)
     - **Block Public Access:** Giữ mặc định (Block all)
     - **Bucket Versioning:** Disabled (hoặc Enabled nếu muốn)
     - **Encryption:** Enable (Server-side encryption with Amazon S3 managed keys)

4. Click **"Create bucket"**
![image]()

### 1.2 Tạo Output Bucket

Lặp lại các bước trên với:
- **Bucket name:** `face-blur-output-bucket-[your-name]`
- Các cấu hình khác giống Input Bucket

**Checkpoint:** Bạn có 2 S3 buckets
