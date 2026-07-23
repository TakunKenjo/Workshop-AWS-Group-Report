---
title : "CI/CD Automated Testing"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.5.6 </b> "
---

## CI/CD Testing - Automated Unit Tests

### 6.1. Pytest Test Suite

**Test Files:**
- `backend/test_validators_unit.py` → **50+ tests** (phone, DOB, fullname validation)
- `backend/test_auth_service_unit.py` → **10+ tests** (helper functions, edge cases)

**Pytest Commands Table:**

| Command | Purpose | Example Output |
|---------|---------|----------------|
| `pytest test_*_unit.py -v` | Run all unit tests (verbose) | `60 passed in 3.45s` |
| `pytest test_*_unit.py --cov=modules --cov-report=term-missing` | Run with code coverage report | Shows coverage % per module + missing lines |
| `pytest test_validators_unit.py::TestPhoneValidation -v` | Run specific test class | `15 passed in 1.2s` |
| `pytest test_validators_unit.py::TestPhoneValidation::test_vn_mobile_format -v` | Run single test method | `1 passed in 0.5s` |

**Test Coverage Summary:**
- TestPhoneValidation: 15 tests (VN mobile format, E.164, invalid inputs)
- TestDOBValidation: 12 tests (date range, future dates, format validation)
- TestFullnameValidation: 10 tests (XSS prevention, unicode, length limits)
- TestEdgeCases: 13 tests (None values, empty strings, boundary conditions)
- TestHelperFunctions: 10 tests (phone normalization, timestamps)

---

### 6.2. CI/CD Pipeline Integration

**buildspec.yml Phases:**

| Phase | Command | Purpose | Behavior on Failure |
|-------|---------|---------|---------------------|
| **pre_build** | `pip install pytest pytest-cov flake8` | Install test dependencies | FAIL entire build |
| | `flake8 backend/ --exclude=manual_test_*.py --exit-zero` | Lint code (style check) | WARNING only (exit-zero) |
| | `pytest backend/test_*_unit.py -v --tb=short --strict-markers` | **Run unit tests** | **HARD FAIL** → Block deployment |
| **build** | `docker build -t smartdocai backend/` | Build Docker image | FAIL entire build |
| | `docker tag smartdocai:latest $ECR_REPO_URI:latest` | Tag image for ECR | FAIL entire build |
| **post_build** | `docker push $ECR_REPO_URI:latest` | Push image to ECR | FAIL entire build |
| | `aws lambda update-function-code --function-name smartdocai --image-uri $ECR_REPO_URI:latest` | Deploy to Lambda | FAIL entire build |

**Key CI/CD Features:**
- **[YES]** Tests in `pre_build` → Deployment blocked if tests fail
- **[YES]** Flake8 with `--exit-zero` → Don't fail on style warnings
- **[YES]** Pytest `--strict-markers` → Fail on undefined test markers
- **[YES]** `--tb=short` → Concise traceback for faster debugging

---

### 6.3. Debugging CI/CD Issues

**AWS CLI Commands for Build Monitoring:**
- Check recent builds: `aws codebuild list-builds-for-project --project-name smartdocai-be-build --max-items 10 --region us-east-1`
- Get build details: `aws codebuild batch-get-builds --ids <build-id> --region us-east-1`

**Common CI/CD Issues & Best Practices:**
1. **Function signature mismatches** → Always verify function names before writing tests
2. **SyntaxError in comments** → Avoid nested docstrings (Python parser treats them as code)
3. **Complex datetime mocking** → Timezone-aware datetime requires extra setup (pytest-freezegun)
4. **Test assertion failures** → Assertions must match actual implementation, not assumptions
5. **CI/CD hard fail advantage** → Blocks deployment early, prevents production bugs

---