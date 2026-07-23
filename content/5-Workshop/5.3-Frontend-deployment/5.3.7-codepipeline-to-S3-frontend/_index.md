---

title : "Automatic Deployment with CodePipeline (GitHub → S3)"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.3.7. </b> "
---

### 1. Create a CodePipeline connected to GitHub

* Search for `CodePipeline` in the ***AWS Console*** search bar.
* Select ***CodePipeline***.
  ![find-code-pipeline](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/find-code-pipeline.png)
* Select ***Create pipeline***.
  ![create-pipeline](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/create-pipeline.png)
* In the Choose creation option interface:

  * Category: Select ***Build custom pipeline***.
  * Then, click ***Next***.
    ![Choose-creation-option](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/Choose-creation-option.png)
* In the Pipeline settings interface:

  * Pipeline name: Enter `smartdocsai-fe-pipeline`.
  * Execution mode: Select ***Queued***.
  * Service role: Select ***New service role***.
  * Role name: Enter `smartdocsai-de-role`.
  * Then, click ***Next***.
    ![pipeline-setting](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/pipeline-setting.png)
* In the Add source stage interface:

  * Source provider: Select ***GitHub (via GitHub App)***.
  * Connection: Click ***Connect to GitHub***.
    ![add-source-stage](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/add-source-stage.png)
* After clicking ***Connect to GitHub***:

  * The ***Create a connection*** window will appear.
  * Connection name: Enter `smartdocsai-github`.
  * Click ***Connect to GitHub***.
    ![create-a-connection](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/create-a-connection.png)
* Click ***Authorize***.
  ![authorize](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/authorize.png)
* In the Connect to GitHub interface:

  * Click ***Install a new app***.
    ![install-a-new-app](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/install-a-new-app.png)
  * Select ***All repositories***.
  * Then, click ***Install & Authorize***.
    ![install-and-authorize](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/install-and-authorize.png)
* Enter the confirmation code to verify the connection.
  ![confirm-access](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/confirm-access.png)
* After the connection is successfully established:

  * Return to the Connect to GitHub interface.
  * App installation - optional: Select the ***GitHub account that was just connected***.
  * Then, click ***Connect***.
    ![connect-github](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/connect-github.png)
* The connection is successfully established.
  ![connect-github-success](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/connect-github-success.png)
* In the Add source stage interface:

  * Connection: Select the ***connection that was just created***.
  * Repository name: Select the ***SmartdocAI-AWS*** repository (the team's project).
  * Default branch: Select **main**.
  * Output artifact format: Select ***CodePipeline default***.
  * Select ***Enable automatic retry on stage failure***.
    ![add-source-stage2](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/add-source-stage2.png)
  * Scroll to the bottom of the page and click ***Next***.
    ![add-source-stage-next](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/add-source-stage-next.png)
* In the Add build stage - optional interface:

  * Build provider: Select ***Other build providers***.
  * Select: Choose ***AWS CodeBuild***.
  * Project name: Click ***Create project***.
    ![add-build-stage](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/add-build-stage.png)
* Project configuration:

  * Project name: Enter `smartdocsai-fe-build`.
  * Project type: Select ***Default project***.
    ![project-config](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/project-config.png)
* Environment:

  * Operating system: Select ***Ubuntu***.
  * Image: Select ***aws/codebuild/standard:8.0***.
    ![envi1](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/envi1.png)
  * Service role: Select ***new service role***.
  * Role name: Enter `smartdocsai-codebuild-role`.
    ![envi2](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/envi2.png)
* Buildspec:

  * Build specification: Select ***Use a buildspec file***.
  * Buildspec name - optional: Enter `smart-docs-ai/smart-docs-ai/buildspec.yml`.
    ![buildspec](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/buildspec.png)
* In the source code:

  * Create the `buildspec.yml` file in the location shown in the image below.
  * Enter the file content as shown in the image.
  * Then, push the code to GitHub and make sure that the `buildspec.yml` file with all required content is available in the GitHub repository.
    ![content-buildspec](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/content-buildspec.png)
* In Logs:

  * Select ***CloudWatch logs - optional***.
  * Group name - optional: Enter `/aws/codebuild/smartdocsai-fe-build`.
  * Then, click ***Continue to CodePipeline***.
    ![logs](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/logs.png)
* In the Add build stage - optional interface:

  * Project name: Select the ***smartdocsai-fe-build*** project that was just created.
  * Build type: Select ***Single build***.
    ![single-build](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/single-build.png)
  * Scroll to the bottom of the page and click ***Next***.
    ![single-build-next](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/single-build-next.png)
* In the Add test stage - optional interface:

  * Select ***Skip test stage***.
    ![add-test-stage](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/add-test-stage.png)
* In the Add deploy stage interface:

  * Provider: Select ***Amazon S3***.
  * Bucket: Select the ***Frontend S3 bucket - aws-smartdocsai-cloud***.
  * Deployment path - optional: Enter `smartdocsai-fe.zip`.
  * Select ***Extract file before deploy***.
  * Click ***Next***.
    ![add-deploy-stage](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/add-deploy-stage.png)
* In the Review interface:

  * Review the CodePipeline configuration.
    ![review-pipeline](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/review-pipeline.png)
  * Scroll to the bottom of the page and click ***Create pipeline***.
    ![step6-create-pipeline](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/step6-create-pipeline.png)
* The pipeline will display a ***build/deploy*** status.
  ![build-deploy](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/build-deploy.png)
* The CodePipeline deployment completes successfully.
  ![deploy-success](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/deploy-success.png)
* **Note:** Delete the old files in the Frontend S3 bucket. When CodePipeline deploys the application, the files will be recreated automatically.
  ![delete-old-file](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/delete-old-file.png)

### 2. Create an Origin Path for CloudFront

* In the CloudFront interface:

  * Select the ***Origins*** tab.
  * Select the CloudFront distribution as shown in the image below.
  * Click ***Edit***.
    ![tab-origin](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/tab-origin.png)
* In the Edit origin interface:

  * Origin path: Enter `/smartdocsai-fe-zip`.
    ![origin-path](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/origin-path.png)
  * Scroll to the bottom of the page and click ***Save changes***.
    ![save-origin-path](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/save-origin-path.png)

### 3. Create a Manual Invalidation for CloudFront

* In the CloudFront page:

  * Select the ***Invalidations*** tab.
  * Click ***Create invalidation***.
    ![tab-invalidation](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/tab-invalidation.png)
* In the Create invalidation interface:

  * Selection method: Select **By paths**.
  * Object paths to invalidate: Enter `/*`.
  * Then, click **Create invalidation**.
    ![create-invalidation](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/create-invalidation.png)
  * A notification confirms that the invalidation was created successfully.
    ![create-invalidation-success](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/create-invalidation-success.png)
* In the CloudFront interface, verify that the Origin path has been updated.
  ![origin-path-updated](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/origin-path-updated.png)

### 4. Create an Automatic Invalidation for CloudFront

This step grants the CodeBuild service role the `cloudfront:CreateInvalidation` permission.

* Access the CloudFront distribution and copy the Distribution ID.
  ![distribution-id](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/distribution-id.png)
* Search for `IAM` in the **AWS Console** search bar.
  ![find-IAM](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/find-IAM.png)
* In the IAM interface:

  * Select **Roles**.
  * Select the **codebuild-smartdocsai-fe-build-service-role** role.
    ![role-name](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/role-name.png)
* In the selected role:

  * Select **Add permissions**.
    ![permission-tab](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/permission-tab.png)
  * Select **Create inline policy**.
    ![create-inline-policy](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/create-inline-policy.png)
* In the Specify permissions interface:

  * Select **JSON**.
  * Enter the following policy in the Policy editor:

    ```
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": "cloudfront:CreateInvalidation",
                "Resource": "arn:aws:cloudfront::623035187993:distribution/E5QRZJSUK0QTJ"
            }
        ]
    }
    ```

![specify-permission](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/specify-permission.png)

* Scroll to the bottom of the page and click **Next**.
  ![specify-permission-next](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/specify-permission-next.png)
* In the Review and create interface:

  * Policy name: Enter `AllowCloudfrontInvalidatio`.
  * Then, click **Create policy**.
    ![review-and-create-policy](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/review-and-create-policy.png)
* After completing this step, the role has permission to create CloudFront invalidations.
  ![policy-invalidation-success](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/policy-invalidation-success.png)
* Edit the **buildspec.yml** file in the repository and add the invalidation command:

  ```
  version: 0.2


  phases:
  install:
      commands:
      # Navigate to the directory containing the FE source code
      - cd smart-docs-ai/smart-docs-ai
      - npm ci


  build:
      commands:
      # Build the application into the dist directory
      - npm run build
  post_build:
      commands:
      # ONLY run CloudFront cache invalidation after the build phase completes successfully
      - aws cloudfront create-invalidation --distribution-id E5QRZJSUK0QTJ --paths "/*"


  artifacts:
  base-directory: smart-docs-ai/smart-docs-ai/dist
  files:
      - '**/*'
  ```

### 5. Verify the Website with CodePipeline

* Access CloudFront and copy the Distribution domain name.
  ![copy-cloudfront-domain](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/copy-cloudfront-domain.png)
* Paste the URL into a new browser tab.

  * At this point, pay attention to the **"Đăng ký ngay"** text in the login form.
    ![before-fe](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/before-fe.png)
* Modify the code from **"Đăng ký ngay"** to **"Đăng ký ngay!"** by adding an exclamation mark (`!`).
* Then, push the code to GitHub.
  ![code-edit](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/code-edit.png)
* Wait for a while for CodePipeline to redeploy the application. The interface will then be updated according to the new code.
  ![after-fe](/images/5-Workshop/5.3-Frontend-deployment/5.3.7-codepipeline-to-S3-frontend/after-fe.png)
