---

title : "Enable Static Website Hosting"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

### 1. Access the Bucket Properties

* In Amazon S3:

  * Select the bucket created previously.
  * Select the ***Properties*** tab.

![properties-bucket](/images/5-Workshop/5.3-Frontend-deployment/5.3.2-enable-static-web-hosting/properties-bucket.png)

### 2. Find Static Website Hosting

* In the **Properties** interface:

  * Scroll down to find **Static Website Hosting**.
  * Select ***Edit***.

![find-static-web-hosting](/images/5-Workshop/5.3-Frontend-deployment/5.3.2-enable-static-web-hosting/find-static-web-hosting.png)

### 3. Configure Website Hosting

* By default, the interface displays **Disable**.

![edit](/images/5-Workshop/5.3-Frontend-deployment/5.3.2-enable-static-web-hosting/edit.png)

* In **Edit static website hosting**:

  * **Static website hosting**: Select ***Enable***.
  * **Hosting type**: Select ***Host a static website***.
  * **Index document**: Enter `index.html`.
  * **Error document (optional)**: Enter `error.html`.

![edit-static-web-hosting](/images/5-Workshop/5.3-Frontend-deployment/5.3.2-enable-static-web-hosting/edit-static-web-hosting.png)

### 4. Save the Configuration

* Review the settings.
* Scroll to the bottom and select ***Save changes***.

![save](/images/5-Workshop/5.3-Frontend-deployment/5.3.2-enable-static-web-hosting/save.png)

### 5. Verify the Configuration

* Static Website Hosting has been successfully enabled. Check the following information:

  * ***S3 static website hosting***: Enabled
  * ***Hosting type***: Bucket hosting
  * ***Bucket website endpoint***: HTTP URL

![static-web-hosting](/images/5-Workshop/5.3-Frontend-deployment/5.3.2-enable-static-web-hosting/static-web-hosting.png)
