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
* Connectivity is verified using **SSH and ping commands**

---

## 🧰 Services Used

* Amazon Web Services (AWS)
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

### 🔹 Step 1: Create a Custom VPC

* Go to **VPC Dashboard → Create VPC**
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

> 💡 Internet Gateway allows public subnet resources to access the internet.

---

### 🔹 Step 4: Create Route Tables

#### 🌍 Public Route Table

* Add Route:

  * `0.0.0.0/0 → Internet Gateway`
* Associate with **Public Subnet**

#### 🔒 Private Route Table

* Initially no internet route (will add NAT later)

---

### 🔹 Step 5: Allocate Elastic IP

* Go to EC2 → Elastic IPs
* Allocate new Elastic IP

---

### 🔹 Step 6: Create NAT Gateway

* Go to VPC → NAT Gateways
* Create NAT Gateway in **Public Subnet**
* Attach **Elastic IP**

> 💡 NAT Gateway must always be in a **Public Subnet**

---

### 🔹 Step 7: Update Private Route Table

* Add Route:

  * `0.0.0.0/0 → NAT Gateway`
* Associate with **Private Subnet**

---

### 🔹 Step 8: Launch EC2 Instances

#### 🌍 Public EC2 (Bastion Host)

* Name: `Bastion-Host`
* Subnet: Public
* Auto-assign Public IP: Enabled

#### 🔒 Private EC2

* Name: `Private-Instance`
* Subnet: Private
* No Public IP

---

## 🔐 Security Group Configuration

### Bastion Host SG

* Allow:

  * SSH (Port 22) → Your IP
  * ICMP → Anywhere

### Private Instance SG

* Allow:

  * SSH → From Bastion Host SG
  * ICMP → Anywhere

> 💡 This ensures only Bastion Host can access Private Instance.

---

## 🔎 Testing & Verification

### 🔹 Step 1: Connect to Bastion Host

```bash
ssh -i key.pem ec2-user@<Public-IP>
```

---

### 🔹 Step 2: Connect to Private Instance

```bash
ssh -i key.pem ec2-user@<Private-IP>
```

---

### 🔹 Step 3: Check Internet Connectivity

```bash
ping google.com
```

---

### 🔹 Step 4: Expected Behavior

* Ping should **work successfully ✅**
* Private instance accesses internet via NAT Gateway

> 💡 NAT Gateway allows **outbound traffic only**, not inbound.

---

## ✅ Expected Output

* Successful SSH into Private Instance via Bastion Host
* `ping google.com` works from Private Instance
* No direct public access to Private Instance

---

## 📘 Key Learnings

* Difference between **Public vs Private Subnet**
* Role of **NAT Gateway in secure internet access**
* Importance of **Route Tables & Security Groups**
* Bastion Host acts as a **secure entry point**
* Private instances remain **hidden from the internet**

---

## 🧹 Cleanup Steps

To avoid AWS charges:

1. Terminate EC2 Instances
2. Delete NAT Gateway
3. Release Elastic IP
4. Delete Route Tables
5. Detach & Delete Internet Gateway
6. Delete Subnets
7. Delete VPC

---

## 🎉 Conclusion

You have successfully built a **secure AWS network architecture** where:

* Public resources are accessible
* Private resources are protected
* Internet access is controlled using **NAT Gateway**

---
