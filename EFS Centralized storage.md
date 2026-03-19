# 🚀 AWS EFS Practical

### 📂 Mounting Amazon EFS on Two EC2 Instances (Multi-AZ Setup)

---

## 🌟 Project Overview

This practical demonstrates how to configure **Amazon Elastic File System (EFS)** and mount it on two EC2 instances (**WebServer1** & **WebServer2**) deployed across different Availability Zones within the same VPC. 
Amazon EFS is a **fully managed, elastic, and scalable network file system** that allows multiple EC2 instances to access a shared file storage simultaneously. Unlike traditional storage systems, EFS automatically scales 
storage capacity up or down based on usage, eliminating the need for manual provisioning.

> 🎯 Goal: Achieve **shared, scalable storage** accessible from multiple EC2 instances while ensuring high availability and fault tolerance.

---

## 🏗️ Architecture Diagram (Conceptual)

```
          +---------------------+
          |     Amazon EFS      |
          |  (Shared Storage)   |
          +----------+----------+
                     |
        ---------------------------------
        |                               |
+---------------+               +---------------+
|  WebServer1   |               |  WebServer2   |
|   (AZ-1)      |               |   (AZ-2)      |
| Mounted /efs  |               | Mounted /efs  |
+---------------+               +---------------+
```

In this architecture, EFS acts as a **centralized storage layer** that is accessible over the **NFS (Network File System) protocol**.
Each EC2 instance connects to EFS using **mount targets**, which are created in each Availability Zone. This ensures **low latency access** and **high availability**, as data remains accessible even if one AZ fails. 
This design follows a distributed system approach used in scalable cloud applications.

---

## 🧰 Tech Stack

* ☁️ AWS EC2 – Provides scalable virtual servers for hosting applications
* 📁 AWS EFS – Managed shared file storage service
* 🔐 Security Groups (NFS - 2049) – Acts as a virtual firewall controlling traffic
* 🐧 Linux (Amazon Linux & Ubuntu) – Operating systems for server configuration
* 💻 CLI (SSH, NFS Client) – Used for remote management and mounting operations

The combination of compute (EC2) and storage (EFS) demonstrates the concept of **decoupling resources**, where storage and processing are managed independently for better scalability and flexibility.

---

## ⚙️ Step-by-Step Implementation

---

### 🔹 1. Launch EC2 Instances

| Instance Name | Availability Zone | OS                    |
| ------------- | ----------------- | --------------------- |
| WebServer1    | AZ-1              | Amazon Linux / Ubuntu |
| WebServer2    | AZ-2              | Amazon Linux / Ubuntu |

✔ Same VPC
✔ Same Security Group

Placing instances in different Availability Zones ensures **fault tolerance and high availability**.
If one AZ experiences failure, the other instance continues to function.

---

### 🔹 2. Configure Security Group

| Rule Type | Protocol | Port | Source   |
| --------- | -------- | ---- | -------- |
| NFS       | TCP      | 2049 | VPC CIDR |

> 🔒 This allows EFS communication between instances

EFS uses the **NFS protocol**, which operates over port 2049.
Security Groups act as stateful firewalls, allowing secure and controlled communication between EC2 and EFS.

---

### 🔹 3. Create EFS File System

* Select same VPC
* Enable **Mount Targets in both AZs**
* Attach same Security Group

Mount targets act as **network endpoints** for accessing EFS within each AZ.
Creating them in multiple AZs ensures redundancy, low latency, and high availability, while EFS internally manages data replication.

---

### 🔹 4. Connect to EC2 Instances

```bash
# Amazon Linux
ssh -i key.pem ec2-user@<IP>

# Ubuntu
ssh -i key.pem ubuntu@<IP>
```

SSH (Secure Shell) provides **encrypted remote access** to instances,
ensuring secure communication for administrative operations.

---

## 🧪 Installation & Setup

---

### 🟡 Amazon Linux Setup

```bash
sudo yum update -y
sudo yum install -y amazon-efs-utils
sudo mkdir /efs
```

The `amazon-efs-utils` package simplifies mounting and provides enhanced features such as encryption and improved performance handling.

---

### 🔵 Ubuntu Setup

```bash
sudo apt update
sudo apt install -y nfs-common
sudo mkdir /efs
```

Ubuntu uses the standard NFS client, which enables communication with EFS using the NFS protocol.

---

## 🔗 Mounting EFS

---

### ✅ Method 1: Using EFS Utils (Recommended)

```bash
sudo mount -t efs <EFS-ID>:/ /efs
```

This method provides optimized and secure mounting with better integration in AWS environments.

---

### ✅ Method 2: Using NFS Client

```bash
sudo mount -t nfs4 -o nfsvers=4.1 <EFS-DNS>:/ /efs
```

📌 Example:

```bash
sudo mount -t nfs4 -o nfsvers=4.1 fs-12345678.efs.ap-south-1.amazonaws.com:/ /efs
```

NFSv4.1 offers improved performance, reliability, and file locking capabilities suitable for distributed systems.

---

## 🔍 Verification

```bash
df -h
```

✔ Look for `/efs` mount

The command displays all mounted file systems and confirms successful attachment of EFS.

---

## 🧪 Testing Shared Storage

### 🖥️ On WebServer1:

```bash
echo "Hello from WebServer1" | sudo tee /efs/test.txt
```

### 🖥️ On WebServer2:

```bash
cat /efs/test.txt
```

✅ Output confirms shared file system working

This demonstrates that both instances can read and write to the same storage in real time.

---

## 🔁 Auto-Mount on Boot

Edit fstab:

```bash
sudo nano /etc/fstab
```

### Amazon Linux:

```bash
<EFS-ID>:/ /efs efs defaults,_netdev 0 0
```

### Ubuntu:

```bash
<EFS-DNS>:/ /efs nfs4 defaults,_netdev 0 0
```

The `/etc/fstab` file ensures the file system is automatically mounted after reboot, 
and `_netdev` delays mounting until the network is available.

---

## 🚨 Troubleshooting Guide

| Issue               | Solution                         |
| ------------------- | -------------------------------- |
| ❌ Mount fails       | Check Security Group (Port 2049) |
| ❌ DNS error         | Verify EFS DNS name              |
| ❌ Timeout           | Ensure mount targets in both AZs |
| ❌ Permission denied | `sudo chmod -R 777 /efs`         |

Most issues are related to networking, DNS configuration, or permissions.

---

## 🎯 Key Learnings

✨ Multi-AZ shared storage setup
✨ NFS-based mounting in Linux
✨ AWS networking & security configuration
✨ Real-time file sharing between servers

This practical highlights concepts like high availability, scalability, and distributed storage in cloud environments.

---

## 📸 Suggested Screenshots (For GitHub)

* EC2 Instances Dashboard
* Security Group Rules
* EFS Configuration
* Terminal Mount Output (`df -h`)
* File Sharing Test

---

## 💡 Use Cases

* 🌐 Scalable Web Applications
* 📊 Data Analytics Pipelines
* 🐳 Container Storage (Docker/Kubernetes)
* 🗂️ Shared Media Storage

EFS is suitable for applications requiring concurrent access, dynamic scaling, and persistent shared storage.

---

## ⭐ Final Result

✅ Successfully mounted **Amazon EFS**
✅ Connected across **multiple AZs**
✅ Enabled **real-time shared storage**

---

## 🔥 Pro Tip

> Always use **EFS Utils** for better performance, encryption, and reliability.
