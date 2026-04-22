# 🌐 AWS VPC Peering Practical

## 🔗 Cross-Region Communication Using Bastion Host (Europe ↔ Hyderabad)

---

## 📌 Project Description

This practical demonstrates how to establish **secure communication between two Virtual Private Clouds (VPCs)** located in **different AWS regions** using **VPC Peering**.

The setup includes:

* A **Europe region VPC** with both Public and Private subnets
* A **Hyderabad region VPC** with a Private subnet
* A **Bastion Host (Public EC2)** to securely access private resources
* Cross-region communication using **private IP addresses**

This project is useful for understanding **multi-region architectures**, **secure access patterns**, and **network-level connectivity in AWS**.

---

## 🎯 Objectives

* Create and configure **two VPCs in different regions**
* Establish a **VPC Peering connection (cross-region)**
* Configure **route tables for inter-VPC communication**
* Use a **Bastion Host** to access private instances
* Verify connectivity using **SSH and ping**

---

## 🏗️ Architecture Overview

### 🌍 Europe Region (Primary VPC)

* VPC CIDR: `10.0.0.0/16`
* Public Subnet: `10.0.1.0/24`
* Private Subnet: `10.0.2.0/24`
* Resources:

  * Public EC2 Instance (**Bastion Host**)
  * Private EC2 Instance

---

### 🌏 Hyderabad Region (Secondary VPC)

* VPC CIDR: `192.168.0.0/16`
* Private Subnet: `192.168.1.0/24`
* Resources:

  * Private EC2 Instance

---

### 🔄 Connectivity

* Cross-region **VPC Peering**
* Communication via **Private IP addresses only**

---

## 🪜 Step-by-Step Implementation

---

### 1️⃣ Create VPC in Europe Region

* Navigate to **VPC Dashboard**
* Create VPC:

  * Name: `Europe-VPC`
  * CIDR: `10.0.0.0/16`

Create Subnets:

* Public Subnet → `10.0.1.0/24`
* Private Subnet → `10.0.2.0/24`

---

### 2️⃣ Create VPC in Hyderabad Region

* Create VPC:

  * Name: `Hyderabad-VPC`
  * CIDR: `192.168.0.0/16`

Create Subnet:

* Private Subnet → `192.168.1.0/24`

---

### 3️⃣ Configure Internet Gateway (Europe VPC)

* Create Internet Gateway
* Attach it to `Europe-VPC`

Update Route Table for Public Subnet:

```bash
Destination: 0.0.0.0/0
Target: Internet Gateway
```

---

### 4️⃣ Launch EC2 Instances

#### Europe Region

* **Public Instance (Bastion Host)**

  * Enable Public IP
* **Private Instance**

  * Disable Public IP

#### Hyderabad Region

* **Private Instance**

  * No Public IP

---

### 5️⃣ Configure Security Groups

For testing, allow all traffic:

```bash
Type: All Traffic
Protocol: All
Port: All
Source: 0.0.0.0/0
```

> ⚠️ Note: This is only for learning purposes. In real-world scenarios, restrict access properly.

---

### 6️⃣ Create VPC Peering Connection

* Go to **VPC → Peering Connections**
* Click **Create Peering Connection**

Configuration:

* Name: `Europe-Hyderabad-Peering`
* Requestor VPC: Europe-VPC
* Acceptor VPC: Hyderabad-VPC (different region)

---

### 7️⃣ Accept Peering Request

* Switch to Hyderabad region
* Open Peering Connections
* Accept the request

✅ Status should become **Active**

---

### 8️⃣ Update Route Tables (Critical Step 🚨)

#### Europe VPC Route Table

```bash
Destination: 192.168.0.0/16
Target: VPC Peering Connection
```

---

#### Hyderabad VPC Route Table

```bash
Destination: 10.0.0.0/16
Target: VPC Peering Connection
```

---

### 9️⃣ Transfer Private Key to Bastion Host

From your local system:

```bash
scp -i your-key.pem your-key.pem ubuntu@<Public-IP>:/home/ubuntu/
```

---

### 🔟 Connect to Bastion Host

```bash
ssh -i your-key.pem ubuntu@<Public-IP>
```

---

### 1️⃣1️⃣ Set Key Permissions

```bash
chmod 400 your-key.pem
```

---

### 1️⃣2️⃣ SSH into Private Instance (Europe)

```bash
ssh -i your-key.pem ubuntu@<Private-IP-Europe>
```

---

### 1️⃣3️⃣ Test Cross-Region Connectivity

Ping Hyderabad private instance:

```bash
ping <Private-IP-Hyderabad>
```

✅ Successful ping confirms:

* VPC Peering is working
* Route tables are correctly configured
* Network communication is established

---

## 🔍 Validation Checklist

* ✔️ Peering connection is **Active**
* ✔️ Correct routes added in both VPCs
* ✔️ Instances launched in correct subnets
* ✔️ Bastion host accessible via SSH
* ✔️ Private instances reachable internally
* ✔️ Cross-region ping successful

---

## ⚠️ Key Concepts to Remember

* **VPC Peering is Non-Transitive**
* Only **private IP communication** is allowed
* CIDR blocks **must not overlap**
* Cross-region peering may introduce **latency**
* Bastion host is used for **secure access to private resources**

---

## 🚀 Conclusion

In this practical, we successfully built a **cross-region AWS network architecture** by connecting two VPCs using **VPC Peering**.

We also implemented a **secure access mechanism using a Bastion Host** and verified communication between private instances using **SSH and ICMP (ping)**.

This setup is widely used in real-world scenarios like:

* Multi-region deployments
* Hybrid cloud architectures
* Secure internal communication systems

---
---
