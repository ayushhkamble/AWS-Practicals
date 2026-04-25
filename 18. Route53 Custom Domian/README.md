# 🌐 Hosting a Static Website on AWS EC2 using Nginx and Configuring Custom Domain with AWS Route 53

---

## 📌 Introduction

This project demonstrates how to host a static website on an AWS EC2 instance using **Nginx** and connect it to a custom domain (**ayushk.online**) purchased from **Hostinger** using **AWS Route 53**.

### 🏗️ Architecture Overview

```

User Browser
↓
Domain (ayushk.online)
↓
Route 53 (DNS)
↓
Elastic IP
↓
EC2 Instance (Ubuntu)
↓
Nginx Web Server
↓
Static Website Files (HTML, CSS, JS)

````

---

## 🎯 Objective

- Host a static website using Nginx on EC2
- Configure a custom domain using Route 53
- Make the website publicly accessible via your domain

---

## 📚 Prerequisites

- ✅ AWS Account  
- ✅ Domain purchased from Hostinger (ayushk.online)  
- ✅ Basic Linux command knowledge  
- ✅ SSH client (Git Bash / Terminal)

---

## 🚀 Step 1: Launch EC2 Instance

1. Go to **AWS Console → EC2 → Launch Instance**
2. Configure:
   - **AMI**: Ubuntu Server 22.04
   - **Instance Type**: t2.micro (Free Tier)
   - **Key Pair**: Create or select existing
   - **Security Group**:
     ```
     SSH (22)     → My IP
     HTTP (80)    → Anywhere (0.0.0.0/0)
     HTTPS (443)  → Anywhere (0.0.0.0/0)
     ```

3. Launch the instance

### 🔐 Connect to EC2 via SSH

```bash
ssh -i your-key.pem ubuntu@your-ec2-public-ip
````

---

## ⚙️ Step 2: Install and Configure Nginx

### 📦 Install Nginx

```bash
sudo apt update -y
sudo apt install nginx -y
```

### ▶️ Start and Enable Nginx

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

### ✅ Verify Installation

Open browser:

```
http://your-ec2-public-ip
```

You should see the **Nginx default page**

---

## 📁 Step 3: Deploy Static Website

### 📤 Upload Website Files

Use SCP or WinSCP:

```bash
scp -i your-key.pem -r ./website/* ubuntu@your-ec2-public-ip:/home/ubuntu/
```

### 📂 Move Files to Nginx Directory

```bash
sudo rm -rf /var/www/html/*
sudo cp -r /home/ubuntu/* /var/www/html/
```

### 🔐 Set Permissions

```bash
sudo chmod -R 755 /var/www/html
```

### 🔄 Restart Nginx

```bash
sudo systemctl restart nginx
```

Now refresh:

```
http://your-ec2-public-ip
```

🎉 Your website should be live!

---

## 🌍 Step 4: Configure Elastic IP

### 📌 Why Elastic IP?

EC2 public IP changes on restart. Elastic IP gives a **static IP**.

### 🛠️ Steps:

1. Go to **EC2 → Elastic IPs**
2. Click **Allocate Elastic IP**
3. Click **Associate Elastic IP**
4. Attach to your EC2 instance

✅ Save this IP (e.g., `13.xx.xx.xx`)

---

## 🌐 Step 5: Configure AWS Route 53

1. Go to **Route 53 → Hosted Zones**
2. Click **Create Hosted Zone**

   * Domain Name: `ayushk.online`
   * Type: Public Hosted Zone

### 📄 Records Created:

* **NS (Name Server)**
* **SOA (Start of Authority)**

Copy the **NS records**

---

## 🔁 Step 6: Update Nameservers in Hostinger

1. Login to **Hostinger**
2. Go to **Domain → DNS / Nameservers**
3. Replace default nameservers with Route 53 NS records

### Example:

```
ns-123.awsdns-45.com
ns-678.awsdns-12.net
ns-910.awsdns-34.org
ns-111.awsdns-56.co.uk
```

### ⏳ DNS Propagation

* Takes **5 mins to 24 hours**

---

## 🧾 Step 7: Create DNS Records in Route 53

### ➤ Create A Record

| Type | Name | Value      |
| ---- | ---- | ---------- |
| A    | @    | Elastic IP |

### ➤ Optional: WWW Subdomain

| Type | Name | Value      |
| ---- | ---- | ---------- |
| A    | www  | Elastic IP |

---

## 🧪 Step 8: Testing

Open in browser:

```
http://ayushk.online
```

or

```
http://www.ayushk.online
```

---

## ⚠️ Troubleshooting

| Issue                | Solution                            |
| -------------------- | ----------------------------------- |
| ❌ Site not loading   | Check Security Group (Port 80 open) |
| ❌ Domain not working | Wait for DNS propagation            |
| ❌ Wrong site         | Check Nginx directory files         |
| ❌ IP mismatch        | Verify Elastic IP                   |
| ❌ Permission denied  | Fix file permissions                |

---

## 🏗️ Project Architecture Diagram

```
[ User ]
   ↓
[ Domain: ayushk.online ]
   ↓
[ Route 53 DNS ]
   ↓
[ Elastic IP ]
   ↓
[ EC2 Ubuntu Instance ]
   ↓
[ Nginx Server ]
   ↓
[ Static Website Files ]
```

---

## 📘 Key Learnings

* ✅ EC2 instance setup and configuration
* ✅ Nginx web server deployment
* ✅ Static website hosting
* ✅ Elastic IP usage
* ✅ DNS management with Route 53
* ✅ Domain linking from Hostinger

---

## 🎁 Bonus: How to Improve This Project

### 🔐 Enable HTTPS (SSL)

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx
```

---

### 🚀 CI/CD Pipeline

* Use GitHub Actions to auto-deploy changes

---

### ☁️ Alternative Hosting

* Use **S3 + CloudFront** for serverless hosting

---

### 🔒 Security Enhancements

* Configure firewall (UFW)
* Enable fail2ban

---

## 🏁 Conclusion

You have successfully:

* Hosted a static website on AWS EC2 using Nginx
* Connected a custom domain using Route 53
* Made your website publicly accessible
