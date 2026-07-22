---
title : "Tạo Amazon DynamoDB"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

### 1.Xác định các đối tượng có trong mô hình dữ liệu

Trong SmartDocAI, DynamoDB được áp dụng cho 3 thành phần chính :

##### Bảng 1 : Bảng Quản lý hồ sơ người dùng ( smartdocai-user-profiles )

- **Partition Key (PK): user_id (String) - Tương ứng với sub (UUID) do AWS Cognito sinh ra.**

- **Attributes:**

email (String): Địa chỉ email.

fullname (String): Họ và tên đầy đủ.

phone (String): Số điện thoại (chuẩn E.164: +84...).

dob (String): Ngày sinh (YYYY-MM-DD).

avatar (String, Optional): Chuỗi Base64 (nếu ảnh < 300KB).

avatar_url (String, Optional): Đường dẫn S3 URL (nếu ảnh > 300KB).

subscription_plan (String): Gói dịch vụ (free / pro).

document_quota (Number): Giới hạn số tài liệu (Mặc định: 50).

documents_used (Number): Số tài liệu đã tải lên.

user_preferences (Map): Cấu hình ngôn ngữ, giao diện ({ language: "vi", theme: "dark" }).

created_at (Number): Timestamp khởi tạo (Unix timestamp).

updated_at (Number): Timestamp cập nhật cuối cùng.

##### Bảng 2: Bảng SmartDoc-Documents

- **Partition Key (PK): user_id (String) - Đảm bảo phân quyền tài liệu theo người dùng (Multi-tenancy).**

- **Sort Key (SK): doc_id (String) - Mã định danh UUID của tài liệu.**

- **Attributes: filename, s3_key, page_count, chunk_count, file_size, status (PROCESSED/FAILED), created_at.**

##### Bảng 3: Bảng SmartDoc-ChatSessions

- **Partition Key (PK): user_id (String).**

- **Sort Key (SK): timestamp (Number) hoặc message_id (String).**

- **Attributes: role (user/assistant), message, sources (List JSON), created_at.**

### 2.Khởi tạo tài nguyên DynamoDB trên AWS ( thông qua AWS CLI )

#### 2.1.Mở Terminal/ PowerShell trên máy local có AWS CLI đã cấu hình


![image23.png](/images/5-Workshop/5.4-Backend-deployment/5.4.2-creating-amazon-dynamoDB/image23.png)


#### 2.2.Tạo bảng smartdocai-user-profiles


![image24.png](/images/5-Workshop/5.4-Backend-deployment/5.4.2-creating-amazon-dynamoDB/image24.png)


#### 2.3.Tạo bảng SmartDoc-Documents


![image25.png](/images/5-Workshop/5.4-Backend-deployment/5.4.2-creating-amazon-dynamoDB/image25.png)


#### 2.4.Tạo bảng smartdocai-chat-history


![image26.png](/images/5-Workshop/5.4-Backend-deployment/5.4.2-creating-amazon-dynamoDB/image26.png)


Kết quả :


![image27.png](/images/5-Workshop/5.4-Backend-deployment/5.4.2-creating-amazon-dynamoDB/image27.png)


### 3.Cấu hình phân quyền IAM ROLE cho AWS Lambda

Mở Terminal/ PowerShell trên máy local có AWS CLI đã cấu hình


![image28.png](/images/5-Workshop/5.4-Backend-deployment/5.4.2-creating-amazon-dynamoDB/image28.png)

