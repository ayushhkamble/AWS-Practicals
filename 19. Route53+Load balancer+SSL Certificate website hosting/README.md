# 🌐 Hosting a Website using Nginx on AWS EC2 with Custom Domain, Load Balancer, ACM SSL & Route 53

---

## 📌 Project Overview

This project demonstrates how to deploy a **secure, highly available website** on AWS using:

* EC2 (Ubuntu) for hosting
* Nginx as a web server
* Application Load Balancer (ALB)
* Target Group
* AWS Certificate Manager (SSL)
* Route 53 for DNS management
* Custom domain from Hostinger (**ayushk.online**)

---

## 🎯 Architecture

```
User (Browser)
      │
      ▼
Route 53 (DNS)
      │
      ▼
Application Load Balancer (HTTP → HTTPS)
      │
      ▼
Target Group
      │
      ▼
EC2 Instance (Nginx Web Server)
```

---

## 🧩 Prerequisites

* AWS Account
* Domain from Hostinger (ayushk.online)
* Basic Linux knowledge
* SSH client (Terminal / Git Bash / PuTTY)

---

# 🚀 Step 1: Launch EC2 Instance (Ubuntu)

### 🔹 Create Instance

* AMI: Ubuntu 22.04
* Instance Type: t2.micro (Free Tier)
* Key Pair: Create/download `.pem` file

### 🔹 Security Group Rules

| Type  | Port | Source    |
| ----- | ---- | --------- |
| SSH   | 22   | Your IP   |
| HTTP  | 80   | 0.0.0.0/0 |
| HTTPS | 443  | 0.0.0.0/0 |

---

### 🔹 Connect via SSH

```bash
chmod 400 key.pem
ssh -i key.pem ubuntu@<EC2_PUBLIC_IP>
```

---

# 🌐 Step 2: Install and Configure Nginx

### 🔹 Install Nginx

```bash
sudo apt update
sudo apt install nginx -y
```

### 🔹 Start and Enable

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

### 🔹 Upload Website Files

#### Option 1: Using nano

```bash
cd /var/www/html
sudo nano index.html
```

Paste your HTML code.

---

#### Option 2: Using wget

```bash
sudo apt install wget -y
cd /var/www/html
sudo wget <file-link>
```

---
### 🔹 Option 3: Upload Files using SCP (Recommended for Local Files)

If your website files are on your local machine, use **SCP (Secure Copy Protocol)** to transfer them to your EC2 instance.

---

### 📥 Copy Files from Local to EC2

Run this command from your **local terminal / command prompt**:

```bash
scp -i key.pem index.html ubuntu@<EC2_PUBLIC_IP>:/home/ubuntu/
scp -i key.pem style.css ubuntu@<EC2_PUBLIC_IP>:/home/ubuntu/
scp -i key.pem script.js ubuntu@<EC2_PUBLIC_IP>:/home/ubuntu/
```

---

### 📂 Move Files to Nginx Directory

Now connect to your EC2 instance and move files:

```bash
sudo mv /home/ubuntu/index.html /var/www/html/
sudo mv /home/ubuntu/style.css /var/www/html/
sudo mv /home/ubuntu/script.js /var/www/html/
```

---

### 🔐 Set Proper Permissions

```bash
sudo chmod -R 755 /var/www/html
```

---

### 🔄 Restart Nginx

```bash
sudo systemctl restart nginx
```

---

### ✅ Verify

Open browser:

```
http://<EC2_PUBLIC_IP>
```

---

## 💡 Pro Tip

* Use SCP when working with **multiple files or full project folders**
* For entire folder upload:

```bash
scp -i key.pem -r my-website-folder ubuntu@<EC2_PUBLIC_IP>:/home/ubuntu/
```

---
### 🔹 Verify

Open browser:

```
http://<EC2_PUBLIC_IP>
```

✅ Website should load

---

# 🎯 Step 3: Create Target Group

1. Go to **EC2 → Target Groups**
2. Click **Create Target Group**

### Configuration:

* Target Type: Instance
* Protocol: HTTP
* Port: 80

### Health Check:

* Path: `/`
* Protocol: HTTP

### Register Target:

* Select your EC2 instance
* Click **Include as pending below**
* Click **Create**

---

# ⚖️ Step 4: Create Application Load Balancer

1. Go to **EC2 → Load Balancers**
2. Click **Create Load Balancer**
3. Choose **Application Load Balancer**

---

### 🔹 Configuration

* Scheme: Internet-facing
* IP Type: IPv4

### 🔹 Network Mapping

* Select at least **2 Availability Zones**

---

### 🔹 Listener

* HTTP (Port 80)

### 🔹 Forward to:

* Select your Target Group

Click **Create Load Balancer**

---

# 🔐 Step 5: Request SSL Certificate (ACM)

1. Go to **AWS Certificate Manager**
2. Click **Request Certificate**

### Add Domains:

```
ayushk.online
www.ayushk.online
```

### Validation Method:

* DNS Validation ✅

---

### 🔹 Add CNAME Records in Route 53

* Go to Hosted Zone
* Add provided CNAME records

⏳ Wait for validation (2–10 mins)

---

# 🔒 Step 6: Attach SSL to Load Balancer

1. Open Load Balancer
2. Go to **Listeners**
3. Add **HTTPS (443)** listener

---

### 🔹 Configure

* Attach ACM Certificate
* Forward to Target Group

---

### 🔁 Redirect HTTP → HTTPS

Edit HTTP listener:

* Add Rule → Redirect to HTTPS (443)

---

# 🌍 Step 7: Configure Route 53 Hosted Zone

1. Go to Route 53 → Hosted Zones
2. Click **Create Hosted Zone**

### Enter:

```
Domain Name: ayushk.online
```

---

### 🔹 Copy Nameservers

Example:

```
ns-123.awsdns-45.com
ns-678.awsdns-90.net
```

---

# 🌐 Step 8: Update Nameservers in Hostinger

1. Login to Hostinger
2. Go to **Domain → DNS Settings**
3. Replace nameservers with Route 53 nameservers

⏳ DNS propagation: 5–30 minutes

---

# 🔗 Step 9: Connect Domain to Load Balancer

1. Go to Route 53 Hosted Zone
2. Click **Create Record**

---

### 🔹 Record Configuration

* Type: A
* Enable: Alias = Yes
* Target: Load Balancer DNS

---

# ✅ Step 10: Testing & Verification

### 🔹 Test Domain

```
http://ayushk.online
https://ayushk.online
```

---

### ✔️ Check:

* Website loads ✅
* HTTPS lock appears 🔒
* Redirect HTTP → HTTPS works
* Load Balancer DNS works

---

# 🛠️ Troubleshooting

### ❌ 502 Bad Gateway

* Check Nginx running:

```bash
sudo systemctl status nginx
```

---

### ❌ Target Unhealthy

* Check security group allows port 80
* Verify health check path `/`

---

### ❌ SSL Pending

* Ensure CNAME records are correct
* Wait for DNS propagation

---

### ❌ Domain Not Resolving

* Verify nameservers updated in Hostinger
* Use:

```bash
nslookup ayushk.online
```

---

# 💡 Tips

* Always use **2 AZs** for high availability
* Keep security groups minimal but correct
* Use **HTTPS only** in production
* Monitor using CloudWatch (optional)

---

# 🎉 Conclusion

You have successfully:

✅ Hosted a website using Nginx
✅ Configured Load Balancer & Target Group
✅ Secured with SSL using ACM
✅ Connected domain via Route 53
✅ Enabled HTTPS access

---

## 📌 Your Final URL

```
https://ayushk.online
```

---
🔥 Your production-ready AWS deployment is complete!
