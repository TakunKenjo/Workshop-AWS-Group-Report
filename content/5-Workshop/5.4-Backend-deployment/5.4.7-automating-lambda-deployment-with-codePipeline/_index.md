---

title : "Automate Lambda Deployment with CodePipeline"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.4.7 </b> "
---

### 1. Verify the Lambda Function

* Search for `Lambda` in the **AWS Console** search bar.
  ![find-lambda](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/find-lambda.png)
* Verify that the **smartdocai** function has already been created, then click on the **function**.
  ![function](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/function.png)
* Check the **Image URI**.
  ![image-URI](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/image-URI.png)

### 2. Create a CodePipeline Connected to Lambda

**Step 1: Create `backend/buildspec.yml`**

* In the **backend/** directory, create a `buildspec.yml` file with the following content, then push the code to Git:

```
version: 0.2


env:
  variables:
    AWS_REGION: us-east-1
    IMAGE_REPO_NAME: smartdocai
    IMAGE_TAG: latest
    LAMBDA_FUNCTION_NAME: smartdocai




phases:


  pre_build:
    commands:
      - echo Login ECR
      - ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
      - REPOSITORY_URI=$ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$IMAGE_REPO_NAME
      - aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $REPOSITORY_URI




  build:
    commands:
      - echo Building Docker image
      - docker build -t $IMAGE_REPO_NAME:$IMAGE_TAG -f backend/Dockerfile backend
      - docker tag $IMAGE_REPO_NAME:$IMAGE_TAG $REPOSITORY_URI:$IMAGE_TAG




  post_build:
    commands:
      - echo Push image
      - docker push $REPOSITORY_URI:$IMAGE_TAG
      - echo Deploy Lambda
      - aws lambda update-function-code --function-name $LAMBDA_FUNCTION_NAME --image-uri $REPOSITORY_URI:$IMAGE_TAG
```

![file-buildspec](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/file-buildspec.png)

**Step 2: Create a CodePipeline**

* Navigate to **CodePipeline** and click **Create pipeline**.
  ![codepipeline](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/codepipeline.png)
* In the Choose creation option interface:

  * Select **Build custom pipeline**.
  * Click **Next**.
    ![choose-creation-option](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/choose-creation-option.png)
* In the Choose pipeline setting interface:

  * Pipeline name: Enter `smartdocai-be-pipeline`.
  * Role name: Enter `AWSCodePipelineServiceRole-us-east-1-smartdocai-be-pipeline`.
    ![choose-pipeline-setting](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/choose-pipeline-setting.png)
* In the Add source stage interface:

  * Source provider: Select **GitHub (via GitHub App)**.
  * Connection: Select the **connection** created during the CodePipeline-to-S3 Frontend integration.
  * Repository: Select **TakunKenjo/SmartdocAI-AWS**.
  * Default branch: Select **main**.
  * Output artifact format: Select **CodePipeline default**.
    ![add-source-stage1](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/add-source-stage1.png)
  * Scroll to the bottom of the page and click **Next**.
    ![add-source-stage2](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/add-source-stage2.png)
* In the Add build stage - optional interface:

  * Build provider: Select **Other build providers**.
  * Select: Choose **AWS CodeBuild**.
  * Project name: Click **Create project**.
    ![add-build-stage](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/add-build-stage.png)

**Step 3: Create the Backend CodeBuild Project**

* In the Create build project interface:

  * Project name: Enter `smartdocai-be-build`.
  * Project type: Select **Default project**.
    ![create-build-project](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/create-build-project.png)
  * Operating system: Select **Ubuntu**.
  * Runtime(s): Select **Standard**.
  * Image: Select **aws/codebuild/standard:8.0**.
    ![envi](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/envi.png)
  * Service role: Select **New service role**.
  * Role name: Enter `codebuild-smartdocai-be-build-service-role`.
    ![service-role](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/service-role.png)
  * Privileged: Check **Enable this flag…**.
    ![privi](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/privi.png)
  * Build specifications: Select **Use a buildspec file**.
  * Buildspec name - optional: Enter `backend/buildspec.yml`.
    ![buildspec-name](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/buildspec-name.png)
  * Scroll to the bottom of the page and click **Continue to CodePipeline**.
    ![continue-to-codepipeline](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/continue-to-codepipeline.png)

**Step 4: Continue Creating the CodePipeline**

* In the Add build stage - optional interface:

  * Project name: Select the newly created **CodeBuild** project: **smartdocai-be-build**.
  * Scroll to the bottom of the page and click **Next**.
    ![add-build-stage-final](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/add-build-stage-final.png)
* In the Add test stage - optional interface:

  * Click **Skip test stage**.
    ![add-test-stage](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/add-test-stage.png)
* In the Add deploy stage interface:

  * Click **Skip deploy stage**.
    ![add-deploy-stage](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/add-deploy-stage.png)
* In the Review interface:

  * Review the Pipeline configuration.
    ![review-pipeline](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/review-pipeline.png)
  * Scroll to the bottom of the page and click **Create pipeline**.

**Step 5: Grant ECR Permissions to the CodeBuild IAM Role**

* Navigate to IAM > Roles:

  * Select **codebuild-smartdocai-be-service-role**.
    ![IAM-role](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/IAM-role.png)
  * In the Permissions tab, click **Add permissions**.
    ![tab-permission](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/tab-permission.png)
  * Select **Attach policies**.
    ![attach-policy](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/attach-policy.png)
* Search for: **AmazonEC2ContainerRegistryPowerUser**

  * Check **AmazonEC2ContainerRegistryPowerUser**.
  * Then, click **Add permissions**.
    ![add-permission](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/add-permission.png)
* The ECR permission policy has been successfully created.
  ![permission-policy-success](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/permission-policy-success.png)

**Step 6: Add `lambda:UpdateFunctionCode` Permission**

* After adding ECR permissions, the role also needs the **lambda:UpdateFunctionCode** permission.
* Click **Add permissions** and select **Create inline policy**.
  ![create-inline-policy](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/create-inline-policy.png)
* In the Specify permissions page:

  * Select **JSON**.
  * Enter the following policy in the editor:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "lambda:UpdateFunctionCode"
      ],
      "Resource": "*"
    }
  ]
}
```

![json](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/json.png)

* Scroll to the bottom of the page and click **Next**.
  ![json-next](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/json-next.png)
* In the Review and create interface:

  * Policy name: Enter `smartdocai-be-codebuild-ecr-lambda-policy`.
  * Click **Create policy**.
    ![create-inline-policy2](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/create-inline-policy2.png)
* A notification confirms that the **permission policy** was created successfully.
  ![permission-policy-inline-success](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/permission-policy-inline-success.png)

**Step 7: Verify CodePipeline**

* Select the **smartdocai-be-pipeline** pipeline and verify that the **pipeline build** has completed successfully.
  ![pipeline-be-success](/images/5-Workshop/5.4-Backend-deployment/5.4.7-automating-lambda-deployment-with-codePipeline/pipeline-be-success.png)
