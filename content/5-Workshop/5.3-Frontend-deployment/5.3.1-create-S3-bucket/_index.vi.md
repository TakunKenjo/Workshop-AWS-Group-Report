---
title : "Tạo S3 Bucket và tải dữ liệu"
date : 2024-01-01 
weight : 1
chapter : false
pre : " <b> 5.3.1. </b> "
---

### 1. Truy cập dịch vụ S3
- Gõ ```S3``` trong thanh tìm kiếm AWS Console
- Chọn ***S3***
![find-s3](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/find-s3.png)
### 2. Khởi tạo Bucket
- Trong giao diện S3, chọn ***Create bucket***
![create-bucket](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/create-bucket.png)
### 3. Cấu hình Cài đặt Bucket cơ bản
- Trong giao diện Create bucket:
  - Bucket namespace: Chọn ***Global namespace***
  - Bucket name: Nhập ```aws-smartdocsai-cloud```
  - Object Ownership: Chọn ***ACLs disabled (recommended)***
![general-config](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/general-config.png)
### 4. Cấu hình cài đặt truy cập công khai
- Chọn ***Block all public access*** (cho tất cả các truy cập công khai)
![block-public-access](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/block-public-access.png)
### 5. Tùy chọn cấu hình bổ sung
- Bucket Versioning: ***Disable***
- Tag, Default encryption, Advanced settings: giữ mặc định
- Kiểm tra và chọn ***Create bucket***
![optional-config](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/optional-config.png)
### 6. Xác minh và kiểm tra Bucket đã tạo
- Thông báo đã tạo bucket thành công
![success-create-bucket](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/success-create-bucket.png)
- Kiểm tra thông tin các bucket đã tạo trong mục ***Amazon S3 > Buckets***
![check-info-bucket](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/check-info-bucket.png)
### 7. Tải dữ liệu lên S3 bucket vừa tạo
- Trong terminal code, khi chạy lệnh ```npm run build``` sẽ tạo ra thư mục ***dist/*** trong chứa các file, folder:
![dist](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/dist.png)
- Trong giao diện S3 bucket vừa tạo:
  - Hiện tại chưa có object nào
  - Chọn ***Upload*** để tài dữ liệu
![upload](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/upload.png)
- Trong giao diện Upload:
  - Bật cửa sổ chứa folder, file cần tải lên
![folder-dialog](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/folder-dialog.png)
  - Chọn các file, folder có trong dist/
![choose-file](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/choose-file.png)
  - Kéo, thả toàn bộ các file, folder vào mục upload của S3 Bucket 
![drag-file](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/drag-file.png)
- Kết quả khi thả các file, folder vào S3 Bucket:
![result-drag-file](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/result-drag-file.png)
- Cuộn xuống cuối trang và chọn ***Upload***
![upload-final](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/upload-final.png)
- Đợi upload dữ liệu và hiển thị thông báo upload thành công
![upload-file-success](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/upload-file-success.png)
- Quan sát file, folder đã tải lên
![info-file-upload](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/info-file-upload.png)






  






