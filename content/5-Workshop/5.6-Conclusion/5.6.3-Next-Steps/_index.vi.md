---
title : "Next Steps & References"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.6.3 </b> "
---

## Next Steps After Workshop

### 1. Production Deployment Considerations
- Enable S3/DynamoDB encryption (KMS)
- Deploy Lambda in VPC with NAT Gateway
- Add API Gateway resource policies (IP whitelist)
- Implement per-user rate limiting
- Enable Cognito MFA
- Add OAuth state parameter (CSRF protection)
- Setup CloudWatch alarms (error rate, latency)
- Implement blue/green deployment for Lambda

### 2. Cost Optimization
- Use S3 Intelligent-Tiering for infrequent access
- Implement DynamoDB auto-scaling (reserved capacity)
- Add CloudFront caching for API responses (where applicable)
- Use Lambda SnapStart (reduce cold start to ~100ms)
- Compress documents before uploading (reduce S3 storage)

### 3. Enhanced Features
- Add multi-region deployment (ap-southeast-1 Singapore)
- Implement real-time collaboration (WebSocket API)
- Add document OCR support (Textract)
- Implement full-text search (OpenSearch)
- Add email notifications (SES)
- Implement audit logs (CloudTrail + S3)

### 4. Learning Resources
- [AWS Serverless Application Model (SAM)](https://aws.amazon.com/serverless/sam/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [Amazon Bedrock Best Practices](https://docs.aws.amazon.com/bedrock/)
- [LangChain Documentation](https://python.langchain.com/)
- [FastAPI Documentation](https://fastapi.tiangolo.com/)

---

## Tài liệu tham khảo

- **Source code:** [GitHub - TakunKenjo/SmartdocAI-AWS](https://github.com/TakunKenjo/SmartdocAI-AWS)
- **Architecture:** Section 5.1.3 "Kiến trúc tổng thể trên AWS"
- **AWS services:** Section 5.1.4 "Các dịch vụ AWS được sử dụng"
- **Testing guide:** Section 5.5 "Kiểm thử hệ thống"
- **Security audit:** `SmartdocAI-AWS/SECURITY_CONSIDERATIONS.md`
- **EventBridge setup:** `SmartdocAI-AWS/EVENTBRIDGE_SETUP_GUIDE.md`
- **Handover document:** `SmartdocAI-AWS/BANGIAO.md` (branch: tam)

---

## Feedback & Contact

Nếu có câu hỏi hoặc feedback về workshop này, vui lòng liên hệ:

- **Email:** 12345levan@gmail.com
- **GitHub:** [@TakunKenjo](https://github.com/TakunKenjo)
- **Workshop Repository:** [Workshop-AWS-Group-Report](https://github.com/TakunKenjo/Workshop-AWS-Group-Report)

**Cảm ơn bạn đã hoàn thành workshop SmartDocAI!** 🎉