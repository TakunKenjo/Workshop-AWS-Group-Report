---
title : "Creating AWS Lambda Engine"
date : 2024-01-01 
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

### 1. Install Docker Desktop

#### 1.1. Requirements
- **Operating System: Windows 10 (64-bit: Home, Pro, Enterprise Build 19041 or higher) or Windows 11.**
- **Hardware Virtualization Enabled: Intel VT-x or AMD-V enabled in BIOS/UEFI.**
- **RAM: Minimum 4 GB (8 GB – 16 GB recommended).**
- **Free Disk Space: Minimum 10 GB.**

#### 1.2. Enable WSL 2 (WINDOWS SUBSYSTEM FOR LINUX 2)
Docker Desktop on Windows leverages the WSL 2 Engine for native Linux container performance.

##### 1.2.1. Open PowerShell with Administrator privileges (Run as Administrator)


![image29.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image29.png)


##### 1.2.2. Run command to enable WSL and Virtual Machine Platform features


![image30.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image30.png)


##### 1.2.3. Restart your computer

##### 1.2.4. Reopen PowerShell (Admin) and set WSL 2 as the default version


![image31.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image31.png)


#### 1.3. Download and Install official Docker Desktop

##### 1.3.1. Access official Docker website: https://www.docker.com/products/docker-desktop/


![image32.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image32.png)


##### 1.3.2. Click "Download for Windows" to download `Docker Desktop Installer.exe`

##### 1.3.3. Open the downloaded `.exe` file to proceed with installation:
- Check "Use WSL 2 instead of Hyper-V (recommended)".
- Check "Add shortcut to desktop".
- Click OK and wait for file extraction to finish.

##### 1.3.4. Click "Close and restart" to apply system changes.

#### 1.4. Initial Launch and Configuration of Docker Desktop

##### 1.4.1. Open Docker Desktop application


![image33.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image33.png)


##### 1.4.2. Select Accept to agree to Subscription Service Agreement terms.

##### 1.4.3. In main Docker Desktop UI, select Settings (Gear icon) ➔ General:
- Ensure "Use the WSL 2 based engine" is checked.


![image34.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image34.png)


##### 1.4.4. Observe bottom-left corner of UI: Green Docker icon with "Engine Running" text indicates Docker is ready.


![image35.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image35.png)


### 2. Write and Optimize Dockerfile


![image36.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image36.png)


### 3. Create Amazon ECR Repository (via AWS CLI)

Open PowerShell and execute command:


![image37.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image37.png)


### 4. Authenticate Docker CLI to Amazon ECR (AUTHENTICATION)

Since Amazon ECR requires temporary security token authentication (valid for 12 hours), fetch a temporary password from AWS ECR and pipe directly to Docker Login:


![image38.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image38.png)


Result: "Login Succeeded" confirms success.

### 5. Build Docker Image and Push to Amazon ECR

#### 5.1. Build Docker Image

Enter command in Terminal:


![image39.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image39.png)


#### 5.2. Push Docker Image to ECR Repository


![image40.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image40.png)


### 6. Synchronize Docker Image with AWS Lambda

Open terminal and enter CLI command to synchronize:


![image41.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image41.png)


Result:


![image42.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image42.png)


### 1. Develop Backend Mangum Handler & Configuration

#### 1.1. Configure temporary storage path in `backend/config.py`

AWS Lambda is Stateless and only permits writing temporary data to `/tmp`:


![image43.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image43.png)


#### 1.2. Create Mangum Handler in `backend/app_api.py`

Use `Mangum` library to convert HTTP Requests from AWS API Gateway to FastAPI ASGI Format:


![image44.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image44.png)


### 2. Prepare DOCKERFILE for AWS Lambda

Create lightweight `backend/Dockerfile` (~1.0 GB), excluding heavy PyTorch or model weights:


![image45.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image45.png)


### 3. Create IAM ROLE and Permissions for AWS Lambda

Lambda requires interaction permissions with S3, DynamoDB, Bedrock, and CloudWatch.

#### 3.1. Create Trust Policy (`trust-policy.json`):


![image46.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image46.png)


#### 3.2. Run AWS CLI commands to create Role and attach Managed Policies:


![image47.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image47.png)


### 4. Package Container & Push Docker Image to Amazon ECR


![image48.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image48.png)


### 5. Initialize AWS Lambda Function

### 1. Open Terminal or PowerShell


![image49.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image49.png)


### 2. Initialize Lambda Function via AWS CLI

#### 2.1. Create Lambda Function from ECR Container Image

This function acts as Backend Core handling all API logic and AI RAG:


![image50.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image50.png)


Next, configure all 7 Environment Variables:


![image51.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image51.png)


Grant API Gateway permissions to invoke this Lambda function:


![image52.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image52.png)


#### 2.2. Create Lambda Function from Source Zip File

This function serves as Cognito Pre Sign-up Trigger to clean unverified email users:


![image53.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image53.png)


Then grant Cognito User Pool permissions to invoke this Lambda function:


![image54.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image54.png)


Result:


![image55.png](/images/5-Workshop/5.4-Backend-deployment/5.4.4-creating-AWS-lambda/image55.png)
