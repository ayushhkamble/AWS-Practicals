# 🚀 Application Load Balancer (ALB) with Path-Based Routing using Nginx on Ubuntu EC2

---

## 📌 Project Overview

This project demonstrates how to configure an **Application Load Balancer (ALB)** in AWS to route traffic based on URL paths.

We will deploy **6 Ubuntu EC2 instances**, host websites using **Nginx**, and route traffic like:

- `/` → Home Page  
- `/sell` → Sell Page  
- `/fashion` → Fashion Page  

---

## 🎯 Objective

By the end of this practical, you will:

- ✅ Launch and configure EC2 instances (Ubuntu)
- ✅ Install and configure Nginx using User Data
- ✅ Create Target Groups
- ✅ Configure an Application Load Balancer
- ✅ Implement Path-Based Routing
- ✅ Test routing in browser

---

## 🧰 AWS Services Used

- EC2 (Ubuntu Server)
- Application Load Balancer (ALB)
- Target Groups
- Security Groups
- Nginx Web Server

---

## 📐 Architecture Diagram

```

```
                  🌐 Internet
                       │
              ┌──────────────────┐
              │   ALB (HTTP 80)  │
              └────────┬─────────┘
                       │
  ┌────────────┬────────────┬────────────┐
  │            │            │
```

tg-home      tg-sell     tg-fashion
│            │            │
┌──────────┐ ┌──────────┐ ┌──────────┐
│ EC2 (2x) │ │ EC2 (2x) │ │ EC2 (2x) │
│  Home    │ │  Sell    │ │ Fashion  │
└──────────┘ └──────────┘ └──────────┘

````

---

## 🌐 Routing Logic

| Path        | Target Group | EC2 Servers |
|------------|-------------|-------------|
| `/`        | tg-home     | 2 instances |
| `/sell`    | tg-sell     | 2 instances |
| `/fashion` | tg-fashion  | 2 instances |

---

## 📝 Prerequisites

- AWS Account
- Basic knowledge of EC2
- Key Pair for SSH
- Ubuntu AMI (20.04 / 22.04)

---

# ⚙️ STEP-BY-STEP IMPLEMENTATION

---

## 🚀 Step 1: Launch 6 EC2 Instances (Ubuntu)

### 🔹 Configuration

- AMI: Ubuntu Server
- Instance Type: t2.micro
- Number of Instances: 6
- Key Pair: Select your key

---

### 🔐 Security Group Settings

Allow:

| Type | Port | Source |
|------|------|--------|
| SSH  | 22   | Your IP |
| HTTP | 80   | 0.0.0.0/0 |

---

## 💻 Step 2: Add User Data (IMPORTANT)

👉 User Data automatically installs Nginx and creates a web page.

---

### 🏠 Home Servers (2 Instances)

```bash
#!/bin/bash
apt update -y
apt install nginx -y
systemctl start nginx
systemctl enable nginx

echo "<h1>Welcome to Home Page</h1>" > /var/www/html/index.html
````

---

### 🛒 Sell Servers (2 Instances)

```bash
#!/bin/bash

apt update -y
apt install nginx -y

systemctl start nginx
systemctl enable nginx

mkdir -p /var/www/html/sell

echo "<h1>Welcome to Sell Page</h1>" > /var/www/html/sell/index.html
```

---

### 👗 Fashion Servers (2 Instances)

```bash
#!/bin/bash

apt update -y
apt install nginx -y

systemctl start nginx
systemctl enable nginx

mkdir -p /var/www/html/fashion

echo "<h1>Welcome to Fashion Page</h1>" > /var/www/html/fashion/index.html
```

---

✅ After launching, wait 2–3 minutes for setup to complete.

---

## 🎯 Step 3: Create Target Groups

Go to:

👉 EC2 Dashboard → Target Groups → Create Target Group

---

### 1️⃣ tg-home

* Protocol: HTTP
* Port: 80
* Register: 2 Home instances

---

### 2️⃣ tg-sell

* Protocol: HTTP
* Port: 80
* Register: 2 Sell instances

---

### 3️⃣ tg-fashion

* Protocol: HTTP
* Port: 80
* Register: 2 Fashion instances

---

## 🌐 Step 4: Create Application Load Balancer

Go to:

👉 EC2 → Load Balancers → Create Load Balancer

---

### Configuration:

* Type: Application Load Balancer
* Name: alb-path-routing
* Scheme: Internet-facing
* IP Type: IPv4

---

### Network Mapping:

* Select your VPC
* Select **at least 2 Availability Zones**

---

### Security Group:

* Allow HTTP (80)

---

### Listener:

* Protocol: HTTP
* Port: 80

---

## 🔀 Step 5: Configure Listener Rules (IMPORTANT)

Go to:

👉 ALB → Listeners → View/Edit Rules

---

### 🟢 Default Rule:

* Forward to → `tg-home`

---

### ➕ Add Rule for Sell

* IF Path = `/sell*`
* THEN Forward to → `tg-sell`

---

### ➕ Add Rule for Fashion

* IF Path = `/fashion*`
* THEN Forward to → `tg-fashion`

---

## ⏳ Step 6: Wait for Health Checks

* Go to Target Groups
* Ensure all targets show:

✅ **Healthy**

---

## 🔍 Step 7: Testing the Setup

Copy your **ALB DNS Name**

---

### 🏠 Test Home Page

```
http://<ALB-DNS>/
```

---

### 🛒 Test Sell Page

```
http://<ALB-DNS>/sell
```

---

### 👗 Test Fashion Page

```
http://<ALB-DNS>/fashion
```

---

## 📊 Expected Output

* Home → "Welcome to Home Page"
* Sell → "Welcome to Sell Page"
* Fashion → "Welcome to Fashion Page"

✅ Load is balanced across 2 servers for each path

---

# 🛠️ Troubleshooting

---

### ❌ Problem: Target Unhealthy

🔧 Solution:

SSH into instance:

```bash
sudo systemctl status nginx
```

If not running:

```bash
sudo systemctl start nginx
```

---

### ❌ Problem: Same Page Everywhere

* Check Listener Rules
* Verify correct Target Group mapping

---

### ❌ Problem: 404 Not Found

* Ensure path rules:

  * `/sell*`
  * `/fashion*`

---

### ❌ Problem: Website Not Opening

* Check Security Groups
* Ensure port 80 is open
* Check instance is running

---

# 🧹 Clean Up (IMPORTANT)

To avoid AWS charges:

1. Delete Load Balancer
2. Delete Target Groups
3. Terminate all EC2 instances
4. Delete Security Groups (if unused)

---

## ✅ Final Result

🎉 You have successfully:

* Deployed **6 Ubuntu EC2 servers**
* Hosted websites using **Nginx**
* Configured **Application Load Balancer**
* Implemented **Path-Based Routing**
* Achieved **Scalable Architecture**

---

## 📌 Use Cases

* Microservices Architecture
* Multi-page websites
* Scalable web applications

---
