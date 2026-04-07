# 🚀 Application Load Balancer (ALB) Setup with Private EC2 Instances (Best Practices)

---

## 📌 Project Overview

This project demonstrates how to design a **secure, highly available, and scalable AWS architecture** using an Application Load Balancer (ALB).

Instead of exposing EC2 instances directly to the internet (which is insecure ❌), we:

* Expose only the ALB 🌐
* Keep EC2 instances in private subnets 🔒
* Allow traffic only through controlled Security Groups 🔐

This is a **real-world production architecture pattern** used in companies.

---

## 🎯 Objective (In Simple Words)

We are building a system where:

* Users access a website via ALB
* ALB forwards requests to backend servers
* Backend servers are hidden from the internet

👉 This improves **security + scalability + reliability**

---

## 🏗️ Architecture Explanation

```
                Internet
                    │
            ┌───────────────┐
            │  ALB (Public) │
            └──────┬────────┘
                   │
        ┌──────────┴──────────┐
        │                     │
 ┌──────────────┐     ┌──────────────┐
 │ EC2 Private  │     │ EC2 Private  │
 │ WebServer 1  │     │ WebServer 2  │
 └──────────────┘     └──────────────┘
```

### 🔍 Flow of Request

1. User hits ALB DNS
2. ALB receives HTTP request
3. ALB checks target group
4. ALB forwards request to healthy EC2
5. EC2 responds → ALB → User

---

## 🧱 Step 1: Create VPC (Foundation of Network)

1. Go to **VPC Dashboard → Create VPC**
2. Choose **VPC only**
3. Configure:

   * Name: `my-vpc`
   * IPv4 CIDR: `10.0.0.0/16`

👉 This CIDR gives ~65,000 IPs (large enough for scaling)

---

## 🌐 Step 2: Create Subnets (Network Segmentation)

We divide the VPC into **public and private zones**.

### Public Subnets (for ALB)

* `10.0.1.0/24` (AZ1)
* `10.0.2.0/24` (AZ2)

### Private Subnets (for EC2)

* `10.0.3.0/24` (AZ1)
* `10.0.4.0/24` (AZ2)

👉 Why 2 AZs?

* Provides **High Availability** (if one AZ fails, other works)

---

## 🌍 Step 3: Internet Gateway (Enable Internet Access)

1. Create Internet Gateway
2. Attach it to VPC
3. Update Route Table for Public Subnets:

```
Destination: 0.0.0.0/0
Target: Internet Gateway
```

👉 This makes subnets **public**

---

## 🔐 Step 4: Security Groups (MOST IMPORTANT 🔥)

### 1️⃣ ALB Security Group (`alb-sg`)

| Type | Port | Source    |
| ---- | ---- | --------- |
| HTTP | 80   | 0.0.0.0/0 |

👉 Meaning: Anyone on internet can access ALB

---

### 2️⃣ EC2 Security Group (`private-ec2-sg`)

| Type | Port | Source |
| ---- | ---- | ------ |
| HTTP | 80   | alb-sg |

👉 Meaning:

* ONLY ALB can talk to EC2
* Direct internet access is BLOCKED ❌

💡 This is called **Security Group Referencing (Best Practice)**

---

## 💻 Step 5: Launch EC2 Instances (Private Servers)

Launch **2 EC2 instances**:

### Configuration:

* AMI: Ubuntu
* Instance: t2.micro
* Subnet: Private Subnets
* Auto-assign Public IP: ❌ Disabled
* Security Group: `private-ec2-sg`

👉 These instances are NOT reachable from internet

---

## 🧾 User Data Script (Auto Setup)

### WebServer 1

```bash
#!/bin/bash
apt update -y
apt install nginx -y
systemctl start nginx
systemctl enable nginx
echo "<h1>WebServer 1</h1>" > /var/www/html/index.html
```

### WebServer 2

```bash
#!/bin/bash
apt update -y
apt install nginx -y
systemctl start nginx
systemctl enable nginx
echo "<h1>WebServer 2</h1>" > /var/www/html/index.html
```

👉 This script runs automatically at launch

---

## 🎯 Step 6: Create Target Group

1. Go to **Target Groups → Create**

2. Settings:

   * Target Type: Instances
   * Protocol: HTTP
   * Port: 80
   * VPC: `my-vpc`

3. Register EC2 instances

👉 ALB sends traffic to this group

---

## ⚖️ Step 7: Create Application Load Balancer

1. Go to **Load Balancers → Create ALB**

### Configuration:

* Scheme: Internet-facing
* IP Type: IPv4
* Subnets: Select BOTH public subnets
* Security Group: `alb-sg`

---

### Listener Configuration

| Protocol | Port | Action                  |
| -------- | ---- | ----------------------- |
| HTTP     | 80   | Forward to Target Group |

👉 ALB will distribute traffic

---

## 🌐 Step 8: Testing (Final Step)

1. Copy ALB DNS name
2. Paste in browser
3. Refresh multiple times

### Output:

```
WebServer 1
WebServer 2
WebServer 1
WebServer 2
```

👉 This proves Load Balancing is working 🎉

---

## ✅ Verification Checklist

* EC2 instances are in **private subnet** ✔️
* No public IP on EC2 ✔️
* Target group shows **Healthy** ✔️
* ALB DNS accessible ✔️

---

## 🛠️ Troubleshooting (Very Important)

### ❌ ALB Not Loading

* Check ALB SG allows port 80

### ❌ Target Unhealthy

* Check Nginx is running
* Check health check path `/`

### ❌ 502 Error

* EC2 not responding

### ❌ No Load Balancing

* Hard refresh browser (Ctrl+Shift+R)

---

## 🔒 Best Practices (Real Industry Use)

### 🟢 High Availability

* Use multiple AZs (already done ✅)

### 🔐 Security

* Never expose EC2 publicly
* Use SG referencing

### 📈 Scalability

* Add Auto Scaling Group

### ❤️ Health Checks

* Ensure proper endpoint `/`

### 📊 Logging

* Enable ALB Access Logs (store in S3)

---

## 📁 Suggested GitHub Structure

```
aws-alb-project/
│── README.md
│── screenshots/
│── diagrams/
│── scripts/
│    ├── webserver1.sh
│    ├── webserver2.sh
```

---

## 📄 README Enhancement Tips

* Add screenshots after each step
* Highlight outputs
* Add explanation in simple words

---

## 🎉 Conclusion

You successfully built a **production-level AWS architecture** where:

* Only ALB is exposed
* EC2 instances are secure
* Traffic is balanced automatically

👉 This is exactly how modern cloud applications are deployed 🚀

---

## ⭐ Bonus Improvements

* Add HTTPS (SSL using ACM)
* Attach Route 53 domain
* Add Auto Scaling
* Use CloudWatch monitoring
---
