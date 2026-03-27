# 🌐 AWS VPC Peering Practical Guide

## 🔐 Private Access Using EC2 Instance Connect Endpoint (No Bastion Host)

---

## 📌 Project Description

This practical demonstrates how to establish **cross-region VPC Peering** and securely access **private EC2 instances without using a Bastion Host**, by leveraging an **EC2 Instance Connect Endpoint (EIC Endpoint)**.

In this setup:

* Two VPCs are created in **different AWS regions**
* Each VPC contains **only a Private Subnet**
* One **private EC2 instance** is launched in each VPC
* An **EC2 Instance Connect Endpoint** is used to access private instances
* Connectivity is verified using **ping between private IPs**

---

## 🎯 Objectives

* Create **two VPCs in different regions**
* Establish **VPC Peering (cross-region)**
* Launch **private EC2 instances**
* Configure **EC2 Instance Connect Endpoint**
* Access private instances **without public IP / bastion**
* Verify communication using **ping**

---

## 🏗️ Architecture Overview

### 🌍 Europe Region

* VPC CIDR: `10.0.0.0/16`
* Private Subnet: `10.0.1.0/24`
* Private EC2 Instance
* EC2 Instance Connect Endpoint

---

### 🌏 Hyderabad Region

* VPC CIDR: `192.168.0.0/16`
* Private Subnet: `192.168.1.0/24`
* Private EC2 Instance
* EC2 Instance Connect Endpoint

---

### 🔗 Connectivity Flow

```
Your System
   ↓
EC2 Instance Connect Endpoint
   ↓
Private EC2 Instance (Europe)
   ↓
VPC Peering
   ↓
Private EC2 Instance (Hyderabad)
```

---

## 🪜 Step-by-Step Implementation

---

### 1️⃣ Create VPCs in Both Regions

#### Europe Region

* VPC: `Europe-VPC`
* CIDR: `10.0.0.0/16`
* Private Subnet: `10.0.1.0/24`

#### Hyderabad Region

* VPC: `Hyderabad-VPC`
* CIDR: `192.168.0.0/16`
* Private Subnet: `192.168.1.0/24`

---

### 2️⃣ Launch Private EC2 Instances

In both regions:

* Select respective VPC & subnet
* Disable Public IP
* Use same key pair (recommended)

---

### 3️⃣ Configure Security Groups

Allow SSH and ICMP:

```bash
Type: SSH (22) → Source: Anywhere or Your IP
Type: All ICMP → Source: VPC CIDR
```

Example:

* Europe SG → `10.0.0.0/16`
* Hyderabad SG → `192.168.0.0/16`

---

### 4️⃣ Create VPC Peering Connection

* Go to **VPC → Peering Connections**
* Create Peering:

Configuration:

* Requestor: Europe-VPC
* Acceptor: Hyderabad-VPC (different region)

---

### 5️⃣ Accept Peering Request

* Switch to Hyderabad region
* Accept connection

✅ Status → **Active**

---

### 6️⃣ Update Route Tables (Important 🔥)

#### Europe Route Table

```bash
Destination: 192.168.0.0/16
Target: VPC Peering
```

#### Hyderabad Route Table

```bash
Destination: 10.0.0.0/16
Target: VPC Peering
```

---

### 7️⃣ Create EC2 Instance Connect Endpoint

#### In Europe Region

* Go to **VPC → Endpoints**
* Click **Create Endpoint**

Configuration:

* Type: **EC2 Instance Connect Endpoint**
* VPC: Europe-VPC
* Subnet: Private Subnet
* Security Group: Allow SSH

---

#### In Hyderabad Region

Repeat the same steps for Hyderabad VPC.

---

### 8️⃣ Connect to Private Instance (Using Endpoint)

* Go to **EC2 → Instances**
* Select private instance
* Click **Connect**
* Choose:

  * **EC2 Instance Connect Endpoint**
* Click **Connect**

✅ You will directly log into the private instance (no bastion needed)

---

### 9️⃣ Verify Internal Connectivity

From Europe private instance:

```bash
ping <Private-IP-Hyderabad>
```

---

From Hyderabad private instance:

```bash
ping <Private-IP-Europe>
```

---

✅ Successful ping confirms:

* VPC Peering is working
* Route tables are correct
* Security groups allow ICMP
* Endpoint access is working

---

## 🔍 Validation Checklist

* ✔️ Both VPCs created correctly
* ✔️ Private instances launched
* ✔️ No public IP used
* ✔️ Peering connection is active
* ✔️ Route tables updated
* ✔️ EC2 Endpoint created
* ✔️ SSH access via endpoint working
* ✔️ Ping successful between instances

---

## ⚠️ Important Notes

* EC2 Instance Connect Endpoint is **regional**
* Works only for **SSH access (port 22)**
* Eliminates need for **Bastion Host**
* VPC Peering allows **private IP communication**
* CIDR ranges must **not overlap**

---

## 🚀 Conclusion

In this practical, we implemented a **modern and secure AWS architecture** by combining:

* **Cross-region VPC Peering**
* **Private EC2 instances**
* **EC2 Instance Connect Endpoint (no bastion host)**

We successfully accessed private instances and verified inter-region communication using **ping**, ensuring secure and efficient networking.

--- 
