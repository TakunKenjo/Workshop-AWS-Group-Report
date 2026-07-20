---
title : "Cấu hình Bucket Policy/Public Object"
date : 2024-01-01 
weight : 4
chapter : false
pre : " <b> 5.3.4. </b> "
---
### 1. Truy cập quyền Bucket
- Trong giao diện S3, chọn tab ***Permissions***
![tab-permission](/images/5-Workshop/5.3-Frontend-deployment/5.3.4-config-bucket-policy-public-object-access/tab-permission.png)
### 2. Tìm Access Control List
- Cuộn xuống và tìm ***Access Control List***:
  - Kiểm tra thấy Bucket owner enforced
  - Có nghĩa ACL đang bị bắt 
  - Cần bật ACL trước để sử dụng ACL để làm công khai các objects
![find-ACL](/images/5-Workshop/5.3-Frontend-deployment/5.3.4-config-bucket-policy-public-object-access/find-ACL.png)
### 3. Bật ACL
- Trong Object Ownership: Chọn ***Edit***
![edit](/images/5-Workshop/5.3-Frontend-deployment/5.3.4-config-bucket-policy-public-object-access/edit.png)
- Trong giao diện Edit Object Ownership:
  - Object Ownership: Chọn ***ACLs enabled ***
  - Đánh dấu ***I acknowledge that ACLs will be restored***
  - Object Ownership: Chọn ***Bucket owner preferred***
  - Chọn ***Save changes***
![edit-step](/images/5-Workshop/5.3-Frontend-deployment/5.3.4-config-bucket-policy-public-object-access/edit-step.png)
### 4. Xác minh cấu hình ACLs
- Kiểm tra trong phần Ownership sẽ thấy ACLs are enabled
![confirm](/images/5-Workshop/5.3-Frontend-deployment/5.3.4-config-bucket-policy-public-object-access/confirm.png)
### 5. Public đối tượng sử dụng ACL
- Về tại tab Objects của bucket:
  - Chọn các ***objects*** muốn công khai
  - Chọn ***Actions*** từ thanh công cụ
  - Chọn ***Make public using ACL***
![public-ACL](/images/5-Workshop/5.3-Frontend-deployment/5.3.4-config-bucket-policy-public-object-access/public-ACL.png)
### 6. Xác nhận truy cập công khai
- Trong giao diện Make public:
  - Xem lại các đối tượng sẽ được công khai
  - Chọn ***Make public*** để xác nhận
![make-public](/images/5-Workshop/5.3-Frontend-deployment/5.3.4-config-bucket-policy-public-object-access/make-public.png)
### 7. Xác minh cấu hình công khai
- Hiển thị thông báo Make public thành công
![make-public-success](/images/5-Workshop/5.3-Frontend-deployment/5.3.4-config-bucket-policy-public-object-access/make-public-success.png)
