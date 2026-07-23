# DANH SÁCH SCREENSHOT CẦN CHỤP — 5.1.3, 5.1.4, 5.5, 5.6

Tài liệu tổng hợp toàn bộ screenshot còn thiếu cho 4 phần workshop, dùng để bổ sung bằng chứng thực tế bên cạnh các sơ đồ Mermaid đã có sẵn. Mỗi mục gồm: **Tên file**, **Path lưu**, **Mô tả nội dung cần thể hiện**, **Cách thực hiện**.

> **Quy ước path:** Lưu vào `static/images/5-Workshop/<section>/<subsection>/`, tham chiếu trong markdown bằng `/images/5-Workshop/<section>/<subsection>/<file>.png`.

---

## 1. Phần 5.1.3 — Kiến trúc tổng thể trên AWS

Phần này đã có đầy đủ 5 sơ đồ Mermaid (kiến trúc, 3-tier, storage, lambda modules, CI/CD). Bổ sung thêm 2 screenshot Console để chứng minh kiến trúc là **thật**, không chỉ là sơ đồ lý thuyết.

**Lưu vào:** `static/images/5-Workshop/5.1-Workshop-overview/`

| # | Tên file | Nội dung cần thể hiện | Cách thực hiện |
|---|----------|------------------------|-----------------|
| 1 | `resource-groups-tagged.png` | AWS Resource Groups / Tag Editor liệt kê toàn bộ resource có tag `Project=smartdocai` (Lambda, S3, DynamoDB, API Gateway, CloudFront...) trên cùng 1 màn hình | Console: **Resource Groups & Tag Editor** → Search Resources → filter tag `Project=smartdocai` → chụp bảng kết quả |
| 2 | `lambda-function-overview.png` | Trang tổng quan Lambda function `smartdocai`: Runtime = Container Image, Memory, Timeout, Last modified | Console: **Lambda** → `smartdocai` → tab Configuration → chụp phần General configuration |

---

## 2. Phần 5.1.4 — Các dịch vụ AWS được sử dụng

Bổ sung screenshot Console cho từng nhóm dịch vụ chính đã liệt kê trong bảng, xác nhận cấu hình khớp với nội dung đã viết.

**Lưu vào:** `static/images/5-Workshop/5.1-Workshop-overview/5.1.4-aws-services-used/`

| # | Tên file | Nội dung cần thể hiện | Cách thực hiện |
|---|----------|------------------------|-----------------|
| 1 | `s3-buckets-list.png` | 3 bucket: `aws-smartdocsai-cloud`, `codepipeline-...`, `smartdocai-storage-...` cùng Region, Creation date | Console: **S3** → General purpose buckets → chụp bảng danh sách (giống ảnh đã xem trong chat) |
| 2 | `dynamodb-table-encryption.png` | Table `smartdocai-user-profiles` → tab Overview hiển thị Encryption type = **AWS KMS**, key `alias/aws/dynamodb` | Console: **DynamoDB** → Tables → `smartdocai-user-profiles` → tab Additional settings → chụp phần Encryption |
| 3 | `cognito-user-pool-providers.png` | Cognito User Pool `us-east-1_3oq5wIiuu` → tab Sign-in experience hiển thị 2 provider: Cognito user pool + Google | Console: **Cognito** → User pools → `smartdocai...` → Sign-in experience tab → chụp phần Federated sign-in |
| 4 | `bedrock-model-access.png` | Model access đã bật cho `mistral.mixtral-8x7b-instruct-v0:1` và `amazon.titan-embed-text-v2:0` | Console: **Bedrock** → Model access → filter theo 2 model trên → chụp trạng thái "Access granted" |
| 5 | `codepipeline-2-pipelines.png` | 2 pipeline: `smartdocai-be-pipeline` và `smartdocsai-fe-pipeline` cùng trạng thái Succeeded gần nhất | Console: **CodePipeline** → Pipelines → chụp bảng danh sách 2 pipeline |

---

## 3. Phần 5.5 — Kiểm thử hệ thống

### 3.1. 5.5.1 — Authentication Testing (4 ảnh)

**Lưu vào:** `static/images/5-Workshop/5.5-System-testing/5.5.1-Authentication/`

| # | Tên file | Nội dung cần thể hiện | Cách thực hiện |
|---|----------|------------------------|-----------------|
| 1 | `registration-success.png` | Terminal chạy `curl -X POST .../auth/register` → JSON response `{"user_id": "...", "email": "..."}`, HTTP 200 | Terminal: chạy lệnh curl đăng ký user test → chụp response |
| 2 | `email-confirmation-code.png` | Email inbox chứa mã xác nhận 6 số từ Cognito (`no-reply@verificationemail.com`) | Gmail: mở email xác nhận → chụp mã code |
| 3 | `login-jwt-tokens.png` | Terminal chạy `curl -X POST .../auth/login` → JSON chứa `id_token`, `access_token`, `refresh_token` | Terminal: chạy lệnh curl login → chụp 3 token trả về |
| 4 | `cognito-users-list.png` | AWS Console Cognito User Pool → danh sách user, trạng thái CONFIRMED | Console: **Cognito** → User pools → Users tab → chụp bảng danh sách |

### 3.2. 5.5.2 — Document Upload & RAG Testing (4 ảnh)

**Lưu vào:** `static/images/5-Workshop/5.5-System-testing/5.5.2-Document-RAG/`

| # | Tên file | Nội dung cần thể hiện | Cách thực hiện |
|---|----------|------------------------|-----------------|
| 1 | `presigned-url-response.png` | Terminal `curl POST /api/upload-url` → JSON chứa `upload_url` (S3 presigned URL), `s3_key` | Terminal: chạy curl xin presigned URL → chụp response |
| 2 | `s3-document-uploaded.png` | S3 Console: bucket storage → prefix `uploads/{user_id}/` chứa file PDF vừa upload | Console: **S3** → `smartdocai-storage-...` → điều hướng tới prefix uploads → chụp danh sách file |
| 3 | `rag-chat-response.png` | Terminal `curl POST /api/chat` → JSON chứa `answer` + `sources` (số chunk trích dẫn) | Terminal: chạy curl hỏi RAG → chụp answer + sources |
| 4 | `rag-modes-comparison.png` | Bảng/chart so sánh thời gian phản hồi 3 chế độ: Standard vs Self-RAG vs Co-RAG | Excel/Sheets: đo thời gian 3 lần gọi mỗi mode → tạo bar chart đơn giản → chụp |

### 3.3. 5.5.3 — Security Testing (4 ảnh — bao gồm CSRF mới)

**Lưu vào:** `static/images/5-Workshop/5.5-System-testing/5.5.3-Security/`

| # | Tên file | Nội dung cần thể hiện | Cách thực hiện |
|---|----------|------------------------|-----------------|
| 1 | `xss-input-blocked.png` | Terminal `curl POST /auth/register` với `fullname: "<script>alert(1)</script>"` → HTTP 400 + error message | Terminal: chạy curl với payload XSS → chụp lỗi 400 |
| 2 | `cors-preflight-headers.png` | Browser DevTools Network tab: request OPTIONS hiển thị header `Access-Control-Allow-Origin` đúng domain CloudFront | Browser: F12 → Network → filter OPTIONS → chụp Response Headers |
| 3 | `csrf-state-in-url.png` | DevTools Network tab: request `/oauth2/authorize` chứa tham số `state=<UUID>` khi bấm "Đăng nhập bằng Google" | Browser: F12 → Network → tìm request tới `smartdocai-fayrun2026.auth...` → chụp query string có `state` |
| 4 | `csrf-attack-blocked.png` | Trang `/login` hiển thị thông báo lỗi "Phiên đăng nhập Google không hợp lệ hoặc đã hết hạn..." sau khi giả lập sửa tay `state` sai trong URL callback | Browser: làm theo Test 3 đã thực hiện (sửa `state` trong URL `/auth/callback`) → chụp thông báo lỗi hiển thị |

### 3.4. 5.5.4 — Profile Testing (2 ảnh)

**Lưu vào:** `static/images/5-Workshop/5.5-System-testing/5.5.4-Profile/`

| # | Tên file | Nội dung cần thể hiện | Cách thực hiện |
|---|----------|------------------------|-----------------|
| 1 | `profile-get-response.png` | Terminal `curl GET /api/profile` kèm Bearer token → JSON chứa `avatar_url`, thông tin cá nhân | Terminal: chạy curl với JWT thật → chụp response |
| 2 | `dynamodb-profile-item.png` | DynamoDB Console: table `smartdocai-user-profiles` → 1 item cụ thể (`user_id`, `avatar_url`, `updated_at`) | Console: **DynamoDB** → Explore items → chụp 1 item mẫu |

### 3.5. 5.5.5 — Monitoring & Logging (5 ảnh — quan trọng nhất, có thêm CloudWatch Alarms mới)

**Lưu vào:** `static/images/5-Workshop/5.5-System-testing/5.5.5-Monitoring/`

| # | Tên file | Nội dung cần thể hiện | Cách thực hiện |
|---|----------|------------------------|-----------------|
| 1 | `cloudwatch-logs-insights-query.png` | CloudWatch Logs Insights chạy query `fields @timestamp, @message | filter @message like /ERROR/` → bảng kết quả | Console: **CloudWatch** → Logs Insights → chọn log group `/aws/lambda/smartdocai` → chạy query → chụp kết quả |
| 2 | `lambda-metrics-dashboard.png` | Lambda Monitor tab: 4 biểu đồ Invocations, Errors, Duration, Throttles | Console: **Lambda** → `smartdocai` → tab Monitor → chụp 4 graph |
| 3 | `cloudwatch-tail-logs-realtime.png` | Terminal `aws logs tail /aws/lambda/smartdocai --follow` → 10-15 dòng log thời gian thực | Terminal: chạy lệnh tail trong lúc gọi thử API → chụp output |
| 4 | `cloudwatch-alarms-status.png` | **[MỚI]** CloudWatch Alarms Console: 4 alarm (`smartdocai-lambda-errors`, `-duration`, `-throttles`, `-apigateway-5xx`) cùng trạng thái OK/Insufficient Data | Console: **CloudWatch** → Alarms → All alarms → filter `smartdocai` → chụp bảng 4 alarm |
| 5 | `sns-subscription-confirmed.png` | **[MỚI]** SNS Console: topic `smartdocai-alerts` → 1 subscription email đã Confirmed | Console: **SNS** → Topics → `smartdocai-alerts` → tab Subscriptions → chụp trạng thái Confirmed |

### 3.6. 5.5.6 — CI/CD Automated Testing (2 ảnh)

**Lưu vào:** `static/images/5-Workshop/5.5-System-testing/5.5.6-CICD/`

| # | Tên file | Nội dung cần thể hiện | Cách thực hiện |
|---|----------|------------------------|-----------------|
| 1 | `pytest-local-results.png` | Terminal `pytest test_*_unit.py -v` → dòng cuối `60 passed in X.XXs` | Terminal: chạy pytest trong thư mục `backend/` → chụp summary |
| 2 | `codebuild-prebuild-test-phase.png` | CodeBuild build logs: phase `pre_build` hiển thị output pytest chạy trong pipeline thật | Console: **CodeBuild** → `smartdocai-be-build` → Build history → chọn build gần nhất → View logs → chụp đoạn pytest |

---

## 4. Phần 5.6 — Tổng kết & Dọn dẹp tài nguyên

### 4.1. 5.6.1 — Workshop Summary & Cost (1 ảnh)

**Lưu vào:** `static/images/5-Workshop/5.6-Conclusion/5.6.1-Summary-Cost/`

| # | Tên file | Nội dung cần thể hiện | Cách thực hiện |
|---|----------|------------------------|-----------------|
| 1 | `billing-cost-explorer.png` | AWS Cost Explorer: biểu đồ chi phí theo dịch vụ trong tháng chạy workshop (đối chiếu với bảng ước tính $1.37/7 ngày) | Console: **Billing** → Cost Explorer → filter theo khoảng ngày workshop → group by Service → chụp biểu đồ |

### 4.2. 5.6.2 — Cleanup Resources (3 ảnh)

**Lưu vào:** `static/images/5-Workshop/5.6-Conclusion/5.6.2-Cleanup/`

| # | Tên file | Nội dung cần thể hiện | Cách thực hiện |
|---|----------|------------------------|-----------------|
| 1 | `s3-empty-bucket-progress.png` | Terminal `aws s3 rm s3://... --recursive` → nhiều dòng `delete: s3://...` chạy liên tục | Terminal: chạy lệnh xóa object hàng loạt → chụp tiến trình |
| 2 | `cloudfront-status-disabled.png` | CloudFront Console: Distribution status chuyển từ Enabled → Disabled | Console: **CloudFront** → Distributions → chụp cột Status sau khi disable |
| 3 | `resources-deleted-verification.png` | Terminal chạy đủ 6 lệnh verify (EventBridge, CloudFront, API Gateway, Lambda, ECR, S3) → toàn bộ trả về rỗng/No results | Terminal: chạy lần lượt các lệnh trong mục "Kiểm tra xác nhận đã dọn dẹp xong" → chụp toàn bộ output |

### 4.3. 5.6.3 — Next Steps & References (không cần ảnh)

Phần này thuần văn bản (đề xuất cải tiến production), không cần screenshot minh họa.

---

## 5. Tổng hợp số lượng

| Phần | Số ảnh cần chụp | Ưu tiên |
|------|------------------|---------|
| 5.1.3 | 2 | Thấp (đã có 5 diagram) |
| 5.1.4 | 5 | Trung bình |
| 5.5.1 | 4 | Cao |
| 5.5.2 | 4 | Cao |
| 5.5.3 | 4 (2 mới cho CSRF) | Cao |
| 5.5.4 | 2 | Trung bình |
| 5.5.5 | 5 (2 mới cho Alarms/SNS) | **Rất cao** |
| 5.5.6 | 2 | Trung bình |
| 5.6.1 | 1 | Thấp |
| 5.6.2 | 3 | Trung bình |
| **TỔNG** | **32 ảnh** | |

## 6. Sau khi chụp xong

1. Copy ảnh vào đúng thư mục `static/images/5-Workshop/.../` theo path đã ghi ở mỗi mục
2. Thêm reference vào file `.vi.md` tương ứng bằng cú pháp:
   ```markdown
   <img src="/images/5-Workshop/<path>/<file>.png" width="90%" style="max-width:900px">
   ```
3. Hugo server (`hugo server -D`) sẽ tự rebuild — F5 lại trình duyệt để kiểm tra ảnh hiển thị đúng
4. Xóa/che thông tin nhạy cảm trước khi chụp nếu cần (Account ID, token thật) — có thể thay bằng giá trị mẫu nếu lo ngại bảo mật khi đưa vào báo cáo công khai
