# 🚀 AWS Practical Guide: Creating a Custom VPC with Internet Connectivity

## 📖 Overview

This practical demonstrates how to build a **custom Virtual Private Cloud (VPC)** from scratch in Amazon Web Services and configure it to allow internet access.

You will:

* Create a VPC with CIDR `/16`
* Create a subnet
* Configure an Internet Gateway
* Update routing rules
* Launch an EC2 instance
* Verify internet connectivity using `ping`

---

## 🎯 Objective

To design and implement a **basic cloud network architecture** and verify external connectivity from an EC2 instance.

---

## 🧱 AWS Services Used

* Amazon VPC – for network isolation
* Amazon EC2 – for virtual server
* Internet Gateway – for internet access

---

## 🌐 Architecture Diagram (Conceptual)

```
                Internet
                    │
            ┌───────────────┐
            │ Internet GW   │
            └──────┬────────┘
                   │
        ┌──────────▼──────────┐
        │       MyVPC         │ (10.0.0.0/16)
        │                     │
        │   ┌─────────────┐   │
        │   │  MySubnet   │   │ (10.0.1.0/24)
        │   │             │
        │   │   EC2       │
        │   └─────────────┘
        │
        └─────────────────────┘
```

---

## 🛠️ Step-by-Step Implementation

### 🔹 Step 1: Create Custom VPC

1. Navigate to **VPC Dashboard**
2. Click **Create VPC**
3. Choose **VPC Only**
4. Enter details:

   * **Name tag**: `MyVPC`
   * **IPv4 CIDR block**: `10.0.0.0/16`
5. Click **Create VPC**

📌 *Explanation:*
A `/16` CIDR block provides **65,536 IP addresses**, suitable for scalable architectures.

---

### 🔹 Step 2: Create Subnet

1. Go to **Subnets → Create Subnet**
2. Select `MyVPC`
3. Enter:

   * **Subnet Name**: `MySubnet`
   * **Availability Zone**: ap-south-1a (or any)
   * **CIDR Block**: `10.0.1.0/24`
4. Click **Create Subnet**

📌 *Explanation:*
Subnet divides VPC into smaller networks. `/24` gives **256 IP addresses**.

---

### 🔹 Step 3: Create and Attach Internet Gateway

1. Go to **Internet Gateways**
2. Click **Create Internet Gateway**
3. Name: `MyIGW`
4. Click **Create**

### 🔗 Attach IGW to VPC

1. Select `MyIGW`
2. Click **Actions → Attach to VPC**
3. Choose `MyVPC`

📌 *Explanation:*
Without an Internet Gateway, your VPC **cannot access the internet**.

---

### 🔹 Step 4: Configure Route Table

1. Go to **Route Tables**
2. Select the route table associated with `MyVPC`
3. Go to **Routes → Edit Routes**
4. Add:

| Destination | Target |
| ----------- | ------ |
| 0.0.0.0/0   | MyIGW  |

5. Save changes

📌 *Explanation:*

* `0.0.0.0/0` = all internet traffic
* This route ensures traffic flows to Internet Gateway

---

### 🔹 Step 5: Associate Subnet with Route Table

1. Open the route table
2. Go to **Subnet Associations**
3. Click **Edit**
4. Select `MySubnet`
5. Save

📌 *Explanation:*
This ensures subnet uses the internet-enabled route table.

---

### 🔹 Step 6: Launch EC2 Instance

1. Go to EC2 Dashboard
2. Click **Launch Instance**
3. Configure:

   * **Name**: `MyServer`
   * **AMI**: Ubuntu / Amazon Linux
   * **Instance Type**: t2.micro (Free Tier)
   * **Network**: `MyVPC`
   * **Subnet**: `MySubnet`
   * Enable **Auto-assign Public IP**
4. Configure Security Group:

   * Allow **SSH (22)** from your IP
   * Allow **All outbound traffic**
5. Launch instance

📌 *Important:*
Public IP is required for internet access.

---

### 🔹 Step 7: Connect to EC2 Instance

```bash
ssh -i your-key.pem ubuntu@<Public-IP>
```

For Amazon Linux:

```bash
ssh -i your-key.pem ec2-user@<Public-IP>
```

---

### 🔹 Step 8: Verify Internet Connectivity

Run:

```bash
ping google.com
```

### ✅ Expected Result:

```
64 bytes from google.com: icmp_seq=1 ttl=115 time=20 ms
```

📌 This confirms:

* DNS resolution works
* Internet connectivity is active

---

## ⚠️ Troubleshooting

If ping fails, check:

| Issue          | Solution               |
| -------------- | ---------------------- |
| No Internet    | Check IGW attached     |
| Route missing  | Add 0.0.0.0/0 route    |
| No Public IP   | Enable auto-assign     |
| Security Group | Allow outbound traffic |
| NACL issues    | Allow inbound/outbound |

---

## 📊 Key Concepts Learned

* VPC CIDR planning
* Subnetting
* Internet Gateway usage
* Route tables and routing
* Public vs Private networking

---

## 🎉 Conclusion

In this practical, we successfully:

* Designed a custom network using Amazon VPC
* Enabled internet access using an Internet Gateway
* Deployed a virtual server using Amazon EC2
* Verified connectivity using the `ping` command

This forms the **foundation of AWS networking** and is essential for real-world cloud deployments.
