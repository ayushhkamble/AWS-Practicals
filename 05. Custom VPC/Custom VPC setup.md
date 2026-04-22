# 🚀 AWS Practical Guide: Custom VPC Setup with EC2 Connectivity Verification

---

## 📌 Project Title

**Setting Up a Custom VPC in AWS and Verifying Internet Connectivity Using EC2**

---

## 🎯 Objective

The objective of this practical is to:

* Create a **Custom Virtual Private Cloud (VPC)** from scratch
* Configure networking components like **subnets, route tables, and internet gateway**
* Launch an **EC2 instance** inside the VPC
* Verify internet connectivity using the `ping` command

---

## 🧰 Services Used

* Amazon VPC
* Subnet
* Internet Gateway (IGW)
* Route Table
* Amazon EC2
* Security Groups

---

## 🏗 Architecture Overview

```
        Internet
            |
     -----------------
     | Internet GW  |
     -----------------
            |
     -----------------
     |   Custom VPC  |  (CIDR: 10.0.0.0/16)
     -----------------
            |
     -----------------
     | Public Subnet | (10.0.1.0/24)
     -----------------
            |
     -----------------
     |   EC2 Instance |
     -----------------
```

---

## ⚙ Step-by-Step Implementation

### 1️⃣ Create a Custom VPC

* Go to **VPC Dashboard → Create VPC**
* Choose **VPC only**
* Name: `MyVPC`
* CIDR Block: `10.0.0.0/16`

✅ Click **Create VPC**

---

### 2️⃣ Create a Public Subnet

* Go to **Subnets → Create Subnet**
* Select `MyVPC`
* Name: `PublicSubnet`
* CIDR Block: `10.0.1.0/24`
* Availability Zone: Any (e.g., ap-south-1a)

✅ Click **Create Subnet**

---

### 3️⃣ Create and Attach Internet Gateway

* Go to **Internet Gateways → Create**
* Name: `MyIGW`
* Click **Create**

👉 Attach it to VPC:

* Select IGW → Actions → Attach to VPC → Choose `MyVPC`

---

### 4️⃣ Create Route Table and Add Route

* Go to **Route Tables → Create**
* Name: `PublicRouteTable`
* Select `MyVPC`

👉 Edit Routes:

* Add route:

```
Destination: 0.0.0.0/0
Target: Internet Gateway (MyIGW)
```

---

### 5️⃣ Associate Route Table with Subnet

* Go to **Subnet Associations**
* Click **Edit**
* Select `PublicSubnet`

---

### 6️⃣ Launch an EC2 Instance

* Go to **EC2 Dashboard → Launch Instance**
* Name: `MyEC2`
* AMI: Ubuntu / Amazon Linux
* Instance Type: t2.micro (Free Tier)

👉 Network Settings:

* VPC: `MyVPC`
* Subnet: `PublicSubnet`
* Enable **Auto-assign Public IP**

---

### 7️⃣ Configure Security Group

Allow the following:

| Type | Protocol | Port | Source    |
| ---- | -------- | ---- | --------- |
| SSH  | TCP      | 22   | Your IP   |
| ICMP | All      | All  | 0.0.0.0/0 |

---

## 🔐 Connect to EC2 Instance

### 💻 For Linux / Mac:

```bash
chmod 400 mykey.pem
ssh -i mykey.pem ec2-user@<Public-IP>
```

### 🪟 For Windows (PowerShell):

```bash
ssh -i mykey.pem ec2-user@<Public-IP>
```

---

## 🌐 Verification

Once connected, run:

```bash
ping google.com
```

### ✅ Expected Output:

```
PING google.com (142.250.xxx.xxx) 56(84) bytes of data.
64 bytes from ...: icmp_seq=1 ttl=115 time=20 ms
64 bytes from ...: icmp_seq=2 ttl=115 time=18 ms
```

👉 This confirms:

* Internet Gateway is working
* Route table is configured correctly
* EC2 instance has internet access

---

## 📊 Key Observations / Learning Outcomes

* Learned how to build a **custom VPC architecture**
* Understood **routing and internet access flow**
* Gained hands-on experience with **EC2 networking**
* Verified connectivity using **ICMP (ping)**

---

## 🧹 Cleanup Steps (Avoid Charges 💸)

* Terminate EC2 instance
* Delete Subnet
* Detach & Delete Internet Gateway
* Delete Route Table
* Delete VPC

---

## 📚 Conclusion

In this practical, we successfully created a **custom AWS VPC** and configured all required networking components. We deployed an EC2 instance and verified internet connectivity using the `ping` command.

This forms the foundation for building secure and scalable cloud architectures.

---

## 💡 Tips & Common Mistakes

⚠ Forgetting to attach Internet Gateway → No internet
⚠ Missing route `0.0.0.0/0` → Connectivity fails
⚠ Security group blocking ICMP → Ping won’t work
⚠ Public IP not enabled → SSH fails

---

## 🌍 Real-World Use Case

This setup is commonly used for:

* Hosting **web applications**
* Creating **public-facing servers**
* Building **cloud-based infrastructure labs**

---
