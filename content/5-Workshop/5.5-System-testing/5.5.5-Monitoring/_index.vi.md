---
title : "Giám sát & Nhật ký hệ thống"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5.5 </b> "
---

Phần này hướng dẫn cách theo dõi sức khỏe hệ thống SmartDocAI qua CloudWatch Logs và Metrics: đọc log thời gian thực, viết truy vấn Logs Insights để phân tích lỗi/hiệu năng, và kiểm tra các chỉ số Lambda quan trọng như số lần gọi, thời gian xử lý và tỷ lệ lỗi.

### 1. Tổng quan CloudWatch Logs

**Log Group:** `/aws/lambda/smartdocai`

**Key Log Patterns:**
- `[Auth]` → Authentication events (login, JWT validation)
- `[Document]` → Upload/indexing operations
- `[RAG]` → Query processing & Bedrock calls
- `[Cleanup]` → EventBridge cleanup results
- `ERROR` → Application errors
- `Traceback` → Python exceptions

**Lệnh xem log thời gian thực:** `aws logs tail /aws/lambda/smartdocai --follow --region us-east-1`

**Ví dụ log thực tế:**
```
2024-03-15T10:30:45.456Z [INFO] User login: test@example.com
2024-03-15T10:30:45.789Z [INFO] JWT validated successfully
2024-03-15T10:30:50.123Z [INFO] RAG query processed: 5 chunks retrieved
2024-03-15T10:30:50.456Z [INFO] Bedrock LLM response: 234 tokens generated
2024-03-15T10:30:50.999Z REPORT RequestId: abc123 Duration: 5234 ms Memory: 2048 MB Max Used: 1234 MB
```

---

### 2. Truy vấn CloudWatch Insights

| Tên truy vấn | Mục đích | Câu lệnh | Kết quả mong đợi |
|------------|---------|------------|-----------------|
| **Error Count (24h)** | Count errors by hour | `fields @timestamp, @message`<br/>`\| filter @message like /ERROR/`<br/>`\| stats count() as error_count by bin(1h)`<br/>`\| sort @timestamp desc` | Hourly error histogram (should be <1% of invocations) |
| **Average Lambda Duration** | Performance metrics | `fields @duration`<br/>`\| stats avg(@duration) as avg_ms,`<br/>`max(@duration) as max_ms,`<br/>`pct(@duration, 99) as p99_ms` | avg: 500-5000ms, max: <10s, p99: <8s |
| **Top 10 Slowest Invocations** | Find performance bottlenecks | `fields @timestamp, @duration, @message`<br/>`\| filter @type = "REPORT"`<br/>`\| sort @duration desc`<br/>`\| limit 10` | List of longest-running requests (cold starts: 2-3s) |
| **Cleanup Results** | EventBridge cleanup stats | `fields @timestamp, @message`<br/>`\| filter @message like /Deleted.*users/`<br/>`\| parse @message "Deleted * users" as deleted_count`<br/>`\| stats sum(deleted_count) as total_deleted by bin(1d)` | Daily count of unconfirmed users deleted |
| **Most Active Users** | User activity ranking | `fields @message`<br/>`\| filter @message like /User login:/`<br/>`\| parse @message "User login: *" as email`<br/>`\| stats count() as login_count by email`<br/>`\| sort login_count desc \| limit 10` | Top 10 users by login frequency |

---

### 3. Chỉ số Lambda (Metrics)

**Bảng các chỉ số CloudWatch:**

| Chỉ số | Command AWS CLI | Giá trị mong đợi (hệ thống khỏe mạnh) |
|--------|-----------------|----------------------------------|
| **Invocations** | `aws cloudwatch get-metric-statistics`<br/>`--namespace AWS/Lambda --metric-name Invocations`<br/>`--dimensions Name=FunctionName,Value=smartdocai`<br/>`--start-time <1h ago> --period 300 --statistics Sum` | 100-1000/hour (depends on traffic) |
| **Errors** | `aws cloudwatch get-metric-statistics`<br/>`--namespace AWS/Lambda --metric-name Errors`<br/>`--dimensions Name=FunctionName,Value=smartdocai`<br/>`--start-time <1h ago> --period 300 --statistics Sum` | <1% of total invocations |
| **Throttles** | `aws cloudwatch get-metric-statistics`<br/>`--namespace AWS/Lambda --metric-name Throttles`<br/>`--dimensions Name=FunctionName,Value=smartdocai`<br/>`--start-time <1h ago> --period 300 --statistics Sum` | 0 (should never throttle with provisioned concurrency) |
| **Duration** | `aws cloudwatch get-metric-statistics`<br/>`--namespace AWS/Lambda --metric-name Duration`<br/>`--dimensions Name=FunctionName,Value=smartdocai`<br/>`--start-time <1h ago> --period 300 --statistics Average,Max` | Avg: 500ms-5s<br/>Cold start: 2-3s<br/>Warm: <1s |

**Lưu ý:** Thay `<1h ago>` bằng timestamp thực tế theo định dạng: `$(date -u -d '1 hour ago' +%Y-%m-%dT%H:%M:%S)`

---