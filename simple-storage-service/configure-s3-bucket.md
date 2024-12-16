# S3 Bucket Configuration

## Table of Contents  
1. [Introduction to Amazon S3](#introduction-to-amazon-s3)  
2. [What You Need Before Starting](#what-you-need-before-starting)  
3. [Step 1: Access Amazon S3 Console](#step-1-access-amazon-s3-console)  
4. [Step 2: Create an S3 Bucket](#step-2-create-an-s3-bucket)  
5. [Step 3: Configure Bucket Settings](#step-3-configure-bucket-settings) 
    - [Types of Buckets](#bucket-types)
    - [Versioning](#versioning)
    - [Encryption](#encryption)
    - [Block Public Access](#block-public-access-settings)
    - [Object Ownership](#object-ownership)
6. [Step 4: Upload Files to Your Bucket](#step-4-upload-files-to-your-bucket)  
7. [Step 5: Manage Permissions](#step-5-manage-permissions)  
8. [Step 6: Enable Static Website Hosting (Optional)](#step-6-enable-static-website-hosting-optional)  
9. [Step 7: Use Lifecycle Policies for Storage Management](#step-7-use-lifecycle-policies-for-storage-management)  
10. [Step 8: Replication in S3](#step-8-replication-in-amazon-s3)
11. [Conclusion](#conclusion)  

---

## Introduction to Amazon S3  

**Amazon Simple Storage Service (S3)** is a scalable, highly available, and secure object storage service provided by AWS. It is used to store and retrieve any amount of data, such as documents, images, videos, and backups.

In this guide, you will learn how to:  
- Create an S3 bucket.  
- Configure basic settings.  
- Upload and manage files.  
- Control access to your bucket.  
- Optionally, use the bucket for hosting a static website.

---

## What You Need Before Starting  

Before configuring an S3 bucket, ensure you have:  
1. An **AWS account**: Sign up at [AWS Console](https://aws.amazon.com/).  
2. **IAM User Credentials**: Use credentials with proper permissions to access and manage S3.  

---

## Step 1: Access Amazon S3 Console  

1. Log in to your **AWS Management Console**.  
2. In the search bar at the top, type `S3`.  
3. Click on **S3** under the "Services" menu.  
   - This will take you to the **Amazon S3 Dashboard**.  

---

## Step 2: Create an S3 Bucket  

1. On the **S3 Dashboard**, click on the **"Create bucket"** button.  
2. Provide the following details:  
   - **Bucket Name**:  
     - Must be globally unique.  
     - Can only contain lowercase letters, numbers, hyphens, and no spaces.  
     - Example: `my-first-s3-bucket-123`.  
   - **Region**:  
     - Select the AWS region closest to your users to reduce latency.  
     - Example: `US East (N. Virginia)`.

3. Click **Next** to configure additional settings.

---

## Step 3: Configure Bucket Settings  

### Bucket Types  

| **Feature**                          | **General Purpose Bucket**                           | **Directory Bucket**                                 |
|--------------------------------------|----------------------------------------------------|----------------------------------------------------|
| **Use Case**                         | Recommended for the majority of use cases.          | For performance-critical workloads requiring **single-digit millisecond latency**. |
| **Storage Classes Supported**        | Supports most S3 storage classes (e.g., Standard, IA, Glacier). | Supports only the **S3 Express One Zone** storage class. |
| **Data Organization**                | Flat storage structure with optional folder concepts via shared prefixes. | Hierarchical storage structure with directories.   |
| **Performance**                      | Optimized for general use cases with standard latency. | Optimized for low-latency and high TPS (hundreds of thousands of transactions per second). |
| **Availability**                     | Data is stored across **three or more Availability Zones (AZs)** for redundancy. | Exists in **a single Availability Zone (AZ)** only. |
| **Resilience**                       | Highly resilient with multi-AZ redundancy.          | Less resilient due to single-AZ storage.           |
| **Bucket Name**                      | Must be **globally unique**.                        | Must be **unique within the chosen AZ**; AZ ID is automatically included in the bucket name's suffix. |
| **Region Selection**                 | Stored in a specified **AWS Region** across multiple AZs. | Created in a specific **AZ within a Region**.      |
| **Recommended Setup**                | Works well with general workloads.                  | Best performance when co-located with compute resources in the same AZ. |
| **Supported S3 Features**            | Supports all Amazon S3 features.                    | Supports a **limited set** of Amazon S3 features.  |

---

#### Key Summary  
1. **General Purpose Buckets**:  
   - Best for typical use cases with standard storage classes and features.  
   - Provides high resilience with multi-AZ redundancy.  
   - Globally unique bucket names.  

2. **Directory Buckets**:  
   - Best for high-performance, low-latency applications requiring hierarchical directories.  
   - Limited to **S3 Express One Zone** storage class.  
   - Exists in a single AZ for optimized performance.  
   - Bucket names are unique **within the chosen AZ**.

---

### Versioning  
- **Purpose**: Keep multiple versions of an object.  
- Turn this **ON** if you want to track changes to files.  
- Example: If you upload `file1.txt` multiple times, previous versions will still be accessible.
- Versioning is a means of keeping multiple variants of an object in the same bucket. You can use versioning to preserve, retrieve, and restore every version of every object stored in your Amazon S3 bucket. With versioning, you can easily recover from both unintended user actions and application failures.

### Object Ownership
**Object Ownership** is a setting in Amazon S3 that determines who owns uploaded objects in a bucket and how access is managed. By default, the AWS account that uploads an object is its owner, even if the bucket belongs to a different account. Object ownership allows you to standardize object ownership and simplify permissions management.

There are two options for configuring **Object Ownership**:  

#### 1. **ACLs Disabled (Recommended)**  

- **What it does**:  
   - Disables the use of **Access Control Lists (ACLs)** for managing permissions on buckets and objects.  
   - All objects in the bucket become owned by the bucket owner, regardless of who uploads them.  

- **How it works**:  
   - Ownership and permissions are managed through **bucket policies** and **IAM policies** instead of ACLs.  
   - The bucket owner has full control over all objects in the bucket.  

- **Benefits**:  
   - Simplifies permissions management—no need to manage object-level ACLs.  
   - Prevents conflicts when objects are uploaded by different AWS accounts.  
   - Helps comply with AWS's best practice of using policies instead of ACLs.  

- **When to Use**:  
   - When you want to centralize ownership and simplify access control.  
   - For buckets that need consistent ownership and minimal complexity in permissions.

**Example**:  
If user A uploads `file1.txt` to a bucket owned by account B with **ACLs disabled**, account B (the bucket owner) automatically owns the uploaded file.  


#### 2. **ACLs Enabled**  

- **What it does**:  
   - Enables the use of **Access Control Lists (ACLs)** for managing permissions.  
   - Object ownership is determined by the AWS account that uploads the object unless explicitly overridden.  

- **How it works**:  
   - Permissions can be set at the bucket or object level using ACLs.  
   - Allows granting fine-grained permissions to specific AWS accounts or the public.  
   - Supports use cases where multiple AWS accounts need to upload and manage their own objects.  

- **Benefits**:  
   - Provides granular control for shared access scenarios.  
   - Useful when you need to grant object-level permissions to different AWS accounts or the public.  

- **When to Use**:  
   - When working with cross-account uploads where objects must retain ownership by the uploading account.  
   - If you need to use ACLs to control access to individual objects.  

**Example**:  
If user A uploads `file1.txt` to a bucket owned by account B with **ACLs enabled**, user A retains ownership of the object unless they explicitly grant ownership or permissions to account B.

---

#### Key Differences Between ACLs Disabled and Enabled  

| **Feature**                      | **ACLs Disabled**                          | **ACLs Enabled**                            |
|----------------------------------|--------------------------------------------|--------------------------------------------|
| **Object Ownership**             | Bucket owner owns all objects              | Uploader owns the objects                  |
| **Permission Management**        | Managed using policies (IAM, bucket)       | Managed using ACLs and policies            |
| **Simplified Management**        | Yes                                        | No—ACLs add additional complexity          |
| **Cross-Account Uploads**        | Objects default to bucket owner            | Uploader retains ownership                 |
| **Public Access**                | Not possible with ACLs                     | Possible by enabling public ACL settings   |


#### AWS Recommendation  

AWS recommends using **"ACLs Disabled"** as the best practice for most use cases. It simplifies ownership and permission management by relying solely on policies. Use **"ACLs Enabled"** only if your specific use case requires fine-grained object-level permissions or cross-account uploads.  

--- 

### Encryption  

**Default Encryption** ensures that all new objects uploaded to an S3 bucket are automatically encrypted on the server side. This is a critical step to secure data at rest, ensuring that unauthorized users cannot access or read the objects even if they gain access to the storage.

When configuring **Default Encryption**, you can choose among the following options:


#### 1. **Server-Side Encryption with Amazon S3 Managed Keys (SSE-S3)**  
- **What it does**: AWS automatically encrypts your data using **Amazon S3-managed encryption keys**.  
- **Management**: You don’t need to manage keys manually—AWS handles both the encryption and key management.  
- **Performance**: Transparent to the user with minimal overhead.   


#### 2. **Server-Side Encryption with AWS Key Management Service Keys (SSE-KMS)**  
- **What it does**: Encrypts objects using **AWS KMS-managed customer master keys (CMKs)**.  
- **Benefits**:  
   - Provides greater control over encryption keys.  
   - Allows auditing of encryption/decryption operations via AWS CloudTrail.  
- **Key Options**:  
   - Use AWS-managed KMS keys (default).  
   - Use your own custom KMS keys for more control.  


#### 3. **Dual-Layer Server-Side Encryption with AWS KMS Keys (DSSE-KMS)**  
- **What it does**: Applies **two layers of encryption** using AWS KMS-managed keys.  
- **Security**: Enhances protection by using two independent layers of encryption.  
- **Pricing**: This option incurs additional costs.  
   - Refer to the [Amazon S3 pricing page](https://aws.amazon.com/s3/pricing) for DSSE-KMS pricing.   


#### Bucket Key for SSE-KMS  

The **S3 Bucket Key** is a feature that reduces encryption costs when using **SSE-KMS**.  

- **How it works**: The S3 Bucket Key creates a unique encryption key for the bucket, which is reused for encrypting multiple objects within the bucket.  
- **Benefits**: Reduces the number of API calls to AWS KMS, thereby lowering costs.  

> **Note**: S3 Bucket Keys are **not supported** for DSSE-KMS.  

**Options**:  
- **Disable**: The Bucket Key is not used.  
- **Enable**: Lower your encryption costs by enabling the Bucket Key.  

---


### Block Public Access Settings  

The **Block Public Access** settings help secure your S3 buckets and objects by controlling public access through Access Control Lists (ACLs), bucket policies, and access point policies. AWS strongly recommends enabling **Block all public access** unless public access is explicitly required.


#### 1. **Block all public access**  
- This setting is a **master control**.  
- Enabling it turns on all four individual settings below.  
- It ensures that no public access is granted, regardless of ACLs or policies.  

**When to Use**:  
- You want to fully restrict public access for the bucket and its objects.  


#### 2. **Block public access to buckets and objects granted through new access control lists (ACLs)**  
- Prevents any **new ACLs** from granting public access to buckets or objects.  
- **Does not affect existing ACLs**—they remain unchanged.  

**Example**:  
- If someone uploads a new object and tries to set its ACL to public, this setting will block that action.  

**When to Use**:  
- You want to prevent accidental public access due to new ACL configurations.  


#### 3. **Block public access to buckets and objects granted through any access control lists (ACLs)**  
- This setting **completely ignores all ACLs** that grant public access.  
- Unlike the previous option, it applies to both **existing and new ACLs**.  

**Example**:  
- If an object already has an ACL with public access, enabling this option will block that access.  

**When to Use**:  
- You want to eliminate all public access controlled by ACLs.  


#### 4. **Block public access to buckets and objects granted through new public bucket or access point policies**  
- Blocks the creation of **new bucket policies** or **access point policies** that grant public access.  
- **Does not change existing policies**—they remain active.  

**Example**:  
- If someone tries to add a bucket policy that makes objects public, this setting blocks that action.  

**When to Use**:  
- You want to stop new bucket policies from inadvertently allowing public access.  


#### 5. **Block public and cross-account access to buckets and objects through any public bucket or access point policies**  
- Completely blocks all public and cross-account access for buckets and objects through policies.  
- Applies to both **existing and new policies**.  

**Example**:  
- If a bucket policy grants public access to all users (`"Principal": "*"`) or another AWS account, enabling this setting will block that access.  

**When to Use**:  
- You want to ensure no public or cross-account access is allowed through policies, even if previously configured.  

---

4. Click **Create bucket** to finalize the configuration.

---

## Step 4: Upload Files to Your Bucket  

1. On the **S3 Dashboard**, click on the newly created bucket name.  
2. Click the **"Upload"** button.  
3. Add files:  
   - Click **"Add files"** and select files from your computer.  
   - Example: `image1.jpg` and `document1.pdf`.  
4. **Set Permissions** (optional):  
   - By default, the uploaded files will be private.  
   - Modify permissions if public access is required.  
5. Click **Upload**.

**Practical Example**:  
- If you uploaded `image1.jpg`, you can access it via the console:  
  ```
  https://<your-bucket-name>.s3.<region>.amazonaws.com/image1.jpg
  ```

---

## Step 5: Manage Permissions  

### Default Permissions  
- By default, only the bucket owner can access and manage files.  

### Grant Public Access  
If you need to make specific files publicly accessible:  
1. Select the file you uploaded (e.g., `image1.jpg`).  
2. Click on **"Permissions"**.  
3. Under **Public access**, enable **"Read"** for everyone.  
4. Save changes.

**Practical Example**:  
- Once public, you can share the file’s link:  
  ```
  https://<your-bucket-name>.s3.<region>.amazonaws.com/image1.jpg
  ```

> **Important**: Be careful when granting public access. Only do this for non-sensitive content.

---

## Step 6: Enable Static Website Hosting (Optional)  

Amazon S3 can host static websites, such as HTML files.

### Steps to Enable Hosting:  
1. Go to the bucket **"Properties"** tab.  
2. Scroll down to **"Static website hosting"**.  
3. Select **"Enable"** and provide:  
   - **Index document**: `index.html`  
   - **Error document** (optional): `error.html`.  
4. Save changes.

### Upload Website Files:  
- Upload files such as `index.html` and `error.html` into the bucket.  
- Make these files **public** (see Step 5).  

### Access Your Website:  
- Use the endpoint provided in the static website hosting settings:  
  ```
  http://<your-bucket-name>.s3-website.<region>.amazonaws.com
  ```

---

## Step 7: Use Lifecycle Policies for Storage Management  

**Lifecycle policies** automatically move files to different S3 storage classes (e.g., Glacier) to save costs.  

### Example Policy:  
- **Day 0**: Upload file to S3 Standard.  
- **Day 30**: Move file to S3 Standard-IA (Infrequent Access).  
- **Day 90**: Move file to Glacier for archival.  

### Steps to Configure:  
1. Go to the bucket **"Management"** tab.  
2. Click **"Create lifecycle rule"**.  
3. Define the rule name and transition policy.  
4. Save the lifecycle rule.

---

## Step 8: Replication in Amazon S3  

**Replication** in Amazon S3 is a feature that enables automatic copying of objects from one bucket to another. This can be done **across AWS regions** (CRR) or **within the same region** (SRR). Replication helps meet requirements for compliance, data backup, and disaster recovery.

---

### 1. Cross-Region Replication (CRR)  

**What is CRR?**  
- Cross-Region Replication copies objects from a source bucket in one AWS region to a destination bucket in another region.  
- It provides redundancy across geographically distant AWS regions.  

**Key Benefits of CRR**:  
- **Disaster Recovery**: Protects data from regional failures.  
- **Compliance**: Helps meet regulatory requirements for storing copies of data in different regions.  
- **Latency Reduction**: Enables access to data from a region closer to end users.  

**Example**:  
If your source bucket is in **`us-east-1`** (North Virginia), CRR can replicate objects to a bucket in **`us-west-2`** (Oregon).  

---

### 2. Same-Region Replication (SRR)  

**What is SRR?**  
- Same-Region Replication copies objects between buckets in the same AWS region.  

**Key Benefits of SRR**:  
- **Compliance**: Helps store multiple copies of data for data sovereignty requirements.  
- **Backup**: Keeps a synchronized backup in the same region.  
- **Log Aggregation**: Replicates logs to a centralized bucket for easier access.  

**Example**:  
If your source bucket is in **`us-east-1`**, SRR can replicate objects to another bucket also located in **`us-east-1`**.

---

### Prerequisites for Replication  

Before configuring replication, ensure the following:  
1. **Enable Versioning**:  
   - Both the **source bucket** and **destination bucket** must have **Versioning** enabled.  
2. **IAM Permissions**:  
   - Create an IAM role with permissions to replicate objects from the source bucket to the destination bucket.  
3. **Bucket Ownership**:  
   - The bucket owner must have permission to replicate data.  

---

### How to Configure and Enable Replication in S3  

### Step 1: Enable Versioning on Both Buckets  
1. Go to the **S3 Console**.  
2. Select the **source bucket**, navigate to the **Properties** tab, and enable **Versioning**.  
3. Repeat the process for the **destination bucket**.  

---

### Step 2: Configure Replication Rule  
1. In the source bucket:  
   - Go to the **Management** tab.  
   - Under **Replication rules**, click **Create replication rule**.  

2. **Add Rule Details**:  
   - **Rule Name**: Give a descriptive name (e.g., `CRR-Rule-01`).  
   - **Status**: Keep the rule enabled.  

3. **Choose Destination Bucket**:  
   - **Cross-Region Replication (CRR)**: Select a bucket in a different region.  
   - **Same-Region Replication (SRR)**: Select a bucket in the same region.  
   - For cross-account replication, specify the bucket's **AWS account ID**.

4. **IAM Role**:  
   - Create or choose an **IAM role** that grants S3 permissions to replicate objects.  
   - AWS can automatically create the required role for you.  

---

### Step 3: Select Replication Options  
1. **Choose Objects to Replicate**:  
   - Replicate all objects or specify a subset of objects (e.g., using a prefix or tags).  

2. **Additional Options**:  
   - **Replicate Delete Markers**: Choose whether to replicate delete markers to the destination bucket.  
   - **Replication Time Control (RTC)**: Enable RTC if you need predictable replication times (additional cost).  

3. Click **Save** to create the replication rule.

---

### Step 4: Verify Replication  
1. Upload a new object to the **source bucket**.  
2. Check the **destination bucket** after a few minutes to confirm the object has been replicated.  
3. If you enabled versioning, ensure that object versions match in both buckets.

---

## Summary of Replication Features  

| Feature                        | **Cross-Region Replication (CRR)**      | **Same-Region Replication (SRR)**   |
|--------------------------------|----------------------------------------|------------------------------------|
| **Regions**                    | Between different AWS regions          | Within the same AWS region         |
| **Use Case**                   | Disaster recovery, compliance, latency | Compliance, backup, log aggregation|
| **Latency**                    | Slightly higher due to regional transfer| Lower, as it remains in the region |
| **Cost**                       | Data transfer costs apply              | No inter-region data transfer costs|
| **Object Ownership**           | Bucket owner can override ownership    | Bucket owner can override ownership|

---

## Notes and Best Practices  

1. **Enable Versioning**: Replication won’t work without enabling versioning on both source and destination buckets.  
2. **IAM Role Management**: Let AWS automatically create the replication IAM role unless you need custom permissions.  
3. **Replication Time Control (RTC)**: Use RTC if predictable replication times (e.g., 15 minutes or less) are needed.  
4. **Monitor Replication**: Use **Amazon S3 Metrics** or **CloudWatch** to monitor replication status and performance.  
5. **Costs**: Replication incurs costs for:  
   - Storage in the destination bucket.  
   - Data transfer (for CRR).  

By understanding CRR and SRR, you can choose the appropriate replication strategy based on your data availability, compliance, and latency requirements.

--- 

## Conclusion  

By following this guide, you can:  
- Create an S3 bucket.  
- Upload and manage files.  
- Control access with permissions.  
- Enable static website hosting.  
- Optimize storage costs using lifecycle policies.

**Next Steps**:  
Explore advanced features like S3 replication, data encryption, and monitoring with AWS CloudWatch.  

---

