---
title : "Tự động triển khai bằng CodePipeline (GitHub → S3)"
date : 2024-01-01 
weight : 7
chapter : false
pre : " <b> 5.3.7. </b> "
---
### 1. Tạo CodePipeline kết nối Github
- Tìm kiếm ```CodePipeLine``` trong công cụ tìm kiếm ***AWS Console***
- Chọn ***CodePipeLine***
![find-code-pipeline](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/find-code-pipeline.png)
- Chọn ***Create pipeline***
![create-pipeline](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/create-pipeline.png)
- Trong giao diện Choose creation option:
  - Category: Chọn ***Build custom pipeline***
  - Sau đó, nhấn ***Next***
![Choose-creation-option](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/Choose-creation-option.png)
- Trong giao diện Pipeline settings:
  - Pipeline name:  Nhập ```smartdocsai-fe-pipeline```
  - Execution mode: Chọn ***Queued***
  - Service role: Chọn ***New service role***
  - Role name: Nhập ```smartdocsai-de-role```
  - Sau đó, nhấn nút ***Next***
![pipeline-setting](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/pipeline-setting.png)
- Trong giao diện Add source stage:
  - Source provider: Chọn ***Github (via Github App)***
  - Connection: Nhấn nút ***Connect to Github***
![add-source-stage](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/add-source-stage.png)
- Sau khi nhấn nút ***Connect to Github***:
  - Sẽ xuất hiện cửa sổ ***Create a connection***
  - Connection name: Nhập ```smartdocsai-github```
  - Nhấn nút ***Connect to Github***
![create-a-connection](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/create-a-connection.png)
- Nhấn nút ***Authorize***
![authorize](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/authorize.png)
- Trong giao diện Connect to Github:
  - Nhấn ***Install a new app***
![install-a-new-app](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/install-a-new-app.png)
  - Chọn ***All repositories***
  - Sau đó nhấn nút ***Install & Authorize***
![install-and-authorize](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/install-and-authorize.png)
- Nhập mã xác nhận kết nối
![confirm-access](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/confirm-access.png)
- Sau khi kết nối thành công:
  - Quay trở lại giao diện Connect to Github
  - App installation - optional: Chọn ***tài khoản github vừa kết nối***
  - Sau đó nhấn nút ***Connect***
![connect-github](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/connect-github.png)
- Kết quả kết nối thành công
![connect-github-success](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/connect-github-success.png)
- Trong giao diện Add source stage:
  - Connection: Chọn ***connection vừa tạo***
  - Repository name: Chọn ***repo SmartdocAI-AWS*** (dự án của nhóm)
  - Default branch: Chọn **main**
  - Output artifact format: Chọn ***CodePipeline default***
  - Chọn ***Enable automatic retry on stage failure***
![add-source-stage2](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/add-source-stage2.png)
  - Cuộn đến cuối trang và chọn ***Next***
![add-source-stage-next](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/add-source-stage-next.png)
- Trong giao diện Add build stage - optional:
  - Build provider: Chọn ***Other build providers***
  - Select: Chọn ***AWS CodeBuild***
  - Project name: Nhấn nút ***Create project***
![add-build-stage](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/add-build-stage.png)
- Project configuration:
  - Project name: Nhập ```smartdocsai-fe-build```
  - Project type: Chọn ***Default project***
![project-config](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/project-config.png)
- Environment:
  - Operating system: Chọn ***Ubuntu***
  - Image: Chọn ***aws/codebuild/standard:8.0***
![envi1](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/envi1.png)
  - Service role: Chọn ***new service role***
  - Role name: Nhập ```smartdocsai-codebuild-role```
![envi2](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/envi2.png)
- Buildspec:
  - Build specification: Chọn ***Use a buildspec file***
  - Buildspec name - optional: Nhập ```smart-docs-ai/smart-docs-ai/buildspec.yml```
![buildspec](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/buildspec.png)
- Trong src code:
  - Tạo file ```buildspec.yml``` ở vị trí như ảnh bên dưới
  - Nhập nội dung file như ảnh
  - Sau đó, push code lên github, đảm bảo trên Github đã có file buildspec.yml với đầy đủ nội dung
![content-buildspec](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/content-buildspec.png)
- Trong Logs:
  - Chọn ***CloudWatch logs-optional***
  - Group name - optional: Nhập ```/aws/codebuild/smartdocsai-fe-build```
  - Sau đó nhấn nút ***Continue to CodePipeline***
![logs](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/logs.png)
- Tại giao diện Add build stage - optional:
  - Project name: Chọn ***project name vừa tạo smartdocsai-fe-build***
  - Build type: Chọn ***Single build***
![single-build](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/single-build.png)
  - Cuộn đến cuối trang và chọn ***Next***
![single-build-next](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/single-build-next.png)
- Tại giao diện Add test stage - optional:
  - Chọn ***Skip test stage***
![add-test-stage](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/add-test-stage.png)
- Trong trang Add deploy stage:
  - provider: Chọn ***Amazon S3***
  - Bucket: Chọn ***bucket S3 của fe - aws-smartdocsai-cloud***
  - Deployment path - optional: Nhập ```smartdocsai-fe.zip```
  - Chọn ***Extract file before deploy***
  - Chọn ***Next***
![add-deploy-stage](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/add-deploy-stage.png)
- Trong giao diện Review:
  - Xem lại thông tin CodePipeline
![review-pipeline](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/review-pipeline.png)
  - Cuộn đến cuối trang, nhấn ***Create pipeline***
![step6-create-pipeline](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/step6-create-pipeline.png)
- Hiển thị trạng thái đang ***build/deploy***
![build-deploy](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/build-deploy.png)
- Kết quả deploy thành công CodePipeline
![deploy-success](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/deploy-success.png)
- Lưu ý xóa các file cũ trong S3 bucket frontend, khi deploy codePipeline các file sẽ tự tạo lại
![delete-old-file](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/delete-old-file.png)
### 2. Tạo Origin path cho Cloudfront
- Tại giao diện Cloudfront:
  - Chọn tab ***Origins***
  - Chọn cloudfront như ảnh bên dưới
  - Chọn ***Edit***
![tab-origin](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/tab-origin.png)
- Tại giao diện Edit origin:
  - Origin path: Nhập ```/smartdocsai-fe-zip```
![origin-path](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/origin-path.png)
  - Cuộn đến cuối trang và chọn ***Save changes***
![save-origin-path](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/save-origin-path.png)
### 3. Tạo Invalidation thủ công cho Cloudfront
- Trong trang cloudfront:
  - Chọn tab ***Invalidations***
  - Chọn ***Create invalidations***
![tab-invalidation](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/tab-invalidation.png)
- Trong giao diện Create invalidation:
  - Selection method: Chọn **By paths**
  - Object paths to invalidation: Nhập ```/*```
  - Sau đó nhấn **Create invalidation**

  - Thông báo tạo invalidation thành công

- Tại giao diện Cloudfront, kiểm tra lại origin path đã được cập nhật


### 4. Tạo Invalidation tự động cho Cloudfront
Bước này sẽ gán quyền cloudfront: CreateInvalidation cho Service role
- Truy cập distribution của Cloudfront và copy ID

- Tìm kiếm ```IAM``` trong công cụ tìm kiếm **AWS Console**

- Trong giao diện IAM:
  - Chọn **Roles**
  - Chọn **Role name codebuild-smartdocsai-fe-build-service-role**

- Trong Role name vừa chọn:
  - Chọn **add permissions**

  - Chọn **Create inline policy**

- Trong giao diện Specify permissons:
  - Chọn **JSON**
  - Nhập đoạn mã sau nào Policy editor
    ```{
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": "cloudfront:CreateInvalidation",
                "Resource": "arn:aws:cloudfront::623035187993:distribution/E5QRZJSUK0QTJ"
            }
        ]
    }```


  - Cuộn xuống cuối trang, nhấn **Next**

- Trong giao diện Review and create:
  - Policy name: Nhập ```AllowCloudfrontInvalidatio```
  - Sau đó, nhấn **Create policy**

- Xong bước này, role đã có quyền tạo invalidation. 

- Sửa **buildspec.yml** trong repo — thêm lệnh invalidation 
    ```version: 0.2


    phases:
    install:
        commands:
        # Di chuyển vào đúng thư mục chứa source code FE
        - cd smart-docs-ai/smart-docs-ai
        - npm ci


    build:
        commands:
        # Thực hiện build ra thư mục dist
        - npm run build
    post_build:
        commands:
        # CHỈ chạy xóa cache CloudFront tại đây sau khi mọi thứ ở phase build đã hoàn tất thành công
        - aws cloudfront create-invalidation --distribution-id E5QRZJSUK0QTJ --paths "/*"


    artifacts:
    base-directory: smart-docs-ai/smart-docs-ai/dist
    files:
        - '**/*'```


### 5. Kiểm tra website với CodePipeline
- Truy cập Cloudfront và copy Distribution domain name

- Dán URL vào tab mới trình duyệt
  - Khi này hãy để ý chữ **“Đăng ký ngay”** trong form đăng nhập

- Tiến hành sửa code từ **“Đăng ký ngay”** thành **“Đăng ký ngay!” (Thêm dấu !)**
- Sau đó push code lên Github


- Đợi 1 lúc để CodePipeline deploy lại, ta sẽ thấy giao diện đã được cập nhật theo code mới
