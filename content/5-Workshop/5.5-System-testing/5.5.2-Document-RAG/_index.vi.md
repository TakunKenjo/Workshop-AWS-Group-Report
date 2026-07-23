---
title : "Kiểm thử tải lên tài liệu & RAG"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.5.2 </b> "
---

Phần này kiểm tra luồng nghiệp vụ cốt lõi của SmartDocAI: tải tài liệu lên qua S3 presigned URL (bỏ qua giới hạn 10MB của API Gateway), lập chỉ mục vector bằng Bedrock Titan Embeddings, và truy vấn RAG ở 3 chế độ khác nhau (Standard, Self-RAG, Co-RAG). Đây là tính năng quan trọng nhất của hệ thống nên được kiểm thử kỹ cả về chức năng lẫn hiệu năng.

### 1. Bảng tổng hợp test case

| # | Test Case | Endpoint | Input | Expected Result | Status |
|---|-----------|----------|-------|-----------------|--------|
| **PRESIGNED URL** |
| 2.1 | Get S3 presigned URL | GET /documents/upload-url?filename=test.pdf | JWT token + filename query param | 200 OK<br/>Return: upload_url (1h expiry) + file_key | PASS |
| 2.2 | Presigned URL requires auth | GET /documents/upload-url?filename=test.pdf | No JWT token | 401 Unauthorized<br/>Error: "Missing Authorization header" | PASS |
| **S3 UPLOAD** |
| 2.3 | Upload PDF to S3 | PUT to presigned URL | PDF file + Content-Type: application/pdf | 200 OK (direct S3 upload, bypass Lambda) | PASS |
| 2.4 | CORS allowed origin | PUT to presigned URL | Origin: https://dutf3c70nnjzl.cloudfront.net | 200 OK + CORS headers | PASS |
| 2.5 | CORS blocked origin | PUT to presigned URL | Origin: https://evil.com | 403 Forbidden (no CORS header) | PASS |
| **DOCUMENT INDEXING** |
| 2.6 | Index uploaded document | POST /documents | JWT + filename + file_key | 200 OK<br/>Return: document_id + chunks_created + processing_time (~8s) | PASS |
| 2.7 | Verify backend processing | Check CloudWatch logs | After POST /documents | Log: "Extracted 5 pages" → "Created 15 chunks" → "Indexed 15 vectors" | PASS |
| **RAG QUERY - STANDARD MODE** |
| 2.8 | Standard RAG query | POST /chat | JWT + query + mode=standard + top_k=5 | 200 OK<br/>Return: answer + sources (5 chunks) + similarity_scores | PASS |
| 2.9 | Query without documents | POST /chat | User with no indexed documents | 200 OK<br/>Return: "No documents found. Please upload first" | PASS |
| **RAG QUERY - SELF-RAG MODE** |
| 2.10 | Self-RAG with relevance grading | POST /chat | query + mode=self-rag + top_k=5 | 200 OK<br/>Return: answer + filtered_chunks (2-3) + relevance_scores | PASS |
| 2.11 | Self-RAG filters low relevance | POST /chat | Irrelevant query to documents | Return: filtered_chunks=0 + "No relevant context found" | PASS |
| **RAG QUERY - CO-RAG MODE** |
| 2.12 | Co-RAG collaborative retrieval | POST /chat | query + mode=co-rag | 200 OK<br/>Return: answer + sources (7-10 chunks) + processing_time (~10-15s) | PASS |

**Điểm cuối API gốc:** `https://d60866voq5.execute-api.us-east-1.amazonaws.com/prod`

**Các bước xử lý ở Backend (lập chỉ mục tài liệu):**
1. Tải file từ S3 → 2. Parse PDF (pypdf2) → 3. Chia nhỏ văn bản (500 tokens, overlap 50) → 4. Sinh embeddings (Bedrock Titan V2, 1024 chiều) → 5. Lưu vector vào FAISS (in-memory, cô lập theo từng user)

**Cấu hình CORS cho S3:**
- **Origin được phép:** CloudFront (dutf3c70nnjzl.cloudfront.net), localhost:5173, localhost:5174
- **Method được phép:** GET, PUT, POST, DELETE, HEAD
- **Max Age:** 3600 giây

**So sánh hiệu năng giữa các chế độ RAG:**

| Metric | Standard RAG | Self-RAG | Co-RAG |
|--------|-------------|----------|--------|
| Processing time | 5-8s | 8-12s | 10-15s |
| Accuracy | 75% | 85% | 80% |
| Chunks used | 5 | 2-3 (filtered) | 7-10 |
| Bedrock API calls | 1 | 3 (grade + generate) | 2 |
| Best for | General queries | High-accuracy answers | Comprehensive context |

---