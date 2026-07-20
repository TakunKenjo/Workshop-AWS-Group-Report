---
title : "Bật Static Website Hosting"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

### 1. Truy cập Properties của Bucket
- Trong Amazon S3:
  - Chọn bucket đã tạo trước đó
  - Chọn tab ***Properties***
![properties-bucket](/images/5-Workshop/5.3-Frontend-deployment/5.3.2-enable-static-web-hosting/properties-bucket.png)
### 2. Tìm Static Website Hosting
- Trong giao diện Properties:
  - Cuộn xuống tìm Static Website Hosting
  - Chọn ***Edit***
![find-static-web-hosting](/images/5-Workshop/5.3-Frontend-deployment/5.3.2-enable-static-web-hosting/find-static-web-hosting.png)
### 3. Cấu hình Website Hosting
- Giao diện hiển thị mặc định là Disable
![edit](/images/5-Workshop/5.3-Frontend-deployment/5.3.2-enable-static-web-hosting/edit.png)
- Trong Edit static website hosting:
  - Static website hosting: Chọn ***Enable***
  - Hosting type: Chọn ***Host a static website***
  - Index document: Nhập ```index.html```
  - Error document (optional): Nhập ```error.html```
![edit-static-web-hosting](/images/5-Workshop/5.3-Frontend-deployment/5.3.2-enable-static-web-hosting/edit-static-web-hosting.png)
### 4. Lưu cấu hình
- Kiểm tra cài đặt
- Cuộn xuống dưới và chọn ***Save changes***
![save](/images/5-Workshop/5.3-Frontend-deployment/5.3.2-enable-static-web-hosting/save.png)
### 5. Xác minh cấu hình
- Static website hosting đã được bật, xem các thông tin:
  - ***S3 static website hosting***: Enabled
  - ***Hosting type***: Bucket hosting
  - ***Bucket website endpoint***: Đường dẫn http
![static-web-hosting](/images/5-Workshop/5.3-Frontend-deployment/5.3.2-enable-static-web-hosting/static-web-hosting.png)
