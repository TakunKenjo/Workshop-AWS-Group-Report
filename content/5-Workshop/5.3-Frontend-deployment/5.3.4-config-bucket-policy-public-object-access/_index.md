---

title : "Configure Bucket Policy/Public Object Access"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.3.4. </b> "
---

### 1. Access Bucket Permissions

* In the S3 interface, select the ***Permissions*** tab.

![tab-permission](/images/5-Workshop/5.3-Frontend-deployment/5.3.4-config-bucket-policy-public-object-access/tab-permission.png)

### 2. Find Access Control List

* Scroll down and find ***Access Control List***:

  * Check that **Bucket owner enforced** is enabled.
  * This means that ACLs are currently disabled.
  * ACLs must be enabled first before using ACLs to make objects publicly accessible.

![find-ACL](/images/5-Workshop/5.3-Frontend-deployment/5.3.4-config-bucket-policy-public-object-access/find-ACL.png)

### 3. Enable ACLs

* Under **Object Ownership**, select ***Edit***.

![edit](/images/5-Workshop/5.3-Frontend-deployment/5.3.4-config-bucket-policy-public-object-access/edit.png)

* In the **Edit Object Ownership** interface:

  * **Object Ownership**: Select ***ACLs enabled***.
  * Check ***I acknowledge that ACLs will be restored***.
  * **Object Ownership**: Select ***Bucket owner preferred***.
  * Select ***Save changes***.

![edit-step](/images/5-Workshop/5.3-Frontend-deployment/5.3.4-config-bucket-policy-public-object-access/edit-step.png)

### 4. Verify the ACL Configuration

* Check the **Ownership** section. It should display **ACLs are enabled**.

![confirm](/images/5-Workshop/5.3-Frontend-deployment/5.3.4-config-bucket-policy-public-object-access/confirm.png)

### 5. Make Objects Public Using ACLs

* Go to the **Objects** tab of the bucket:

  * Select the ***objects*** you want to make publicly accessible.
  * Select ***Actions*** from the toolbar.
  * Select ***Make public using ACL***.

![public-ACL](/images/5-Workshop/5.3-Frontend-deployment/5.3.4-config-bucket-policy-public-object-access/public-ACL.png)

### 6. Confirm Public Access

* In the **Make public** interface:

  * Review the objects that will be made publicly accessible.
  * Select ***Make public*** to confirm.

![make-public](/images/5-Workshop/5.3-Frontend-deployment/5.3.4-config-bucket-policy-public-object-access/make-public.png)

### 7. Verify the Public Access Configuration

* A notification confirming that the objects were successfully made public will be displayed.

![make-public-success](/images/5-Workshop/5.3-Frontend-deployment/5.3.4-config-bucket-policy-public-object-access/make-public-success.png)
