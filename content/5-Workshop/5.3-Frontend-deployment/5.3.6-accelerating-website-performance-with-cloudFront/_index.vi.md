---
title : "Tăng tốc Website với CloudFront"
date : 2024-01-01 
weight : 6
chapter : false
pre : " <b> 5.3.6. </b> "
---
### 1. Chặn tất cả public access vào S3
- Trong giao diện S3 bucket:
  - Chọn tab ***Permissions***
  - Hiện tại sẽ thấy ***Block all public access***: ***Off***
  - Chọn ***Edit*** trong phần ***Block public access (bucket settings)***
![permission-overview](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/permission-overview.png)
  - Sau đó, chọn ***Block all public access***
  - Chọn ***Save changes***
![edit-block-public-access](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/edit-block-public-access.png)
  - Trong cửa sổ xác nhận, nhập ```confirm```
  - Sau đó, chọn ***confirm***
![confirm-edit-block-public-access](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/confirm-edit-block-public-access.png)
  - Kiểm tra ***Block all public access*** đã được ***On***
![block-public-access-on](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/block-public-access-on.png)
- Kiểm tra chức năng của Block all public access:
  - Trong giao diện S3 bucket, chọn tab ***Properties***
![properties](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/properties.png)
  - Cuộn xuống cuối trang, tại mục ***Static website hosting***, copy URL
![copy-URL](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/copy-URL.png)
  - Mở URL tại cửa sổ ẩn danh mới
  - Kết quả cho thấy Block all public access thành công
![block-all-public-access-success](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/block-all-public-access-success.png)
### 2. Cấu hình Amazon Cloudfront
- Tìm kiếm ```Cloudfront``` trong thanh tìm kiếm ***AWS Console***
![find-cloudfront](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/find-cloudfront.png)
- Chọn ***Create distribution***
![create-distribution](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/create-distribution.png)
- Tại giao diện Get started:
  - Distribution name: Nhập ```aws-smartdocsai-cloud-distribution```
  - Sau đó chọn ***Next***
![get-started](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/get-started.png)
- Trong giao diện Specify origin:
  - Origin type: Chọn ***Amazon S3***
  - Origin: S3-origin nhấn nút ***Browse S3***
![specify-origin](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/specify-origin.png)
  - Chọn ***S3 bucket aws-smartdocsai-cloud***
  - Nhấn nút ***Choose***
![select-s3-location](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/select-s3-location.png)
  - S3 origin đã chọn:
![s3-origin-choosed](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/s3-origin-choosed.png)
  - Cuộn đến cuối trang và nhấn ***Next***
![s3-origin-next](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/s3-origin-next.png)
- Trong giao diện Enable security
  - Web Application Firewall (WAF): Chọn ***Do not enable security protections***
  - Sau đó, nhấn nút ***Next***
![enable-security](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/enable-security.png)
- Trong giao diện Review and create:
  - Kiểm tra lại các thông tin Cloudfront
  - Sau đó nhấn nút ***Create distribution***
![review-and-create-distribution](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/review-and-create-distribution.png)
- Sau đó, sẽ chuyển hướng đến trang thông tin Cloudfront với trạng thái ***Deploying***
![cloudfront-deploying](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/cloudfront-deploying.png)
- Đợi vài phút, trạng thái sẽ chuyển sang thời gian được cập nhật
![cloudfront-deploy-success](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/cloudfront-deploy-success.png)
- Quay lại giao diện S3 bucket:
  - Chọn tab ***Permissions***
![permission-return](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/permission-return.png)
  - Cuộn xuống trong mục ***Bucket policy***, xem các thông tin quan trọng
![bucket-policy](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/bucket-policy.png)
### 3. Cấu hình Default root object
- Trong giao diện Cloudfront:
  - Chọn tab ***General***
  - Setting: Chọn ***Edit***
![default-root-general](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/default-root-general.png)
- Trong giao diện Edit Setting
  - Default root object - optional: Nhập ```index.html```
![default-root-setting](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/default-root-setting.png)
  - Cuộn đến cuối trang và nhấn ***Save changes***
![default-root-save](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/default-root-save.png)
- Kiểm tra Default root object đã hiện ***index.html***
![check-default-root-edit](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/check-default-root-edit.png)
### 4. Tạo Error pages
- Trong Distributions của Cloudfront:
  - Chọn tab ***Error pages***
  - Nhấn ***Create custom error response***
![create-custom-error](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/create-custom-error.png)
- Trong trang Create custom error response:
  - HTTP error code: Chọn ***403:Forbidden***
  - Error caching minimum TTL: Nhập ```10```
  - Customize error response: Chọn ***Yes***
  - Response page path: Nhập ```/index.html```
  - HTTP Response code: Chọn ***200: OK***
  - Sau đó, nhấn ***Create custom error response***
![create-first-custom-error](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/create-first-custom-errorr.png)
  - Tạo tương tự cho Error response 2 với các thông tin như ảnh bên dưới
![create-second-custom-error](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/create-second-custom-errorr.png)
- Kiểm tra các Error pages đã tạo
![check-list-custom-error](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/check-list-custom-error.png)
### 5. Kiểm tra cấu hình Amazon Cloudfront
- Trong giao diện Cloudfront, chọn ******
![cloudfront-id](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/cloudfront-id.png)
- Tại mục Distribution domain name, ***copy URL***
![copy-url-cloudfront](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/copy-url-cloudfront.png)
- Mở URL vào một tab khác của trình duyệt
![check-cloudfront-url](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/check-cloudfront-url.png)