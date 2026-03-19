# 🌐🚀 AWS NAT Gateway Practical Guide

### 🔐 Secure Private Instance Access using Bastion Host

---

## 📌 Project Overview

In this practical, we build a **secure AWS network architecture** where:

* A **custom VPC** is created with `/16` CIDR
* Two subnets are configured:

  * 🌍 Public Subnet (Internet-facing)
  * 🔒 Private Subnet (Isolated)
* Internet access is managed using:

  * 🌐 Internet Gateway (IGW)
  * 🔁 NAT Gateway
* Two EC2 instances are launched:

  * 🖥️ Bastion Host (Public Instance)
  * 🔐 Private Instance (No Public IP)
* Secure login is performed using **SCP + SSH**
* Internet connectivity is verified from private instance

---

## 🎯 Objectives

✔ Build a custom VPC network
✔ Enable controlled internet access
✔ Implement secure SSH using Bastion Host
✔ Understand routing with NAT Gateway
✔ Test outbound internet from private instance

---

## 🏗️ Step 1: Create Custom VPC

1. Navigate to **VPC Dashboard**
2. Click **Create VPC**
3. Configure:

| Setting    | Value       |
| ---------- | ----------- |
| Name       | MyVPC       |
| CIDR Block | 10.0.0.0/16 |
| Tenancy    | Default     |

✅ This CIDR allows up to **65,536 IP addresses**

---

## 🌐 Step 2: Create Subnets

### 🌍 Public Subnet

| Setting        | Value         |
| -------------- | ------------- |
| Name           | Public-Subnet |
| CIDR           | 10.0.1.0/24   |
| Auto Public IP | Enabled       |

👉 Used for NAT Gateway & Bastion Host

---

### 🔒 Private Subnet

| Setting        | Value          |
| -------------- | -------------- |
| Name           | Private-Subnet |
| CIDR           | 10.0.2.0/24    |
| Auto Public IP | Disabled       |

👉 Used for secure backend instances

---

## 🌍 Step 3: Create Internet Gateway (IGW)

1. Go to **Internet Gateways**
2. Click **Create**
3. Name: `MyIGW`
4. Attach to **MyVPC**

✅ Enables internet connectivity for public subnet

---

## 📡 Step 4: Create NAT Gateway

1. Navigate to **NAT Gateways**
2. Click **Create NAT Gateway**
3. Select:

   * Subnet: `Public-Subnet`
   * Allocate new Elastic IP

⏳ Wait until status becomes **Available**

💡 **Why NAT Gateway?**
It allows private instances to access the internet **without exposing them publicly**

---

## 📊 Step 5: Configure Route Tables

---

### 🌍 Public Route Table

1. Create: `Public-RT`
2. Add route:

```
0.0.0.0/0 → Internet Gateway (MyIGW)
```

3. Associate with:

   * Public-Subnet

---

### 🔒 Private Route Table (NAT Route Table)

1. Create: `Private-RT`
2. Add route:

```
0.0.0.0/0 → NAT Gateway
```

3. Associate with:

   * Private-Subnet

---

## 💻 Step 6: Launch EC2 Instances

---

### 🖥️ Public Instance (Bastion Host)

| Setting   | Value           |
| --------- | --------------- |
| Name      | Public-Instance |
| Subnet    | Public-Subnet   |
| Public IP | Enabled         |

🔐 Security Group:

* Allow **SSH (Port 22)** from your IP

---

### 🔐 Private Instance

| Setting   | Value            |
| --------- | ---------------- |
| Name      | Private-Instance |
| Subnet    | Private-Subnet   |
| Public IP | Disabled         |

🔐 Security Group:

* Allow SSH **only from Public Instance**

---

## 🔐 Step 7: Transfer Key using SCP

From your **local machine**:

```bash
scp -i mykey.pem mykey.pem ubuntu@<Public-IP>:/home/ubuntu/
```

👉 This copies your private key to the Bastion Host

---

## 🔑 Step 8: Connect to Bastion Host

```bash
ssh -i mykey.pem ubuntu@<Public-IP>
```

Set permission:

```bash
chmod 400 mykey.pem
```

---

## 🔄 Step 9: SSH into Private Instance

From inside Bastion Host:

```bash
ssh -i mykey.pem ubuntu@<Private-IP>
```

✅ Now you are inside the private instance securely

---

## 🌐 Step 10: Verify Internet Connectivity

Run:

```bash
ping google.com
```

✔ Successful response = NAT Gateway working
❌ Failure = Check routes / SG / NAT status

---

## 🧠 Architecture Diagram (Text View)

```
                 🌐 Internet
                      │
                      ▼
            ┌───────────────────┐
            │ Internet Gateway │
            └───────────────────┘
                      │
        ┌───────────────────────────┐
        │      Public Subnet        │
        │                           │
        │  🖥️ Bastion Host          │
        │  📡 NAT Gateway           │
        └───────────────────────────┘
                      │
        ┌───────────────────────────┐
        │      Private Subnet       │
        │                           │
        │  🔐 Private EC2 Instance  │
        └───────────────────────────┘
                      │
                🌍 Internet (via NAT)
```

---

## 🔍 Key Concepts Explained

### 🌐 Internet Gateway (IGW)

* Allows inbound & outbound internet access
* Attached to VPC

### 🔁 NAT Gateway

* Allows **only outbound internet**
* Keeps private instances secure

### 🔐 Bastion Host

* Public EC2 used to access private EC2
* Acts as a **secure jump server**

---

## ⚠️ Common Errors & Fixes

| Issue             | Solution                  |
| ----------------- | ------------------------- |
| Ping not working  | Check NAT Gateway route   |
| SSH fails         | Verify security groups    |
| Timeout error     | Ensure IGW attached       |
| Permission denied | Run `chmod 400 mykey.pem` |

---

## 🎯 Real-World Use Case

This architecture is used in:

* 🔒 Secure production environments
* 🏦 Banking & enterprise systems
* ☁️ Cloud-native applications
* 🧠 Data processing backends

---

## ✅ Conclusion

You have successfully implemented:

✔ Custom VPC with subnet segmentation
✔ Internet Gateway for public access
✔ NAT Gateway for private internet access
✔ Bastion Host for secure SSH
✔ End-to-end connectivity verification

---

## 💡 Bonus Tip

You can extend this setup by adding:

* 🔁 Auto Scaling Groups
* ⚖️ Load Balancer
* 📊 CloudWatch Monitoring
* 🔐 IAM Roles for better security

---
