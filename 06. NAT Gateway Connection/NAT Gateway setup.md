# 🚀 AWS Practical Guide: NAT Gateway Architecture with Public & Private Subnets

---

## 📌 Project Title

**Implementing NAT Gateway for Secure Internet Access in a Private Subnet (AWS VPC)**

---

## 🎯 Objective

To design and configure a secure AWS network where:

* A **Public EC2 Instance (Bastion Host)** has direct internet access
* A **Private EC2 Instance** does **NOT** have direct internet access
* The private instance accesses the internet **securely via NAT Gateway**
* Connectivity is verified using **SSH, SCP, and ping commands**

---

## 🧰 Services Used

* Amazon Web Services
* Amazon VPC
* Amazon EC2
* Internet Gateway
* NAT Gateway
* Elastic IP
* Route Tables & Security Groups

---

## 🏗 Architecture Overview

```
                🌐 Internet
                     │
             ┌───────────────┐
             │ Internet GW   │
             └──────┬────────┘
                    │
        ┌──────────────────────────┐
        │        Custom VPC        │
        │                          │
        │   🌍 Public Subnet       │
        │   ┌──────────────────┐   │
        │   │ Bastion Host     │◄── SSH
        │   │ (Public EC2)     │   │
        │   └────────┬─────────┘   │
        │            │             │
        │        NAT Gateway       │
        │            │             │
        │   🔒 Private Subnet      │
        │   ┌──────────────────┐   │
        │   │ Private EC2      │   │
        │   └──────────────────┘   │
        └──────────────────────────┘
```

---

## ⚙️ Step-by-Step Implementation

---

### 🔹 Step 1: Create a Custom VPC

* Go to VPC Dashboard → Create VPC
* Name: `My-VPC`
* CIDR Block: `10.0.0.0/16`

---

### 🔹 Step 2: Create Subnets

#### 🌍 Public Subnet

* Name: `Public-Subnet`
* CIDR: `10.0.1.0/24`
* Enable **Auto-assign Public IP**

#### 🔒 Private Subnet

* Name: `Private-Subnet`
* CIDR: `10.0.2.0/24`

---

### 🔹 Step 3: Create & Attach Internet Gateway

* Create Internet Gateway → `My-IGW`
* Attach it to your VPC

> 💡 Enables internet access for public subnet

---

### 🔹 Step 4: Create Route Tables

#### 🌍 Public Route Table

* Add Route:
  `0.0.0.0/0 → Internet Gateway`
* Associate with Public Subnet

#### 🔒 Private Route Table

* Keep empty for now

---

### 🔹 Step 5: Allocate Elastic IP

* Go to EC2 → Elastic IPs
* Click Allocate

---

### 🔹 Step 6: Create NAT Gateway

* Go to VPC → NAT Gateways
* Create in Public Subnet
* Attach Elastic IP

> 💡 NAT Gateway must always be in Public Subnet

---

### 🔹 Step 7: Update Private Route Table

* Add Route:
  `0.0.0.0/0 → NAT Gateway`
* Associate with Private Subnet

---

### 🔹 Step 8: Launch EC2 Instances

#### 🌍 Bastion Host (Public EC2)

* Name: `Bastion-Host`
* Subnet: Public
* Auto-assign Public IP: Enabled

#### 🔒 Private Instance

* Name: `Private-Instance`
* Subnet: Private
* No Public IP

---

## 🔐 Security Group Configuration

### Bastion Host SG

* Allow:

  * SSH (22) → Your IP
  * ICMP → Anywhere

---

### Private Instance SG

* Allow:

  * SSH → From Bastion SG
  * ICMP → Anywhere

> 💡 Only Bastion can access Private Instance

---

## 🔎 Testing & Verification

---

### 🔹 Step 1: Connect to Bastion Host

```bash
ssh -i key.pem ec2-user@<Public-IP>
```

---

### 🔹 Step 2: Copy Private Key to Bastion Host (SCP)

```bash
scp -i key.pem key.pem ec2-user@<Public-IP>:/home/ec2-user/
```

👉 Copies key from local machine → Bastion Host

---

### 🔹 Step 3: Set Permission on Bastion

```bash
chmod 400 key.pem
```

---

### 🔹 Step 4: Connect to Private Instance

```bash
ssh -i key.pem ec2-user@<Private-IP>
```

---

### 🔹 Step 5: Verify Internet Access

```bash
ping google.com
```

---

### 🔹 Step 6: Expected Output

* SSH into Private Instance ✅
* Ping works successfully ✅
* No public access to private instance ✅

---

## 🔥 Alternative (Advanced - No Key Copy)

```bash
ssh -A -i key.pem ec2-user@<Public-IP>
ssh ec2-user@<Private-IP>
```

👉 Uses SSH Agent Forwarding (More Secure)

---

## 📘 Key Learnings

* Public vs Private Subnet
* Role of NAT Gateway
* Secure access using Bastion Host
* SCP for file transfer
* Route Tables & Security Groups

---

## 🧹 Cleanup Steps

1. Terminate EC2 instances
2. Delete NAT Gateway
3. Release Elastic IP
4. Delete Route Tables
5. Detach & Delete Internet Gateway
6. Delete Subnets
7. Delete VPC

---

## 🎉 Conclusion

You have successfully built a **secure AWS architecture** using Amazon Web Services where:

* Public resources are accessible
* Private resources are protected
* Internet access is controlled via NAT Gateway
* Secure access is achieved using Bastion Host & SCP

---
