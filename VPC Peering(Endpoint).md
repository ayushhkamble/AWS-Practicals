# 🌐 AWS VPC Endpoint Practical 

## 🔐 Private Connectivity Between Resources Using VPC Endpoints

---

## 📌 Project Description

This practical demonstrates how to use **VPC Endpoints** to enable **secure, private communication within AWS** without requiring an Internet Gateway, NAT Gateway, or public IP addresses.

In this setup:

* Two VPCs are created in **different regions**
* Each VPC contains **only a Private Subnet**
* A **Private EC2 instance** is launched in each VPC
* A **VPC Endpoint** is configured to securely access AWS services (like S3) without internet access

This setup highlights how AWS ensures **secure, internal communication using private networking**.

---

## 🎯 Objectives

* Create **VPCs in two different regions**
* Launch **Private EC2 instances** (no public IP)
* Configure **VPC Endpoints (Gateway Endpoint for S3)**
* Enable **private access to AWS services**
* Validate connectivity without internet usage

---

## 🏗️ Architecture Overview

### 🌍 Region 1 (Europe)

* VPC CIDR: `10.0.0.0/16`
* Private Subnet: `10.0.1.0/24`
* Private EC2 Instance
* VPC Endpoint (S3)

---

### 🌏 Region 2 (Hyderabad)

* VPC CIDR: `192.168.0.0/16`
* Private Subnet: `192.168.1.0/24`
* Private EC2 Instance
* VPC Endpoint (S3)

---

### 🔒 Key Concept

* No Internet Gateway
* No Public Subnet
* No NAT Gateway
* All communication happens via **AWS Private Network**

---

## 🪜 Step-by-Step Implementation

---

### 1️⃣ Create VPC in Europe Region

* Go to **VPC Dashboard**
* Create VPC:

  * Name: `Europe-VPC`
  * CIDR: `10.0.0.0/16`

Create Private Subnet:

* `10.0.1.0/24`

---

### 2️⃣ Create VPC in Hyderabad Region

* Create VPC:

  * Name: `Hyderabad-VPC`
  * CIDR: `192.168.0.0/16`

Create Private Subnet:

* `192.168.1.0/24`

---

### 3️⃣ Launch Private EC2 Instances

In both regions:

* Launch EC2 Instance
* Select respective VPC and Private Subnet
* Disable Auto-assign Public IP

---

### 4️⃣ Configure Security Groups

Allow internal communication:

```bash id="z7fxrb"
Type: All Traffic
Protocol: All
Port: All
Source: VPC CIDR
```

Example:

* Europe SG → Source: `10.0.0.0/16`
* Hyderabad SG → Source: `192.168.0.0/16`

---

### 5️⃣ Create VPC Endpoint (S3)

#### In Europe Region

* Go to **VPC → Endpoints**
* Click **Create Endpoint**

Configuration:

* Service Category: AWS Services
* Service Name: `com.amazonaws.<region>.s3`
* VPC: Europe-VPC
* Route Table: Select Private Route Table

---

#### In Hyderabad Region

Repeat the same steps:

* Select respective region S3 service
* Attach to Hyderabad VPC route table

---

### 6️⃣ Verify Route Table Entries

After creating endpoint, route table will automatically include:

```bash id="ckf6f0"
Destination: pl-xxxx (S3 Prefix List)
Target: VPC Endpoint
```

---

### 7️⃣ Connect to Private Instance

Since instances are private, use:

* EC2 Instance Connect (if enabled)
  OR
* Temporary Bastion (optional, for testing)

---

### 8️⃣ Test S3 Connectivity (Without Internet)

Install AWS CLI (if not installed):

```bash id="9rxn5q"
sudo apt update
sudo apt install awscli -y
```

---

### 9️⃣ Configure AWS CLI

```bash id="y22tcd"
aws configure
```

Enter:

* Access Key
* Secret Key
* Region

---

### 🔟 Test Access to S3

```bash id="5ckrb6"
aws s3 ls
```

or

```bash id="w41y41"
aws s3 ls s3://<your-bucket-name>
```

✅ If successful:

* Traffic is going via **VPC Endpoint**
* No internet is used

---

## 🔍 Validation Checklist

* ✔️ No Internet Gateway attached
* ✔️ No Public IP assigned
* ✔️ VPC Endpoint created successfully
* ✔️ Route tables updated automatically
* ✔️ EC2 instance can access S3
* ✔️ AWS CLI commands working

---

## ⚠️ Important Notes

* VPC Endpoints are **region-specific**
* They do **not connect two VPCs directly**
* Used for accessing **AWS services privately**
* Two types:

  * Gateway Endpoint (S3, DynamoDB)
  * Interface Endpoint (PrivateLink)

---

## 🚀 Conclusion

In this practical, we successfully configured **VPC Endpoints** in two separate regions to allow **private access to AWS services without internet connectivity**.

We learned:

* How to build a **fully private AWS architecture**
* How AWS services can be accessed securely using **VPC Endpoints**
* Importance of **route tables and private networking**

This approach is widely used in:

* Secure enterprise environments
* Financial systems
* Data-sensitive applications

---

## 📌 GitHub Commit Message

```bash id="as8y7v"
Added AWS VPC Endpoint practical demonstrating private subnet architecture in multiple regions with secure S3 access without internet gateway or NAT.
```

---

Just tell me 👍
