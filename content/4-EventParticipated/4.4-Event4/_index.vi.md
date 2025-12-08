---
title: "Event Report – AI/ML/GenAI on AWS"
date: "2025-11-15"
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---


# Summary Report: “AI/ML/GenAI on AWS Workshop”

### Event Objectives

- Giới thiệu tổng quan hệ sinh thái AI/ML/GenAI trên AWS  
- Hướng dẫn cách sử dụng Amazon SageMaker trong toàn bộ vòng đời ML  
- Trình bày kiến trúc và demo Generative AI với Amazon Bedrock  
- Trang bị kiến thức thực hành về RAG, prompt engineering và guardrails  

### Event Details

- **Date:** Saturday, November 15, 2025  
- **Time:** 8:30 AM – 12:00 PM  
- **Location:** AWS Vietnam Office  

---

# Key Highlights

## Welcome & Introduction (8:30 – 9:00 AM)

- Người tham dự đăng ký và networking  
- Giới thiệu tổng quan workshop và mục tiêu học tập  
- Ice-breaker activity giúp kết nối các nhóm  
- Trình bày tổng quan về tình hình AI/ML tại Việt Nam  

---

# AWS AI/ML Services Overview (9:00 – 10:30 AM)

## Amazon SageMaker – Nền tảng ML toàn diện

- Hướng dẫn tiền xử lý dữ liệu và gán nhãn  
- Train, tune, deploy mô hình  
- Tích hợp MLOps pipeline  
- Tối ưu chi phí với Autopilot, Processing Jobs, Batch Transform  

### Live Demo: SageMaker Studio

- Minh họa toàn bộ ML lifecycle  
- Experiment tracking, model registry  
- Quản lý notebook và pipeline ML  

---

# **AWS AI/ML Services Overview (Expanded)**

Ngoài SageMaker, AWS còn cung cấp bộ dịch vụ AI/ML mạnh mẽ giúp doanh nghiệp triển khai AI nhanh chóng mà không cần tự xây dựng mô hình.

## **Amazon Rekognition – Computer Vision**

**Chức năng:**

- Nhận diện vật thể, cảnh vật  
- Phân tích khuôn mặt, cảm xúc  
- Face matching  
- Phát hiện nội dung không phù hợp  
- OCR cơ bản (text-in-image)

**Pricing:**  
- **$1.00 / 1,000 ảnh** (Label detection)  
- **$0.012 / video minute**  
- **$0.001 / face search request**

---

## **Amazon Comprehend – NLP Service**

**Chức năng:**

- Phân tích cảm xúc  
- Nhận diện thực thể (NER)  
- Phân loại văn bản  
- Trích xuất key phrases  
- Custom classifier  

**Pricing:**  
- **$0.0001 / 100 ký tự**

---

## **Amazon Transcribe – Speech-to-Text**

**Chức năng:**

- Chuyển giọng nói thành văn bản  
- Phân biệt người nói  
- Custom vocabulary  
- Call analytics  

**Pricing:**  
- **$0.024 / phút** (standard STT)  
- **$0.048 / phút** (analytics)

---

## **Amazon Polly – Text-to-Speech**

**Chức năng:**

- Chuyển văn bản thành giọng nói tự nhiên  
- Nhiều giọng Neural TTS  
- Hỗ trợ nhiều ngôn ngữ  

**Pricing:**  
- **$16 / 1 triệu ký tự** (standard)  
- **$20 / 1 triệu ký tự** (neural)

---

## **Amazon Textract – Document AI / OCR nâng cao**

**Chức năng:**

- OCR tài liệu  
- Trích xuất bảng và form  
- Key-value pairs  
- ID analyzer  

**Pricing:**  
- **$1.50 / 1,000 trang** (OCR)  
- **$50 / 1,000 trang** (Forms)  
- **$15 / 1,000 trang** (Tables)

---

## **Amazon Kendra – Enterprise Search**

**Chức năng:**

- Semantic search  
- Tích hợp nhiều nguồn dữ liệu  
- FAQ Matching  
- Kết hợp với chatbot  

**Pricing:**  
- **$1.50 / giờ** (Developer Edition)  
- **$7 / giờ** (Enterprise Edition)

---

## **Amazon Lex – NLP Chatbot**

**Chức năng:**

- Xây dựng chatbot văn bản & giọng nói  
- Nhận diện intent  
- Tích hợp với Lambda và Contact Center  

**Pricing:**  
- **$0.0075 / text request**  
- **$0.02 / voice request**

---

## **Amazon Forecast**

- Dự báo chuỗi thời gian  
- Demand forecasting  
- Capacity planning  
- Trả tiền theo số lượng data & training hours  

---

## **Amazon Personalize**

- Đề xuất sản phẩm giống Amazon.com  
- Personalized ranking  
- User-item recommendations  

**Pricing:**  
- Trả theo:  
  - Data ingestion  
  - Training hours  
  - Recommendation requests  

---

# Coffee Break (10:30 – 10:45 AM)

- Nghỉ ngơi và networking cùng diễn giả và AWS SA  

---

# Generative AI with Amazon Bedrock (10:45 AM – 12:00 PM)

## Foundation Models

- So sánh Claude, Llama, Titan  
- Tiêu chí lựa chọn mô hình  

## Prompt Engineering

- Prompt viết cơ bản → nâng cao  
- Chain-of-thought  
- Zero-shot & few-shot  

## RAG (Retrieval-Augmented Generation)

- RAG architecture tiêu chuẩn  
- Knowledge Base integration  
- Giảm hallucination, tăng độ chính xác  

## Bedrock Agents

- Multi-step workflows  
- Tool API integration  
- Quan sát & debugging agents  

## Guardrails

- Lọc nội dung độc hại  
- Kiểm soát input/output  
- Bảo mật và tuân thủ dữ liệu  

## Live Demo: GenAI Chatbot

- Prompt → Agent → Knowledge Base  
- Triển khai chatbot Bedrock end-to-end  

---

# Key Takeaways

## AI/ML Mindset

- Hiểu trọn ML lifecycle  
- MLOps là nền tảng cho sản xuất  
- GenAI cần dữ liệu sạch & kiểm soát rủi ro  

## Technical Architecture

- SageMaker: ML end-to-end  
- Bedrock: GenAI serverless platform  
- RAG: quan trọng trong enterprise  
- Prompt engineering: tối ưu mô hình  

## Modernization Strategy

- Kết hợp ML + GenAI  
- Chọn Foundation Model theo use case  
- Triển khai guardrails  

---

# Applying to Work

- Xây dựng ML pipelines bằng SageMaker  
- Triển khai RAG cho nội bộ doanh nghiệp  
- Áp dụng prompt engineering thường xuyên  
- POC chatbot GenAI với Bedrock  

---

# Event Experience

### Học hỏi từ chuyên gia AWS
- Kiến thức thực chiến AI/ML và GenAI  

### Demo thực tế
- SageMaker Studio và Bedrock workflow  

### Tư duy chiến lược AI
- Hiểu vai trò RAG, Agents, Guardrails  

### Networking
- Kết nối với kỹ sư, chuyên gia và cộng đồng AI  


