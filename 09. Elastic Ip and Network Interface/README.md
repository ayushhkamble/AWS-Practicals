# 🚀 AWS Practical Guide

## 📌 Assigning Elastic IP & Network Interface to EC2 Instance

---

## 🌟 Project Overview

This practical demonstrates how to:

* Allocate and attach an **Elastic IP (EIP)**
* Create and attach an **Elastic Network Interface (ENI)**
* Understand how networking works in **Amazon EC2**

---

## 🎯 Objective

* Provide a **static public IP** using Elastic IP
* Add an additional **network interface** to an EC2 instance
* Test connectivity and verify configuration

---

## 🧱 Prerequisites

* AWS Account
* Basic knowledge of **EC2 instances**
* A running EC2 instance (Ubuntu/Amazon Linux)

---

## 🏗️ Architecture

```
EC2 Instance
   │
   ├── Primary Network Interface (eth0)
   │       └── Elastic IP (Static Public IP)
   │
   └── Secondary Network Interface (eth1)
           └── Private Communication
```

---

# 🛠️ Step-by-Step Implementation

---

## 🔹 Step 1: Launch EC2 Instance

1. Go to AWS Console → **EC2 Dashboard**
2. Click **Launch Instance**
3. Configure:

   * Name: `MyEC2`
   * AMI: Ubuntu / Amazon Linux
   * Instance Type: t2.micro
   * Network: Default VPC
4. Allow:

   * SSH (Port 22)
5. Launch the instance

---

## 🔹 Step 2: Allocate Elastic IP

1. Go to **EC2 Dashboard → Elastic IPs**
2. Click **Allocate Elastic IP Address**
3. Keep default settings
4. Click **Allocate**

✅ You will now get a **public static IP**

---

## 🔹 Step 3: Associate Elastic IP with EC2

1. Select the allocated Elastic IP
2. Click **Actions → Associate Elastic IP**
3. Choose:

   * Resource Type: Instance
   * Select your EC2 instance
4. Click **Associate**

✅ Your EC2 instance now has a **static public IP**

---

## 🔹 Step 4: Connect to Instance

Use SSH:

```bash
ssh -i your-key.pem ubuntu@<Elastic-IP>
```

---

## 🔹 Step 5: Create Network Interface (ENI)

1. Go to **EC2 Dashboard → Network Interfaces**
2. Click **Create Network Interface**
3. Configure:

   * Name: `MyENI`
   * Subnet: Same as EC2 instance
   * Security Group: Same as EC2
4. Click **Create**

---

## 🔹 Step 6: Attach ENI to EC2 Instance

1. Select the created ENI
2. Click **Actions → Attach**
3. Choose:

   * Instance: Your EC2 instance
   * Device Index: `1` (for second interface)
4. Click **Attach**

---

## 🔹 Step 7: Verify Network Interface in EC2

Login to instance:

```bash
ip a
```

✅ You should see:

* `eth0` → Primary interface
* `eth1` → Secondary interface (ENI)

---

## 🔹 Step 8: Configure Secondary Interface (Optional)

For Ubuntu:

```bash
sudo dhclient eth1
```

Check IP:

```bash
ip a
```

---

## 🔹 Step 9: Test Connectivity

Ping external server:

```bash
ping google.com
```

Or test internal communication using private IPs.

---

# 📊 Key Concepts

### 🌐 Elastic IP

* Static public IPv4 address
* Remains constant even after instance restart

### 🔌 Network Interface (ENI)

* Virtual network card
* Can have:

  * Private IP
  * Security groups
  * MAC address

---

# ⚠️ Important Notes

* Elastic IP is **chargeable if not attached**
* One instance can have **multiple ENIs** (depends on instance type)
* Always use the same subnet while attaching ENI

---

# ✅ Result

* Successfully assigned **Elastic IP** to EC2
* Attached **secondary network interface (ENI)**
* Verified networking inside instance

---

# 📌 Conclusion

This practical helps in understanding **AWS networking fundamentals**, including static IP allocation and multiple network interfaces in **Amazon Web Services**, which are essential for real-world cloud deployments.

---
