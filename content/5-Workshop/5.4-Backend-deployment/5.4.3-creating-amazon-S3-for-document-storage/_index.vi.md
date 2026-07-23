---
title : "Tạo Amazon S3 lưu trữ tài liệu"
date : 2024-01-01 
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

### 1. Khai báo Tên Bucket và Region


![image5.png](/images/5-Workshop/5.4-Backend-deployment/5.4.3-creating-amazon-S3-for-document-storage/image5.png)


### 2. Chạy lệnh Tạo S3 Bucket


![image6.png](/images/5-Workshop/5.4-Backend-deployment/5.4.3-creating-amazon-S3-for-document-storage/image6.png)


### 3. Cấu hình CORS (Cho phép Frontend Tải tài liệu trực tiếp)

#### 3.1. Định nghĩa cấu hình CORS bằng Biến PowerShell:


![image7.png](/images/5-Workshop/5.4-Backend-deployment/5.4.3-creating-amazon-S3-for-document-storage/image7.png)


#### 3.2. Chạy lệnh áp dụng CORS policy:
![image8.png](/images/5-Workshop/5.4-Backend-deployment/5.4.3-creating-amazon-S3-for-document-storage/image8.png)


### 4. Cấu hình Phân Quyền IAM Role cho AWS Lambda




Đảm bảo IAM Role chạy Lambda Function (ví dụ: smartdocai-lambda-role) được đính kèm Policy truy cập S3.

Chạy lệnh gán quyền AmazonS3FullAccess cho Lambda Role:


![image9.png](/images/5-Workshop/5.4-Backend-deployment/5.4.3-creating-amazon-S3-for-document-storage/image9.png)


Kết quả:


![image10.png](/images/5-Workshop/5.4-Backend-deployment/5.4.3-creating-amazon-S3-for-document-storage/image10.png)


Cấu Trúc Thư Mục S3


![image11.png](/images/5-Workshop/5.4-Backend-deployment/5.4.3-creating-amazon-S3-for-document-storage/image11.png)



| STT | Thư mục (Prefix) | Loại tệp | Mô tả & Chức năng |
| --- | --- | --- | --- |
| 1 | uploads/{user_id}/ | .pdf, .docx | Lưu trữ file tài liệu gốc mà người dùng tải lên từ Frontend qua Presigned URL. |
| 2 | vectorstore/{user_id}/smartdoc_index/ | index.faiss, index.pkl | Lưu chỉ mục Vector DB FAISS đã chuyển đổi từ tài liệu của người dùng, dùng cho RAG search. |
| 3 | processed_files/ | {user_id}.json | Lưu thông tin quản lý file (tên file, số trang, số chunks, s3_key) để hiển thị danh sách ở Frontend. |
| 4 | chat_history/ | {user_id}.json | Lưu toàn bộ lịch sử hỏi đáp, câu trả lời AI và nguồn trích dẫn (sources) per-user. |
| 5 | avatars/{user_id}/ | .jpg, .png | Lưu ảnh đại diện của user khi dung lượng ảnh > 300KB (nếu < 300KB sẽ lưu base64 ở DynamoDB). |
| 6 | metadata/ | search_config.json | Lưu cài đặt RAG chung (chunk_size, chunk_overlap, trạng thái Reranker/Self-RAG/Co-RAG). |

