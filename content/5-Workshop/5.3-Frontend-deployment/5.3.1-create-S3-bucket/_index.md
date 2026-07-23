---

title : "Create an S3 Bucket and Upload Data"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.3.1. </b> "
---

### 1. Access the S3 Service

* Type `S3` in the AWS Console search bar.
* Select ***S3***.

![find-s3](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/find-s3.png)

### 2. Create a Bucket

* In the S3 interface, select ***Create bucket***.

![create-bucket](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/create-bucket.png)

### 3. Configure Basic Bucket Settings

* In the **Create bucket** interface:

  * **Bucket namespace**: Select ***Global namespace***.
  * **Bucket name**: Enter `aws-smartdocsai-cloud`.
  * **Object Ownership**: Select ***ACLs disabled (recommended)***.

![general-config](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/general-config.png)

### 4. Configure Public Access Settings

* Select ***Block all public access*** to block all public access.

![block-public-access](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/block-public-access.png)

### 5. Configure Additional Settings

* **Bucket Versioning**: ***Disable***.
* **Tag**, **Default encryption**, and **Advanced settings**: Keep the default settings.
* Review the configuration and select ***Create bucket***.

![optional-config](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/optional-config.png)

### 6. Verify and Check the Created Bucket

* A notification confirming that the bucket was created successfully will be displayed.

![success-create-bucket](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/success-create-bucket.png)

* Check the information of the created buckets under ***Amazon S3 > Buckets***.

![check-info-bucket](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/check-info-bucket.png)

### 7. Upload Data to the Newly Created S3 Bucket

* In the code terminal, running the command `npm run build` will create a ***dist/*** directory containing the following files and folders:

![dist](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/dist.png)

* In the newly created S3 bucket interface:

  * The bucket currently does not contain any objects.
  * Select ***Upload*** to upload the data.

![upload](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/upload.png)

* In the **Upload** interface:

  * Open the window containing the folders and files to be uploaded.

![folder-dialog](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/folder-dialog.png)

* Select the files and folders contained in the `dist/` directory.

![choose-file](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/choose-file.png)

* Drag and drop all files and folders into the S3 Bucket upload area.

![drag-file](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/drag-file.png)

* The result after dropping the files and folders into the S3 Bucket:

![result-drag-file](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/result-drag-file.png)

* Scroll to the bottom of the page and select ***Upload***.

![upload-final](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/upload-final.png)

* Wait for the data to finish uploading until the successful upload notification is displayed.

![upload-file-success](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/upload-file-success.png)

* Review the uploaded files and folders.

![info-file-upload](/images/5-Workshop/5.3-Frontend-deployment/5.3.1-create-S3-bucket/info-file-upload.png)
