---
title : "Workshop Summary & Cost"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.6.1 </b> "
---

## Tổng kết Workshop

Xin chúc mừng! Bạn đã hoàn thành việc triển khai và testing hệ thống **SmartDocAI** trên AWS với kiến trúc serverless đầy đủ.

### Những gì đã học được

#### 1. **Kiến trúc Serverless & Microservices**
- Deploy FastAPI application lên Lambda với Docker container
- Sử dụng API Gateway làm HTTP endpoint public
- Tận dụng CloudFront CDN cho static assets và API caching
- Thiết kế 3-tier architecture (Presentation → Application → Data)

#### 2. **Authentication & Authorization**
- Tích hợp Cognito User Pool cho user management
- Implement JWT-based authentication
- Hỗ trợ multi sign-in methods (Email/Password + Google OAuth)
- Per-user data isolation với S3 prefixes + DynamoDB partition keys

#### 3. **AI & Machine Learning Integration**
- Sử dụng Amazon Bedrock (Mixtral 8x7B LLM + Titan Embeddings V2)
- Xây dựng RAG (Retrieval-Augmented Generation) pipeline
- Vector search với FAISS in-memory database
- Implement 3 RAG modes: Standard, Self-RAG, Co-RAG

#### 4. **CI/CD Pipeline**
- Setup CodePipeline tự động trigger từ GitHub push
- Integrate pytest unit tests trong CodeBuild (hard fail)
- Docker-based deployment với ECR registry
- Automated Lambda function updates

#### 5. **Automation & Monitoring**
- EventBridge scheduled rules cho cleanup tasks
- CloudWatch Logs + Insights cho application monitoring
- Lambda metrics (invocations, duration, errors, throttles)
- Real-time log tailing và query

#### 6. **Security Best Practices**
- Input validation (phone, DOB, fullname với XSS prevention)
- CORS restrictions (no wildcard `*`)
- HTTPS only (TLS 1.2+)
- JWT expiration + signature validation
- Security audit với documented limitations

#### 7. **Cost Optimization**
- Pay-per-use model với serverless services
- On-demand DynamoDB billing (no idle cost)
- S3 presigned URLs (bypass Lambda for uploads)
- CloudFront Free Tier (1 TB/month)
- **Estimated cost:** $7-15/month cho moderate usage

---

## Cost Summary - Tổng chi phí

### Chi phí thực tế đã phát sinh (Workshop Duration)

Giả sử workshop chạy trong **7 ngày** với moderate testing:

| Dịch vụ | Usage | Chi phí (7 days) | Chi phí (Monthly estimate) |
|---------|-------|------------------|---------------------------|
| Lambda | 5K invocations, 2GB RAM, 5s avg | $0.20 | $0.85 |
| API Gateway | 5K requests | $0.02 | $0.08 |
| S3 (Frontend) | 2 GB storage, 5 GB transfer | $0.10 | $0.35 |
| S3 (Storage) | 10 GB storage, 10 GB transfer | $0.35 | $1.20 |
| DynamoDB | 2K reads, 1K writes | $0.05 | $0.15 |
| Cognito | 50 users (MAU) | Free | Free |
| CloudFront | 20 GB transfer (Free tier) | $0.00 | $0.00 |
| Bedrock (LLM) | 200K input tokens, 50K output tokens | $0.13 | $0.45 |
| Bedrock (Embeddings) | 1M tokens | $0.10 | $0.35 |
| CodePipeline | 1 active pipeline | Free | Free |
| CodeBuild | 5 builds, 5 min each | $0.02 | $0.08 |
| ECR | 1.5 GB storage | $0.15 | $0.20 |
| EventBridge | 2016 events (every 5min × 7 days) | Free | Free |
| CloudWatch Logs | 0.5 GB ingested | $0.25 | $1.00 |
| **TOTAL** | | **~$1.37** | **~$4.71** |

**Notes:**
- **[YES]** Workshop cost: **Dưới $2** (rất low cost)
- **[YES]** Free Tier: Cognito, EventBridge, CodePipeline, CloudFront (1 TB) đều free
- **[NOTE]** Lambda provisioned concurrency (optional): +$4/month nếu enable
- **[NOTE]** Production cost với higher traffic: $15-50/month

---