---
title : "Creating Amazon S3 for document storage"
date : 2024-01-01 
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

### 1. Declare Bucket Name and Region


![image5.png](/images/5-Workshop/5.4-Backend-deployment/5.4.3-creating-amazon-S3-for-document-storage/image5.png)


### 2. Run Command to Create S3 Bucket


![image6.png](/images/5-Workshop/5.4-Backend-deployment/5.4.3-creating-amazon-S3-for-document-storage/image6.png)


### 3. Configure CORS (Allow Frontend Direct Document Upload)

#### 3.1. Define CORS configuration using PowerShell variables:


![image7.png](/images/5-Workshop/5.4-Backend-deployment/5.4.3-creating-amazon-S3-for-document-storage/image7.png)


#### 3.2. Run command to apply CORS policy:
![image8.png](/images/5-Workshop/5.4-Backend-deployment/5.4.3-creating-amazon-S3-for-document-storage/image8.png)


### 4. Configure IAM Role Permissions for AWS Lambda

Ensure the IAM Role running the Lambda Function (e.g., `smartdocai-lambda-role`) has attached S3 access policy.

Run command to attach `AmazonS3FullAccess` policy to the Lambda Role:


![image9.png](/images/5-Workshop/5.4-Backend-deployment/5.4.3-creating-amazon-S3-for-document-storage/image9.png)


Result:


![image10.png](/images/5-Workshop/5.4-Backend-deployment/5.4.3-creating-amazon-S3-for-document-storage/image10.png)


S3 Directory Structure:


![image11.png](/images/5-Workshop/5.4-Backend-deployment/5.4.3-creating-amazon-S3-for-document-storage/image11.png)


| No. | Directory (Prefix) | File Type | Description & Function |
| --- | --- | --- | --- |
| 1 | `uploads/{user_id}/` | `.pdf`, `.docx` | Stores original document files uploaded by users from the Frontend via Presigned URLs. |
| 2 | `vectorstore/{user_id}/smartdoc_index/` | `index.faiss`, `index.pkl` | Stores FAISS Vector DB index converted from user documents, used for RAG search. |
| 3 | `processed_files/` | `{user_id}.json` | Stores file management info (filename, page count, chunk count, s3_key) to display file lists on the Frontend. |
| 4 | `chat_history/` | `{user_id}.json` | Stores complete Q&A chat history, AI answers, and source citations per user. |
| 5 | `avatars/{user_id}/` | `.jpg`, `.png` | Stores user avatar images when image size > 300KB (if < 300KB, base64 is stored in DynamoDB). |
| 6 | `metadata/` | `search_config.json` | Stores general RAG settings (`chunk_size`, `chunk_overlap`, Reranker/Self-RAG/Co-RAG toggle states). |
