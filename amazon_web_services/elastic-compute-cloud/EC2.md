
# Amazon EC2 (Elastic Compute Cloud)

## Table of Contents
1. [Amazon EC2](#what-is-ec2)
2. [Amazon Machine Image](#what-is-an-ami)
3. [Instance Families in EC2](#what-are-instance-families)
4. [VPCs in EC2](#why-do-we-attach-a-vpc-when-creating-an-ec2-instance)
5. [Security Groups in EC2](#why-do-we-select-or-create-a-security-group-when-creating-an-instance)
6. [Keypair in EC2](#why-do-we-need-a-key-pair)
7. [Storage Options in EC2](#how-to-attach-storage-to-ec2-instances)
8. [User Scripting Options in EC2](#how-to-run-user-scripts-in-ec2-instances)

## What is EC2?
AWS EC2 (Elastic Compute Cloud) is a service provided by Amazon Web Services (AWS) that allows you to rent virtual servers, known as EC2 instances, to run your applications. You can think of it as renting a computer in the cloud that you can configure to meet your specific needs.

With EC2, you can choose the operating system (Linux, Windows, etc.), hardware configuration (amount of CPU, memory, storage), and network settings. It is highly scalable, meaning you can increase or decrease the number of instances depending on your application's demand. You only pay for the resources you use, which makes it cost-effective.

### **Key Features:**
- **On-Demand Availability:** Launch instances when needed without long-term commitments.
- **Scalability:** Easily increase or decrease capacity based on demand.
- **Flexibility:** Wide range of instance types to suit your application.
- **Security:** Offers robust security features such as encryption and firewall rules.
- **Cost-Effectiveness:** Pay only for what you use, with options like spot instances for even lower costs.

---

## What is an AMI?
An **Amazon Machine Image (AMI)** is a template used to create EC2 instances. It contains the operating system, application software, and configurations needed to launch an instance.

### Key Features of AMI:
- **Customization:** Allows creating personalized configurations.
- **Reusability:** Launch multiple instances using the same AMI.
- **Version Control:** Save and reuse specific setups for consistency.

### Example Use Case:
1. Choose an AMI with Linux and WordPress pre-installed to launch a blogging website.
2. Save a customized AMI after adding custom themes and plugins for future replication.

### Types of AMIs:
- **AWS-Provided:** Basic templates like Linux, Windows.
- **Marketplace AMIs:** Pre-configured by third-party vendors.
- **Custom AMIs:** Created with your specific configurations.

By choosing different AMIs, you can create different "flavors" of servers. For example:

- A web server recipe (with Apache or Nginx pre-installed).
- A database server recipe (with MySQL or PostgreSQL set up).
- A blank server recipe (with just the operating system).

---

## **What is an Instance in AWS?**
An **instance** in AWS is a virtual server that you run using the Amazon Elastic Compute Cloud (EC2) service. It is the actual working server created when you launch an Amazon Machine Image (AMI). Instances are used to host applications, databases, websites, or any other workloads you need in the cloud.

When you create an EC2 instance, you select its configuration, including the hardware (CPU, memory, storage, and networking) and its software (operating system, applications).

---

## What Are Instance Families?
AWS EC2 instances are grouped into **instance families** based on their hardware capabilities and use cases. Each family is optimized for a specific type of workload, such as general-purpose computing, memory-intensive applications, or compute-heavy tasks.

### **Instance Family Types**

1. **General Purpose Instances:**
   - Balanced performance for a wide variety of workloads.
   - Ideal for web servers, app servers, small/medium databases, and development environments.
   - **Examples:**
     - **t-series:** Cost-effective for low to moderate workloads (e.g., t2.micro, t3.medium).
     - **m-series:** Balanced CPU, memory, and storage (e.g., m5.large, m6i.xlarge).

2. **Compute Optimized Instances:**
   - Optimized for compute-intensive applications like batch processing, high-performance computing, and gaming servers.
   - **Examples:**
     - **c-series:** High CPU-to-memory ratio (e.g., c5.large, c6i.xlarge).

3. **Memory Optimized Instances:**
   - Designed for applications that process large datasets in memory.
   - Ideal for high-performance databases, in-memory caching, and real-time big data analytics.
   - **Examples:**
     - **r-series:** Optimized for memory (e.g., r5.large, r6i.xlarge).
     - **x-series:** High memory-to-CPU ratio (e.g., x1.32xlarge).
     - **z-series:** High memory and compute performance (e.g., z1d.large).

4. **Storage Optimized Instances:**
   - Optimized for high, sequential read/write access to large datasets.
   - Used for data warehousing, Hadoop, and NoSQL databases.
   - **Examples:**
     - **i-series:** High-speed SSD storage for transactional workloads (e.g., i3.large, i4i.xlarge).
     - **d-series:** Dense storage for large datasets (e.g., d2.xlarge).

5. **Accelerated Computing Instances:**
   - Use hardware accelerators (GPUs or FPGAs) to boost performance for tasks like machine learning, graphics rendering, and scientific modeling.
   - **Examples:**
     - **p-series:** GPU instances for ML and AI workloads (e.g., p3.2xlarge).
     - **g-series:** GPU instances for graphics applications (e.g., g4dn.xlarge).
     - **f-series:** FPGA instances for custom hardware acceleration (e.g., f1.2xlarge).

6. **Networking Optimized Instances:**
   - Designed for workloads requiring high-speed networking or low latency.
   - **Examples:**
     - **h-series:** High disk throughput and storage (e.g., h1.2xlarge).
     - **u-series:** Ultra-high memory (e.g., u-12tb1.metal).

---

### **Key Differences Between Instance Families**

| **Feature**               | **General Purpose**     | **Compute Optimized**     | **Memory Optimized**       | **Storage Optimized**      | **Accelerated Computing** |
|---------------------------|-------------------------|---------------------------|----------------------------|----------------------------|---------------------------|
| **CPU vs. Memory Ratio**   | Balanced               | High CPU-to-memory ratio  | High memory-to-CPU ratio   | Balanced with storage focus| Varies (based on GPU/FPGA)|
| **Use Case**               | Web servers, databases | Batch processing, gaming  | In-memory analytics        | Data warehousing, big data | AI, ML, Graphics          |
| **Examples**               | t3, m6i                | c5, c6i                   | r5, x1                     | i3, d2                     | p3, g4dn                  |

---

### **Choosing the Right Instance Type**
- **General Purpose:** Use if you're starting out and unsure about specific needs. Perfect for balanced workloads like web hosting.
- **Compute Optimized:** Choose for tasks that demand high CPU power, like scientific simulations or game servers.
- **Memory Optimized:** Use for applications requiring large memory allocations, like in-memory databases or big data analytics.
- **Storage Optimized:** Opt for heavy disk-based workloads, like data lakes or large transactional databases.
- **Accelerated Computing:** Ideal for GPU-intensive applications, such as video processing, AI model training, or 3D rendering.

AWS offers a **free tier** with **t2.micro** or **t3.micro**, a great option to experiment and understand the capabilities of EC2 before scaling up to other instance types.

---

## **Why Do We Attach a VPC When Creating an EC2 Instance?**

A **VPC (Virtual Private Cloud)** is a logically isolated network in AWS where you can launch your EC2 instances and other resources. Attaching an EC2 instance to a VPC is essential because it provides a **secure and customizable networking environment**.

### **Uses and Importance of VPC:**
1. **Isolation and Security:**  
   - A VPC isolates your EC2 instance from other AWS customers. Your resources are confined to a private, secure network that you control.

2. **Subnet Management:**  
   - You can create **subnets** (smaller network segments within a VPC) to organize your resources. For example, place public-facing resources like web servers in public subnets and private resources like databases in private subnets.

3. **IP Address Control:**  
   - You define the IP address range for your VPC using a CIDR block (e.g., 192.168.0.0/16). This gives you control over IP addressing for your resources.

4. **Customizable Routing and Connectivity:**  
   - You can control how your EC2 instances communicate with the internet, other AWS services, or on-premises systems using **route tables** and **VPNs**.

5. **Firewall Control:**  
   - VPCs work alongside **security groups** and **network ACLs** to control inbound and outbound traffic to your EC2 instances.

6. **Internet or Private Access:**  
   - Attach an **Internet Gateway** to the VPC for public internet access or a **NAT Gateway** for private access.

### **What Does VPC Do for an EC2 Instance?**
- It **defines the network environment** for the instance (IP range, subnets, routing).
- Ensures the instance has connectivity as per your requirements:
  - Public-facing for web servers.
  - Private for databases and internal apps.
- Protects the instance using firewalls and access control.

---

## **Why Do We Select or Create a Security Group When Creating an Instance?**

A **security group** is like a **virtual firewall** for your EC2 instance. It controls the **inbound and outbound traffic** to and from the instance. AWS requires you to attach at least one security group when creating an instance to ensure a base level of access control.

### **Uses and Importance of a Security Group:**
1. **Access Control:**  
   - Security groups allow you to define **rules** for which IP addresses or ranges can access your EC2 instance. For example:
     - Allow HTTP traffic (port 80) for web servers.
     - Allow SSH traffic (port 22) from your IP address for remote access.

2. **Stateful Behavior:**  
   - Security groups are **stateful**, meaning if you allow inbound traffic (e.g., SSH), the corresponding outbound response is automatically allowed.

3. **Enhanced Security:**  
   - They act as a **layer of protection**, ensuring only specific types of traffic can reach the instance.

4. **Ease of Management:**  
   - Security groups can be attached to multiple instances. Changing the rules of a security group applies to all instances using it.

5. **Dynamic Updates:**  
   - You can modify the rules of a security group at any time without rebooting the instance.

### **What Does a Security Group Do for an EC2 Instance?**
- **Allows or Denies Traffic:**  
   - Controls what traffic can enter or leave the instance based on rules for specific protocols, ports, and IP ranges.

- **Example:**  
   - For a web server:
     - Inbound rule: Allow HTTP (port 80) from anywhere.
     - Outbound rule: Allow all traffic (default).

- **Protects Against Unauthorized Access:**  
   - Prevents unauthorized IPs from accessing sensitive services, such as database ports (e.g., 3306 for MySQL).

---

### **Summary of Why Both Are Needed**
- **VPC:** Provides the networking environment where your EC2 instance operates, defining connectivity, IP ranges, and isolation.
- **Security Group:** Defines the rules for traffic entering and leaving the EC2 instance, acting as a virtual firewall for security.  

Together, they ensure **safe and organized deployment** of your EC2 instance in a secure and efficient network.

---

## Why Do We Need a Key Pair?
A **key pair** is used for secure SSH-based access to your EC2 instance.

### Importance of a Key Pair:
1. **Secure Authentication:** Prevents unauthorized access with key-based login.
2. **Encryption:** Ensures encrypted communication during login.
3. **Access Control:** Without the private key, access is impossible.

### How It Works:
1. AWS generates a public-private key pair.
2. The public key is stored in the instance, and the private key is downloaded as a `.pem` file.
3. Use the private key to SSH into the instance.

### Example Command:
```bash
ssh -i "your-key.pem" ec2-user@<your-instance-ip>
```

---

## **How to Attach Storage to EC2 Instances**

Amazon EC2 instances require storage for operating system files, applications, and data. AWS provides **Elastic Block Store (EBS)** and **Instance Store** as the primary storage options for EC2 instances. Here's an explanation of how to attach storage to EC2 instances:

---

### **1. Types of Storage for EC2:**

#### **Elastic Block Store (EBS):**
- **What It Is:** A scalable, persistent block storage service designed for EC2.
- **Use Cases:** Suitable for general-purpose workloads, databases, file storage, etc.
- **Features:**
  - Persistent: Retains data even after the instance stops.
  - Flexible: Allows resizing and snapshots.
  - Types:
    - General Purpose SSD (gp3, gp2)
    - Provisioned IOPS SSD (io2, io1)
    - Throughput Optimized HDD (st1)
    - Cold HDD (sc1)

#### **Instance Store:**
- **What It Is:** Temporary block storage physically attached to the host machine.
- **Use Cases:** Ideal for temporary storage needs like caches or scratch data.
- **Features:**
  - Non-persistent: Data is lost when the instance stops.
  - High performance for ephemeral workloads.

#### **Amazon S3 (Complementary):**
While not directly attached to EC2, S3 is often used for storing large amounts of data accessible over the network.

---

### **2. Steps to Attach an EBS Volume to an EC2 Instance:**

#### **Step 1: Create an EBS Volume**
1. Open the **AWS Management Console** and navigate to **EC2 > Elastic Block Store > Volumes**.
2. Click on **Create Volume**.
3. Select the size, type (e.g., gp3), and availability zone (must match the instance's zone).
4. Create the volume.

#### **Step 2: Attach the Volume to the Instance**
1. In the EBS console, select the volume.
2. Click **Actions > Attach Volume**.
3. Select the EC2 instance to attach the volume to.
4. Specify the device name (e.g., `/dev/xvdf`) and click **Attach**.

#### **Step 3: Connect and Mount the Volume**
1. SSH into your EC2 instance using the private key.
2. Check if the volume is recognized:
   ```bash
   lsblk
   ```
   The new volume should appear (e.g., `/dev/xvdf`).
3. Format the volume:
   ```bash
   sudo mkfs -t ext4 /dev/xvdf
   ```
4. Create a mount point:
   ```bash
   sudo mkdir /mnt/mydata
   ```
5. Mount the volume:
   ```bash
   sudo mount /dev/xvdf /mnt/mydata
   ```
6. To make the mount permanent, edit the `/etc/fstab` file:
   ```bash
   echo '/dev/xvdf /mnt/mydata ext4 defaults,nofail 0 2' | sudo tee -a /etc/fstab
   ```

---

### **3. Adding Storage During Instance Launch:**
- When creating an EC2 instance, you can add storage in the **Storage Options** section.
- Add multiple volumes or increase the root volume size if needed.

---

### **4. Expanding EBS Volume Size:**
You can expand the size of an attached EBS volume without downtime:
1. Go to **EC2 > Volumes**.
2. Select the volume, click **Modify Volume**, and increase the size.
3. Resize the filesystem on the instance:
   ```bash
   sudo resize2fs /dev/xvdf
   ```

---

### **5. Benefits of Using EBS with EC2:**
1. **Persistence:** Data is retained even if the instance is stopped or terminated (if not deleted with the instance).
2. **Snapshots:** Create backups and restore volumes easily.
3. **Scalability:** Modify size and type as needed.

---

## **How to Run User Scripts in EC2 Instances**

AWS EC2 allows you to run initialization scripts, known as **user data scripts**, automatically when an instance is launched. These scripts are typically used to install software, configure the environment, or perform other setup tasks.

---

### **What Are User Scripts?**
- **User scripts** are shell commands or scripts provided as part of the instance's user data.
- They execute automatically during the first boot of the EC2 instance.
- Common formats:
  - Shell scripts (`#!/bin/bash`)
  - Cloud-init directives (`#cloud-config`)

---

## Summary
Amazon EC2 is a powerful cloud service offering flexibility, scalability, and security for various workloads. Key components like AMIs, VPCs, security groups, and key pairs ensure a robust and secure setup tailored to user needs.
