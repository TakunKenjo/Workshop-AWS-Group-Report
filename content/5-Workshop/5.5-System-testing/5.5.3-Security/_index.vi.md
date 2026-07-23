---
title : "Security Testing"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.5.3 </b> "
---

## Security Testing

### 3.1. Input Validation Testing

**Validation Rules Table:**

| Field | Valid Examples | Invalid Examples | Error Message |
|-------|----------------|------------------|---------------|
| **Phone** | 0901234567 → +84901234567<br/>+84901234567 (E.164) | "123456" (too short)<br/>"+1234567890" (non-VN)<br/>"abcdefghij" (non-digit) | "Invalid phone format"<br/>"Only Vietnam (+84) supported"<br/>"Phone must contain digits" |
| **DOB** | 1990-01-15<br/>1900-01-01 (minimum)<br/>2026-12-31 (maximum) | 2027-01-01 (future)<br/>1899-12-31 (too old)<br/>1990-13-45 (invalid date) | "DOB cannot be in the future"<br/>"DOB must be after 1900"<br/>"Invalid date format" |
| **Fullname** | "Nguyen Van A"<br/>"Trần Thị Bích Ngọc" (unicode)<br/>"John Doe" | "A" (< 2 chars)<br/>`<script>alert(1)</script>` (XSS)<br/>"x" × 101 (> 100 chars)<br/>"" (empty) | "Min 2 chars"<br/>"XSS prevention"<br/>"Max 100 chars"<br/>"Fullname required" |

**Unit Test Coverage:**
- `backend/test_validators_unit.py` → **50+ tests** covering edge cases
  - TestPhoneValidation (15 tests)
  - TestDOBValidation (12 tests)
  - TestFullnameValidation (10 tests)
  - TestEdgeCases (13 tests)

**Run tests:** `cd backend/ && pytest test_validators_unit.py -v`

---

### 3.2. CORS & JWT Testing

**Test Summary Table:**

| # | Test Case | Input | Expected Result | Status |
|---|-----------|-------|-----------------|--------|
| **CORS VALIDATION** |
| 3.1 | Allowed origin (CloudFront) | Origin: https://dutf3c70nnjzl.cloudfront.net | 200 OK + CORS headers<br/>`Access-Control-Allow-Origin: https://dutf3c...` | PASS |
| 3.2 | Disallowed origin | Origin: https://evil.com | 403 Forbidden (no CORS header) | PASS |
| 3.3 | OPTIONS preflight | OPTIONS request with CORS headers | 200 OK + Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS | PASS |
| **JWT AUTHENTICATION** |
| 3.4 | Missing JWT token | No Authorization header | 401 Unauthorized<br/>Error: "Missing Authorization header" | PASS |
| 3.5 | Invalid JWT format | Bearer invalid_token_xyz | 401 Unauthorized<br/>Error: "Invalid JWT token" | PASS |
| 3.6 | Expired JWT token | JWT with exp < current time | 401 Unauthorized<br/>Error: "JWT token has expired" | PASS |
| 3.7 | JWT from different pool | JWT signed by wrong Cognito pool | 401 Unauthorized<br/>Error: "Invalid JWT issuer" | PASS |

**JWT Validation Flow in Lambda:**
```python
# modules/auth_service.py
def validate_jwt(token: str) -> dict:
    # 1. Decode header to get key ID (kid)
    # 2. Fetch Cognito JWKS from well-known endpoint
    # 3. Verify signature with public key
    # 4. Check exp claim (expiration)
    # 5. Check iss claim (issuer = Cognito pool)
    # 6. Return decoded claims (user_id, email, etc.)
```

**CORS Security Notes:**
- **[YES]** Fixed: CORS wildcard `*` removed
- **[YES]** Only 3 origins allowed: CloudFront + localhost:5173 + localhost:5174
- **[NOTE]** Production: Remove localhost origins before deploy

---

### 3.3. Security Audit Checklist

Reference: `SECURITY_CONSIDERATIONS.md`

| Security Control | Status | Notes |
|-----------------|--------|-------|
| **Authentication** | **Implemented** | Cognito JWT tokens |
| **Authorization** | **Implemented** | Per-user data isolation |
| **Input validation** | **Implemented** | Phone, DOB, fullname validators |
| **XSS prevention** | **Implemented** | `<script>` tag blocked in fullname |
| **CORS restrictions** | **Fixed** | Wildcard removed (commit xyz) |
| **HTTPS only** | **Enforced** | TLS 1.2+ on CloudFront/API Gateway |
| **SQL injection** | **N/A** | No SQL database (DynamoDB) |
| **Rate limiting** | **Not implemented** | 10K RPS API Gateway default |
| **Encryption at rest** | **Not enabled** | S3/DynamoDB no KMS encryption |
| **VPC isolation** | **Not configured** | Lambda in public internet |
| **OAuth state param** | **Missing** | CSRF risk in Google OAuth |
| **MFA** | **Disabled** | Can enable in Cognito |

**Production recommendations:**
1. Enable S3/DynamoDB encryption (KMS)
2. Add API Gateway resource policies (IP whitelist)
3. Implement per-user rate limiting
4. Enable Cognito MFA for sensitive operations
5. Add OAuth state parameter
6. Deploy Lambda in VPC with NAT Gateway

---