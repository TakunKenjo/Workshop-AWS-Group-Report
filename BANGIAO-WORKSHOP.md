# BÀN GIAO WORKSHOP - SYSTEM TESTING & CONCLUSION

**Ngày:** 22/07/2026  
**Người thực hiện:** LZNT  
**Phần phụ trách:** 5.5 System Testing + 5.6 Conclusion

---

## ✅ CÔNG VIỆC ĐÃ HOÀN THÀNH

### 1. Refactor Workshop Content (Phase 1-4)

#### Phase 1: Fix Code Syntax Highlighting
- ❌ Vấn đề: JSON code blocks với inline comments `#` không render đúng
- ✅ Giải pháp: Xóa comments, tạo bảng riêng để giải thích
- 📁 Files: `5.1.4-aws-services-used/_index.vi.md`

#### Phase 2: Replace Unprofessional Emojis
- ❌ Vấn đề: 47 emoji instances (✅❌ℹ️🔥💡) không chuyên nghiệp
- ✅ Giải pháp: Thay bằng `[YES]`, `[NO]`, `[NOTE]`, `[ERROR]`, `[OK]`

#### Phase 3: Add Visual Diagrams
- ❌ Vấn đề: Workshop quá nhiều code, thiếu sơ đồ
- ✅ Giải pháp: 
  - Thêm 9 image placeholders vào `5.1.3-overall-aws-architecture/_index.vi.md`
  - Tạo file `MERMAID_DIAGRAMS.md` với 10 diagram codes
  - Chuyển tất cả markdown images sang HTML `<img>` tags với width control

#### Phase 4: Clean Unnecessary Content
- 🗑️ Đã xóa từ 5.5: Load Testing (~90 lines), Testing Checklist (~50 lines), VPC Endpoint (~77 lines) = **217+ lines**
- 🗑️ Đã xóa từ 5.6: PowerShell script (~200 lines), 618 duplicate lines = **~800 lines**
- 📊 Tối giản cleanup flowchart từ 17 steps → 3 phases

---

### 2. Restructure Sections 5.5 & 5.6 (HOÀN THÀNH)

#### ✅ Section 5.5 - System Testing
**Trước:** 1 file lớn (495 lines) với 6 sections

**Sau:** Cấu trúc phân cấp với 6 subsections

```
5.5-System-testing/
├── _index.vi.md (Overview - 80 lines)
├── 5.5.1-Authentication/
│   └── _index.vi.md (14 test cases, JWT validation, EventBridge)
├── 5.5.2-Document-RAG/
│   └── _index.vi.md (12 test cases, S3 upload, RAG modes comparison)
├── 5.5.3-Security/
│   └── _index.vi.md (XSS, CORS, JWT, validation tables)
├── 5.5.4-Profile/
│   └── _index.vi.md (13 CRUD test cases)
├── 5.5.5-Monitoring/
│   └── _index.vi.md (CloudWatch Logs + Insights queries)
└── 5.5.6-CICD/
    └── _index.vi.md (Pytest + CodeBuild integration)
```

#### ✅ Section 5.6 - Conclusion
**Trước:** 1 file lớn (~950 lines) với 10 sections

**Sau:** Cấu trúc phân cấp với 3 subsections

```
5.6-Conclusion/
├── _index.vi.md (Giới thiệu tổng quan - 40 lines)
├── 5.6.1-Summary-Cost/
│   └── _index.vi.md (Workshop summary + Cost breakdown)
├── 5.6.2-Cleanup/
│   └── _index.vi.md (Cleanup flow + Verification + Troubleshooting)
└── 5.6.3-Next-Steps/
    └── _index.vi.md (Production tips + Resources + Feedback)
```

---

### 3. Hugo Site Status

✅ **Hugo server:** Đang chạy tại `http://localhost:1313/`  
✅ **Build status:** 223 pages (112 EN + 111 VI)  
✅ **Live reload:** Enabled, tự động rebuild khi có thay đổi  
✅ **Subsections:** Đã xuất hiện trong sidebar navigation  

---

## 📋 CÔNG VIỆC CẦN LÀM

### 🎯 NHIỆM VỤ CHÍNH: CHỤP SCREENSHOTS (15-20 ảnh)

**Lưu ý quan trọng:**
- Backend Deployment có 55 ảnh vì **TẠO resources** qua GUI step-by-step
- System Testing chỉ cần **15-20 ảnh** vì **VERIFY kết quả** qua terminal/API
- Tỉ lệ: **70% text (tables, commands) - 30% ảnh (results)**

---

### 📸 DANH SÁCH SCREENSHOTS CHI TIẾT

#### **5.5.1 Authentication Testing** (3-4 ảnh)

**Save to:** `static/images/5-Workshop/5.5-System-testing/5.5.1-Authentication/`

| # | Tên file | Nội dung cần chụp | Cách chụp |
|---|----------|-------------------|-----------|
| 1 | `registration-success.png` | Terminal với `curl -X POST .../auth/register` → JSON response `{"user_id": "...", "email": "..."}` | Terminal: Chạy curl command → Screenshot kết quả 200 OK |
| 2 | `email-confirmation.png` | Email inbox với mã xác nhận 6 số | Gmail/Outlook: Mở email từ no-reply@verificationemail.com → Screenshot |
| 3 | `login-jwt-tokens.png` | Terminal với `curl -X POST .../auth/login` → JSON với `id_token`, `access_token`, `refresh_token` | Terminal: Chạy curl login → Screenshot 3 JWT tokens |
| 4 | `cognito-user-pool.png` | AWS Console: Cognito User Pool với danh sách users | Console: Cognito > User pools > smartdocai-pool > Users tab → Screenshot table |

**Reference trong markdown:**
```markdown
![Registration Success](/images/5-Workshop/5.5-System-testing/5.5.1-Authentication/registration-success.png)
```

---

#### **5.5.2 Document Upload & RAG** (3-4 ảnh)

**Save to:** `static/images/5-Workshop/5.5-System-testing/5.5.2-Document-RAG/`

| # | Tên file | Nội dung cần chụp | Cách chụp |
|---|----------|-------------------|-----------|
| 1 | `presigned-url-response.png` | Terminal với `curl GET /documents/upload-url?filename=test.pdf` → JSON với `upload_url` (S3 presigned URL) | Terminal: curl GET → Screenshot JSON response |
| 2 | `s3-uploaded-file.png` | AWS Console: S3 bucket `smartdocai-storage-xxx` với file đã upload | Console: S3 > Buckets > smartdocai-storage > Objects tab → Screenshot uploaded PDF |
| 3 | `rag-query-response.png` | Terminal với `curl POST /chat` → JSON với `answer` + `sources` (5 chunks) | Terminal: curl POST với query → Screenshot RAG answer + similarity scores |
| 4 | `rag-performance-chart.png` | (Optional) Chart so sánh 3 modes: Standard (5-8s) vs Self-RAG (8-12s) vs Co-RAG (10-15s) | Excel/Google Sheets: Tạo bar chart → Screenshot |

---

#### **5.5.3 Security Testing** (2 ảnh)

**Save to:** `static/images/5-Workshop/5.5-System-testing/5.5.3-Security/`

| # | Tên file | Nội dung cần chụp | Cách chụp |
|---|----------|-------------------|-----------|
| 1 | `xss-blocked.png` | Terminal với `curl -X POST .../auth/register` với `fullname: "<script>alert(1)</script>"` → 400 Bad Request | Terminal: curl với XSS payload → Screenshot error response |
| 2 | `cors-preflight.png` | Browser DevTools Network tab: OPTIONS request với CORS headers (`Access-Control-Allow-Origin`) | Browser: F12 > Network > Filter OPTIONS → Screenshot CORS headers |

---

#### **5.5.4 Profile Testing** (2 ảnh)

**Save to:** `static/images/5-Workshop/5.5-System-testing/5.5.4-Profile/`

| # | Tên file | Nội dung cần chụp | Cách chụp |
|---|----------|-------------------|-----------|
| 1 | `profile-get-response.png` | Terminal với `curl GET /profile` + Bearer token → JSON với user data (email, fullname, phone, dob) | Terminal: curl GET với JWT → Screenshot profile data |
| 2 | `dynamodb-profile-item.png` | AWS Console: DynamoDB table `smartdocai-user-profiles` với user item | Console: DynamoDB > Tables > smartdocai-user-profiles > Explore items → Screenshot 1 item |

---

#### **5.5.5 Monitoring & Logging** (3-4 ảnh) 🔥 QUAN TRỌNG NHẤT

**Save to:** `static/images/5-Workshop/5.5-System-testing/5.5.5-Monitoring/`

| # | Tên file | Nội dung cần chụp | Cách chụp |
|---|----------|-------------------|-----------|
| 1 | `cloudwatch-logs-insights.png` | AWS Console: CloudWatch Logs Insights với query `fields @timestamp, @message | filter @message like /ERROR/` | Console: CloudWatch > Logs Insights > Run query → Screenshot results table |
| 2 | `lambda-metrics-dashboard.png` | AWS Console: Lambda metrics graphs (Invocations, Errors, Duration, Throttles) | Console: Lambda > smartdocai > Monitor tab → Screenshot 4 graphs |
| 3 | `cloudwatch-tail-logs.png` | Terminal với `aws logs tail /aws/lambda/smartdocai --follow` (real-time logs) | Terminal: aws logs tail → Screenshot 10-20 log lines |
| 4 | `eventbridge-rule-active.png` | AWS Console: EventBridge rule `smartdocai-cleanup-unconfirmed` (Status: ENABLED, Schedule: rate(5 minutes)) | Console: EventBridge > Rules > smartdocai-cleanup → Screenshot rule details |

---

#### **5.5.6 CI/CD Testing** (1-2 ảnh)

**Save to:** `static/images/5-Workshop/5.5-System-testing/5.5.6-CICD/`

| # | Tên file | Nội dung cần chụp | Cách chụp |
|---|----------|-------------------|-----------|
| 1 | `pytest-results.png` | Terminal với `pytest backend/test_*` → Output `60 passed in 12.34s` | Terminal: pytest command → Screenshot test summary |
| 2 | `codebuild-test-phase.png` | AWS Console: CodeBuild build logs với "Running tests" phase (pytest output) | Console: CodeBuild > Build history > View logs → Screenshot TEST phase |

---

#### **5.6.2 Cleanup Resources** (2-3 ảnh)

**Save to:** `static/images/5-Workshop/5.6-Conclusion/`

| # | Tên file | Nội dung cần chụp | Cách chụp |
|---|----------|-------------------|-----------|
| 1 | `s3-empty-bucket.png` | Terminal với `aws s3 rm s3://smartdocai-storage-xxx --recursive` → Output "delete: s3://..." (nhiều dòng) | Terminal: aws s3 rm → Screenshot deletion progress |
| 2 | `cloudfront-disabled.png` | AWS Console: CloudFront distribution với Status "Disabled" | Console: CloudFront > Distributions > Select distribution → Screenshot Status column |
| 3 | `billing-zero-cost.png` | (Optional) AWS Billing dashboard với $0.00 sau cleanup | Console: Billing > Bills → Screenshot current month $0 |

---

### 📁 TẠO FOLDERS CHO IMAGES

**Chạy lệnh PowerShell:**
```powershell
cd "c:\Users\lznt\Downloads\AWS-TT\Workshop-AWS-Group-Report\static\images\5-Workshop"

# Tạo folders cho 5.5 subsections
New-Item -ItemType Directory -Force -Path "5.5-System-testing/5.5.1-Authentication"
New-Item -ItemType Directory -Force -Path "5.5-System-testing/5.5.2-Document-RAG"
New-Item -ItemType Directory -Force -Path "5.5-System-testing/5.5.3-Security"
New-Item -ItemType Directory -Force -Path "5.5-System-testing/5.5.4-Profile"
New-Item -ItemType Directory -Force -Path "5.5-System-testing/5.5.5-Monitoring"
New-Item -ItemType Directory -Force -Path "5.5-System-testing/5.5.6-CICD"

# Folder 5.6 đã có sẵn (cleanup-resources-flow.png)
# Kiểm tra: ls 5.6-Conclusion
```

---

### 🔧 CẬP NHẬT MARKDOWN FILES (SAU KHI CÓ ẢNH)

**Ví dụ: 5.5.1-Authentication/_index.vi.md**

Thêm screenshots vào các vị trí phù hợp:

```markdown
### 1.1. Registration Testing

**Test case: Valid registration**
```bash
curl -X POST https://d60866voq5.execute-api.us-east-1.amazonaws.com/prod/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "SecurePass123!",
    "fullname": "Nguyen Van Test",
    "phone": "0901234567",
    "dob": "1990-01-15"
  }'
```

**Expected response:**
![Registration Success](/images/5-Workshop/5.5-System-testing/5.5.1-Authentication/registration-success.png)

---

### 1.2. Email Confirmation

Kiểm tra email inbox để lấy mã xác nhận 6 số:

![Email Confirmation Code](/images/5-Workshop/5.5-System-testing/5.5.1-Authentication/email-confirmation.png)

---

### 1.3. Login & JWT Tokens

**Test case: Successful login**
```bash
curl -X POST https://.../prod/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email": "test@example.com", "password": "SecurePass123!"}'
```

**Response với JWT tokens:**
![JWT Tokens Response](/images/5-Workshop/5.5-System-testing/5.5.1-Authentication/login-jwt-tokens.png)

---

### 1.4. Verify trong AWS Console

Kiểm tra Cognito User Pool:

![Cognito User Pool Users](/images/5-Workshop/5.5-System-testing/5.5.1-Authentication/cognito-user-pool.png)
```

---

## 📊 TỔNG KẾT SCREENSHOTS

| Section | Screenshots | Priority |
|---------|-------------|----------|
| 5.5.1 Authentication | 3-4 ảnh | HIGH |
| 5.5.2 Document RAG | 3-4 ảnh | HIGH |
| 5.5.3 Security | 2 ảnh | MEDIUM |
| 5.5.4 Profile | 2 ảnh | MEDIUM |
| 5.5.5 Monitoring | 3-4 ảnh | **CRITICAL** (cần nhiều nhất) |
| 5.5.6 CI/CD | 1-2 ảnh | MEDIUM |
| 5.6.2 Cleanup | 2-3 ảnh | LOW (optional) |
| **TOTAL** | **15-20 ảnh** | |

---

## 🎯 WORKFLOW ĐỀ XUẤT

### Bước 1: Setup Environment (30 phút)
1. Deploy SmartDocAI lên AWS (nếu chưa có)
2. Tạo test user account
3. Upload 1-2 PDF documents
4. Cài đặt AWS CLI + configure credentials

### Bước 2: Chụp Screenshots (1-2 giờ)
1. **Authentication (30 phút):**
   - Chạy curl register → Screenshot
   - Check email → Screenshot
   - Chạy curl login → Screenshot
   - Mở Cognito Console → Screenshot

2. **Document RAG (30 phút):**
   - Chạy curl presigned URL → Screenshot
   - Upload file → Check S3 → Screenshot
   - Chạy curl RAG query → Screenshot

3. **Monitoring (30 phút):** 🔥 QUAN TRỌNG
   - Mở CloudWatch Logs Insights → Screenshot
   - Mở Lambda Metrics → Screenshot
   - Chạy aws logs tail → Screenshot
   - Check EventBridge rule → Screenshot

4. **Security/Profile/CI/CD (30 phút):**
   - Chạy curl XSS test → Screenshot
   - Check DynamoDB → Screenshot
   - Chạy pytest → Screenshot

### Bước 3: Tích hợp vào Markdown (30 phút)
1. Tạo folders cho images
2. Copy screenshots vào folders
3. Update markdown files với image references
4. Hugo rebuild → Preview tại localhost:1313
5. Verify tất cả ảnh hiển thị đúng

---

## 📝 GHI CHÚ BỔ SUNG

### Files cần giữ nguyên (KHÔNG SỬA)
- ✅ `5.5-System-testing/_index.vi.md` (overview đã sửa vào 15:57)
- ✅ `5.6-Conclusion/_index.vi.md` (overview đã sửa vào 15:58)
- ✅ Tất cả 6 subsections trong 5.5.1 → 5.5.6 (đã split xong)
- ✅ Tất cả 3 subsections trong 5.6.1 → 5.6.3 (đã split xong)

### Files đã pull từ main (22/07/2026 16:00)
- ✅ Section 5.1.5 - UI Function (mới thêm)
- ✅ Backend Deployment content updates (5.4.1 → 5.4.5)
- ✅ 55 screenshots trong `static/images/5-Workshop/5.4-Backend-deployment/`

### Untracked files (chưa commit)
- ⚠️ `MERMAID_DIAGRAMS.md` (10 diagram codes)
- ⚠️ `BANGIAO-WORKSHOP.md` (file này)
- ⚠️ Tất cả subsections 5.5.1 → 5.5.6, 5.6.1 → 5.6.3

---

## ✅ CHECKLIST HOÀN THÀNH

- [ ] Tạo folders cho screenshots (6 folders cho 5.5 + 1 folder 5.6)
- [ ] Chụp 15-20 screenshots theo table trên
- [ ] Copy images vào folders tương ứng
- [ ] Update markdown files với image references
- [ ] Verify Hugo site hiển thị ảnh đúng
- [ ] Generate 10 PNG diagrams từ `MERMAID_DIAGRAMS.md` (optional, có thể làm sau)
- [ ] Commit & push changes lên GitHub
- [ ] Tạo Pull Request để merge vào main

---

**Next Session Focus:**
1. Chụp screenshots (ưu tiên 5.5.5 Monitoring trước - quan trọng nhất)
2. Tích hợp ảnh vào markdown
3. Verify Hugo build

**Thời gian ước tính còn lại:** 2-3 giờ
