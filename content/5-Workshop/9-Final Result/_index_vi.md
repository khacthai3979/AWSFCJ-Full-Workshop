---
title : "Test hệ thống End-to-end"
date : "2025-10-10"
weight : 2
chapter : false
pre : " <b> 9. </b> "
---

## Bước 6: Test Hệ thống End-to-En

### 6.1 Chuẩn bị Video Test

**Yêu cầu video:**
- Format: .mp4 hoặc .mov
- Có ít nhất 1 khuôn mặt rõ ràng
- Độ dài: 10-30 giây (để test nhanh)
- Kích thước: < 100 MB

**Download video test miễn phí:**
- Pexels.com → Search "people talking"
- Pixabay.com → Search "interview"
- Hoặc quay video từ điện thoại

### 6.2 Upload Video lên S3

1. S3 Console → Input bucket (`face-blur-input-bucket-**`)
2. Click **Upload**
3. Click **Add files**
4. Chọn video test
5. Click **Upload**
6. Đợi upload hoàn tất (progress bar = 100%)

**Lưu ý:** Đợi upload hoàn tất trước khi check logs!

### 6.3 Monitor Workflow Execution

#### 6.3.1 Check Lambda 1 (Start Face Detect)

1. Lambda Console → `StartFaceDetectFunction`
2. Tab **Monitor** → **View logs in CloudWatch**
3. Click vào log stream mới nhất (thời gian gần nhất)

**Logs thành công:**
```
START RequestId: abc-123-def
Started processing: job-id-xyz123
END RequestId: abc-123-def
```

**Nếu có lỗi:**
- Check S3 trigger đã cấu hình chưa?
- Check IAM permissions

#### 6.3.2 Check Step Functions

1. Step Functions Console → `FaceBlurStateMachine`
2. Tab **Executions**
3. Click vào execution mới nhất (status: Running hoặc Succeeded)

**Visual Workflow:**
- 🔵 **Blue (Running):** Step đang chạy
- 🟢 **Green (Succeeded):** Step hoàn thành
- 🔴 **Red (Failed):** Step bị lỗi

**Workflow sẽ trải qua:**
1. **CheckJobStatus** (lặp nhiều lần với Wait1Second)
   - Đợi Rekognition phân tích video
   - Thời gian: 30-60 giây cho video 30 giây
2. **GetTimestampsAndFaces**
   - Lấy thông tin khuôn mặt và timestamps
   - Thời gian: 5-10 giây
3. **BlurFacesOnVideo**
   - Xử lý blur với OpenCV
   - Thời gian: 1-5 phút tùy video
4. **ExecutionSucceeded**
   - Hoàn thành!

**Thời gian tổng:** 2-7 phút cho video 30 giây

#### 6.3.3 Check Lambda 4 (Blur Faces) - Chi tiết

1. Lambda Console → `BlurFacesFunction`
2. Tab **Monitor** → **View logs in CloudWatch**
3. Click vào log stream mới nhất


**Nếu thấy lỗi:**
- Check IAM permissions (S3 GetObject/PutObject)
- Check memory và timeout đủ chưa?
- Check output bucket name đúng chưa?

### 6.4 Download và Verify Output Video

#### 6.4.1 Download Video

1. S3 Console → Output bucket (`face-blur-output-bucket-*****`)
2. Tìm file video (cùng tên với input)
3. Select file → Click **Download**
4. Đợi download hoàn tất

#### 6.4.2 Verify Kết quả

**Mở video và kiểm tra:**

**Faces bị blur (pixelated):**
- Tất cả khuôn mặt trong video bị làm mờ
- Blur style: Pixelation (ô vuông)
- Blur theo chuyển động của khuôn mặt
