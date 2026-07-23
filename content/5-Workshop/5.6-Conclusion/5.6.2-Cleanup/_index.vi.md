---
title : "Cleanup Resources"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.6.2 </b> "
---

## Dọn dẹp tài nguyên AWS

### **WARNING:** QUAN TRỌNG - Đọc trước khi xóa

**CẢNH BÁO:**
- **[NO] DATA LOSS:** Xóa tài nguyên sẽ **MẤT HẾT DỮ LIỆU** (user profiles, documents, logs)
- **[NO] KHÔNG THỂ PHỤC HỒI:** Không có backup/snapshot được tạo trong workshop này
- **[YES]** Nếu muốn giữ lại data:
  - Export DynamoDB table trước khi xóa
  - Download S3 objects (documents + avatars)
  - Export CloudWatch logs nếu cần analyze

---

### Cleanup Flow - Thứ tự xóa tài nguyên

**Flowchart:** 3 phases cleanup (Stop Traffic → Remove Compute → Delete Data)

<img src="/images/5-Workshop/5.6-Conclusion/cleanup-resources-flow.png" width="75%" style="max-width:1000px">

**Cleanup Summary:**

| Phase | Resources | Key Actions | Time |
|-------|-----------|-------------|------|
| **Phase 1: Stop Traffic** | EventBridge, CloudFront, API Gateway | Disable rule → Disable CloudFront (⏱️ wait 30min) → Delete API Gateway | ~35 min |
| **Phase 2: Remove Compute** | CodePipeline, CodeBuild, Lambda, ECR | Delete CI/CD pipeline → Delete Lambda function → Delete Docker images | ~5 min |
| **Phase 3: Delete Data** | S3 Buckets (2), DynamoDB, Cognito, IAM | Empty S3 → Delete buckets → Delete user data → Delete permissions | ~5 min |

**Total Time:** ~45 minutes (chủ yếu chờ CloudFront disable)

---

### Example: Delete Resources với AWS CLI

**1. Delete EventBridge Rule:**
```powershell
# Disable rule
aws events disable-rule --name smartdocai-cleanup-unconfirmed --region us-east-1

# Remove Lambda target
aws events remove-targets --rule smartdocai-cleanup-unconfirmed --ids "1" --region us-east-1

# Delete rule
aws events delete-rule --name smartdocai-cleanup-unconfirmed --region us-east-1
```

**2. Delete CloudFront (requires 30+ min wait):**
```powershell
$distId = "E1234ABCD5678"  # Replace with your distribution ID
aws cloudfront get-distribution-config --id $distId --output json > dist-config.json
# Edit dist-config.json: Set "Enabled": false
aws cloudfront update-distribution --id $distId --if-match <ETag> --distribution-config file://dist-config.json
# Wait until status = Disabled (check console every 5 min)
aws cloudfront delete-distribution --id $distId --if-match <new-ETag>
```

**3. Empty & Delete S3 Bucket:**
```powershell
$bucket = "smartdocai-storage-623035187993"
aws s3 rm s3://$bucket --recursive --region us-east-1  # Empty bucket
aws s3api delete-bucket --bucket $bucket --region us-east-1  # Delete bucket
```

**4. Delete Lambda Function:**
```powershell
aws lambda delete-function --function-name smartdocai --region us-east-1
```

**5. Delete DynamoDB Table (with backup):**
```powershell
# Optional: Backup first
aws dynamodb scan --table-name smartdocai-user-profiles --region us-east-1 > backup.json
# Delete table
aws dynamodb delete-table --table-name smartdocai-user-profiles --region us-east-1
```

**6. Delete Cognito User Pool:**
```powershell
aws cognito-idp delete-user-pool --user-pool-id us-east-1_3oq5wIiuu --region us-east-1
```

**Full list of commands:** Tham khảo AWS Documentation hoặc file `CLEANUP_GUIDE.md` trong source code

---

## Verify Cleanup Completed

**Check AWS Console manually:**

## Verify Cleanup Completed

**Checklist:**

```powershell
# 1. EventBridge
aws events list-rules --region us-east-1 | Select-String "smartdocai"
# Expected: No results

# 2. CloudFront
aws cloudfront list-distributions --query "DistributionList.Items[?contains(Comment, 'smartdocai')]" --output table
# Expected: Empty

# 3. API Gateway
aws apigateway get-rest-apis --region us-east-1 | Select-String "smartdocai"
# Expected: No results

# 4. Lambda
aws lambda list-functions --region us-east-1 | Select-String "smartdocai"
# Expected: No results

# 5. ECR
aws ecr describe-repositories --region us-east-1 | Select-String "smartdocai"
# Expected: No results

# 6. S3
aws s3 ls | Select-String "smartdocai"
# Expected: No results

# 7. DynamoDB
aws dynamodb list-tables --region us-east-1 | Select-String "smartdocai"
# Expected: No results

# 8. Cognito
aws cognito-idp list-user-pools --max-results 60 --region us-east-1 | Select-String "smartdocai"
# Expected: No results

# 9. CloudWatch Logs
aws logs describe-log-groups --region us-east-1 | Select-String "smartdocai"
# Expected: No results

# 10. IAM Roles
aws iam list-roles | Select-String "smartdocai"
# Expected: No results
```

**Expected final state:** Tất cả commands trên trả về empty/no results

---

## Billing Estimate After Cleanup

**Remaining charges (nếu có):**
- CloudFront distribution (nếu chưa xóa): ~$0.01/day
- CloudWatch Logs (nếu chưa xóa): ~$0.001/day per GB stored
- S3 Glacier transitions (nếu có lifecycle policy): Varies

**Zero charges sau khi cleanup hoàn tất:**
- **[OK]** Lambda: Deleted (no more invocations)
- **[OK]** API Gateway: Deleted (no more requests)
- **[OK]** S3: Deleted (no more storage)
- **[OK]** DynamoDB: Deleted (no more read/write capacity)
- **[OK]** Bedrock: Pay-per-use (no idle cost)
- **[OK]** ECR: Deleted (no more image storage)
- **[OK]** CodePipeline: Deleted (no active pipeline)
- **[OK]** CodeBuild: Deleted (no more builds)

**Check final bill after 24-48h:**
```powershell
# View current month billing
aws ce get-cost-and-usage `
  --time-period Start=$(Get-Date -Format yyyy-MM-01),End=$(Get-Date -Format yyyy-MM-dd) `
  --granularity MONTHLY `
  --metrics BlendedCost `
  --region us-east-1
```

---

## Troubleshooting Common Issues

### Issue 1: "Cannot delete S3 bucket - not empty"

**Solution:**
```powershell
# Force empty bucket (including versioned objects)
aws s3api delete-objects `
  --bucket smartdocai-storage-623035187993 `
  --delete "$(aws s3api list-object-versions --bucket smartdocai-storage-623035187993 --query '{Objects: Versions[].{Key:Key,VersionId:VersionId}}' --output json)"

# Then delete bucket
aws s3api delete-bucket --bucket smartdocai-storage-623035187993 --region us-east-1
```

### Issue 2: "Cannot delete IAM role - policy still attached"

**Solution:**
```powershell
# List attached policies
aws iam list-attached-role-policies --role-name smartdocai-lambda-role

# Detach each policy
aws iam detach-role-policy `
  --role-name smartdocai-lambda-role `
  --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole

# Delete inline policies (nếu có)
aws iam list-role-policies --role-name smartdocai-lambda-role
aws iam delete-role-policy --role-name smartdocai-lambda-role --policy-name policy-name

# Then delete role
aws iam delete-role --role-name smartdocai-lambda-role
```

### Issue 3: "CloudFront distribution still deploying"

**Solution:**
- Đợi 5-10 phút cho distribution status = "Deployed"
- Không thể disable khi status = "In Progress"
- Sau khi disabled, đợi thêm 15-30 phút mới có thể delete

### Issue 4: "EventBridge rule has targets"

**Solution:**
```powershell
# Remove targets first
aws events remove-targets `
  --rule smartdocai-cleanup-unconfirmed `
  --ids "1" `
  --region us-east-1

# Then delete rule
aws events delete-rule --name smartdocai-cleanup-unconfirmed --region us-east-1
```

---