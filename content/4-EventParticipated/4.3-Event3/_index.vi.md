---
title: "AWS Perimeter Protection Workshop – Tăng cường bảo mật với CloudFront & WAF"
date: "2025-11-18"
chapter: false
weight: 3
pre: "<b>4.3.</b>"
---

# AWS Perimeter Protection Workshop – Tăng cường bảo mật với CloudFront & WAF

## Tổng quan sự kiện
Ngày **18/11**, tại văn phòng **AWS Vietnam** để tham gia workshop **AWS Perimeter Protection**, tập trung vào bảo mật ứng dụng web bằng **Amazon CloudFront** và **AWS WAF**.  
Buổi workshop cung cấp nhiều kiến thức **thực chiến**, insight mới và demo trực tiếp giúp hiểu sâu về cách bảo vệ ứng dụng từ **Edge đến Origin**.

Buổi chia sẻ được dẫn dắt bởi:
- **Nguyễn Gia Hưng** – Head of Solutions Architect, AWS Vietnam

---

## CloudFront – Bảo vệ từ Edge đến Origin

### Mô hình định giá mới
Một trong những điểm nổi bật nhất là **mô hình định giá theo gói** của CloudFront — giúp doanh nghiệp không còn lo “hóa đơn tăng đột biến”:

| Gói       | Giá             |
|-----------|-----------------|
| Free      | $0              |
| Pro       | $15/tháng       |
| Business  | $200/tháng      |
| Premium   | $1,000/tháng    |

Mô hình giá này phù hợp cả **startup**, **SME** và **enterprise**, đảm bảo **chi phí ổn định** và dễ dự toán ngân sách.

---

### Hiệu suất & tối ưu chi phí
Một số kết quả benchmark thực tế được chia sẻ:

- **Giảm 63%** chi phí Load Balancer  
- **Giảm 80%** sử dụng CPU trên EC2 origin  
- **HTTP/3 (QUIC)** hỗ trợ **11 download song song**  
- **Tự động nén HTTP** giảm **82%** kích thước file  

Điều này cho thấy việc **đưa logic caching ra Edge** vừa giảm tải cho backend, vừa tăng tốc độ truy cập.

---

### Tính năng CloudFront mới
Một số tính năng nổi bật:

#### VPC Origin
- Cho phép đặt **ALB/EC2 trong Private Subnet**
- CloudFront tạo **ENI** và **Tunnel traffic** trực tiếp
- Origin **không tiếp xúc internet**
- Tăng bảo mật cho các hệ thống tài chính, ngân hàng

#### Mutual TLS (mTLS – sắp ra mắt)
- Xác thực **client certificate** tại CloudFront
- Quan trọng với **Financial APIs**, **Open Banking**, **B2B Integration**

---

## AWS WAF – Chiến lược bảo vệ ứng dụng toàn diện

### Xu hướng mối đe dọa
Workshop phân tích các xu hướng tấn công tăng mạnh:

- **DDoS tăng 29%** YoY  
- **AI bot surge tăng 155%** (GPT, Claude, OpenAI Agents…)  

Điều này yêu cầu chiến lược bảo vệ **đa lớp – đa tầng**, không chỉ chặn ở Layer 7.

---

### Giải pháp bảo vệ cốt lõi
Các thành phần chủ lực của AWS trong bảo vệ biên:

#### Rate-based Rules
- Giới hạn tốc độ gửi request
- Chặn flood traffic & brute force

#### Anti-DDoS AMR (Automatic Mitigation Rules)
- Phát hiện tự động, phản hồi trong vài giây
- Không cần can thiệp thủ công

#### Bot Control
- Phát hiện **Evasive Bot**
- Sử dụng **Client interrogation** và **Fingerprinting**

#### AWS Shield Advanced
- Bảo vệ DDoS toàn diện
- **Automatic Mitigation**
- **Cost Protection**: AWS **refund** chi phí do DDoS

---

## Pattern khuyến nghị cho WAF Rules

Thứ tự ưu tiên của Rule ảnh hưởng trực tiếp đến **hiệu suất**, **chi phí** và **tỷ lệ false positive**.  
Đề xuất theo thứ tự:

1. **Allowed IPs**  
2. **Blocked IPs**  
3. **Anti-DDoS AMRs**  
4. **Rate limiting**  
5. **Anonymous IPs / IP Reputation**  
6. **Core rules**  
7. **False positive exceptions**  
8. **Bot/Fraud rules**  

Việc sắp xếp đúng giúp tối ưu luồng xử lý và giảm chi phí WAF.

---

## Best Practices quan trọng

### End-to-end Visibility
- **CloudWatch RUM**  
- **AWS Internet Monitor**  
- **Infrastructure Monitoring**  

Quan sát từ **Client → Edge → Origin**.

### Maximize Caching
- Normalize cache key  
- Cache dynamic content với **short TTL**  
- Cache error để giảm tải backend  

### Block Unwanted Requests
- AWS WAF  
- Malicious pattern blocking  
- Rate limiting  

### Offload Business Logic
- **CloudFront Functions** cho:
  - CORS
  - Redirects
  - Cookie settings
  - Header injection

Giảm logic cần xử lý tại backend.

### Automatic Failover
- Route 53 **Health-based Routing**  
- CloudFront **Origin Groups**  

Đảm bảo **HA (High Availability)** khi origin gặp sự cố.

---

## Kết luận
Workshop giúp mình hiểu rõ cách **đưa việc bảo vệ hệ thống ra Edge**, tối ưu chi phí, tăng hiệu suất và xây dựng **mô hình bảo mật đa lớp**.  
Điểm mình ấn tượng nhất là cách CloudFront và WAF phối hợp với Shield, Bot Control, Route 53 để tạo thành **perimeter protection architecture** hoàn chỉnh.

Việc áp dụng các best practice này không chỉ giúp hệ thống **an toàn**, mà còn **tối ưu chi phí vận hành**.


