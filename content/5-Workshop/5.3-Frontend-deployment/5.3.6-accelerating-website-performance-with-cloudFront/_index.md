---

title : "Accelerate Website Performance with CloudFront"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.3.6. </b> "
---

### 1. Block All Public Access to S3

* In the S3 bucket interface:

  * Select the ***Permissions*** tab.
  * You will see that ***Block all public access*** is currently set to ***Off***.
  * Select ***Edit*** under ***Block public access (bucket settings)***.

![permission-overview](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/permission-overview.png)

* Then, select ***Block all public access***.
* Select ***Save changes***.

![edit-block-public-access](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/edit-block-public-access.png)

* In the confirmation window, enter `confirm`.
* Then, select ***Confirm***.

![confirm-edit-block-public-access](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/confirm-edit-block-public-access.png)

* Verify that ***Block all public access*** is now set to ***On***.

![block-public-access-on](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/block-public-access-on.png)

* Verify the functionality of **Block all public access**:

  * In the S3 bucket interface, select the ***Properties*** tab.

![properties](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/properties.png)

* Scroll to the bottom of the page. Under ***Static website hosting***, copy the URL.

![copy-URL](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/copy-URL.png)

* Open the URL in a new incognito browser window.
* The result shows that **Block all public access** is working successfully.

![block-all-public-access-success](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/block-all-public-access-success.png)

### 2. Configure Amazon CloudFront

* Search for `CloudFront` in the ***AWS Console*** search bar.

![find-cloudfront](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/find-cloudfront.png)

* Select ***Create distribution***.

![create-distribution](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/create-distribution.png)

* In the **Get started** interface:

  * **Distribution name**: Enter `aws-smartdocsai-cloud-distribution`.
  * Then, select ***Next***.

![get-started](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/get-started.png)

* In the **Specify origin** interface:

  * **Origin type**: Select ***Amazon S3***.
  * For **Origin**, select the ***Browse S3*** button.

![specify-origin](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/specify-origin.png)

* Select the ***aws-smartdocsai-cloud*** S3 bucket.
* Select the ***Choose*** button.

![select-s3-location](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/select-s3-location.png)

* The selected S3 origin will be displayed:

![s3-origin-choosed](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/s3-origin-choosed.png)

* Scroll to the bottom of the page and select ***Next***.

![s3-origin-next](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/s3-origin-next.png)

* In the **Enable security** interface:

  * **Web Application Firewall (WAF)**: Select ***Do not enable security protections***.
  * Then, select the ***Next*** button.

![enable-security](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/enable-security.png)

* In the **Review and create** interface:

  * Review the CloudFront configuration.
  * Then, select ***Create distribution***.

![review-and-create-distribution](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/review-and-create-distribution.png)

* You will then be redirected to the CloudFront distribution details page with the status ***Deploying***.

![cloudfront-deploying](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/cloudfront-deploying.png)

* Wait a few minutes for the deployment to complete. The status will be updated accordingly.

![cloudfront-deploy-success](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/cloudfront-deploy-success.png)

* Return to the S3 bucket interface:

  * Select the ***Permissions*** tab.

![permission-return](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/permission-return.png)

* Scroll down to the ***Bucket policy*** section and review the important information.

![bucket-policy](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/bucket-policy.png)

### 3. Configure the Default Root Object

* In the CloudFront interface:

  * Select the ***General*** tab.
  * Under **Settings**, select ***Edit***.

![default-root-general](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/default-root-general.png)

* In the **Edit Setting** interface:

  * **Default root object - optional**: Enter `index.html`.

![default-root-setting](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/default-root-setting.png)

* Scroll to the bottom of the page and select ***Save changes***.

![default-root-save](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/default-root-save.png)

* Verify that the **Default root object** is set to ***index.html***.

![check-default-root-edit](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/check-default-root-edit.png)

### 4. Create Error Pages

* In the CloudFront distribution interface:

  * Select the ***Error pages*** tab.
  * Select ***Create custom error response***.

![create-custom-error](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/create-custom-error.png)

* In the **Create custom error response** page:

  * **HTTP error code**: Select ***403: Forbidden***.
  * **Error caching minimum TTL**: Enter `10`.
  * **Customize error response**: Select ***Yes***.
  * **Response page path**: Enter `/index.html`.
  * **HTTP Response code**: Select ***200: OK***.
  * Then, select ***Create custom error response***.

![create-first-custom-error](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/create-first-custom-error.png)

* Create the second error response similarly using the information shown in the image below.

![create-second-custom-error](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/create-second-custom-error.png)

* Verify the created error pages.

![check-list-custom-error](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/check-list-custom-error.png)

### 5. Verify the Amazon CloudFront Configuration

* In the CloudFront interface, select the distribution.

![cloudfront-id](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/cloudfront-id.png)

* Under **Distribution domain name**, ***copy the URL***.

![copy-url-cloudfront](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/copy-url-cloudfront.png)

* Open the URL in another browser tab to verify the website.

![check-cloudfront-url](/images/5-Workshop/5.3-Frontend-deployment/5.3.6-accelerating-website-performance-with-cloudFront/check-cloudfront-url.png)
