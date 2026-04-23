# 🌐 Hosting a Static Website on AWS EC2 using Nginx and Configuring Custom Domain with AWS Route 53

---

## 📌 Introduction

This project demonstrates how to host a static website on an AWS EC2 instance using **Nginx** and connect it to a custom domain (**ayushk.online**) purchased from **Hostinger** using **AWS Route 53**.

---

## 🏗️ Architecture Overview

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
Static Website Files (Downloaded using wget)

````

---

## 🎯 Objective

- Host a static website using Nginx on EC2  
- Download website files directly on server using `wget`  
- Configure custom domain using Route 53  

---

## 📚 Prerequisites

- ✅ AWS Account  
- ✅ Domain purchased from Hostinger (ayushk.online)  
- ✅ Basic Linux knowledge  
- ✅ Public URL of your HTML template (ZIP file or GitHub repo)

---

## 🚀 Step 1: Launch EC2 Instance

1. Go to **AWS Console → EC2 → Launch Instance**
2. Configure:
   - **AMI**: Ubuntu Server 22.04
   - **Instance Type**: t2.micro
   - **Security Group**:
     ```
     SSH (22)     → My IP
     HTTP (80)    → Anywhere
     HTTPS (443)  → Anywhere
     ```

3. Connect via SSH:

```bash
ssh -i your-key.pem ubuntu@your-ec2-public-ip
````

---

## ⚙️ Step 2: Install and Configure Nginx

```bash id="cmd1"
sudo apt update -y
sudo apt install nginx wget unzip -y
```

```bash id="cmd2"
sudo systemctl start nginx
sudo systemctl enable nginx
```

✅ Verify:

```
http://your-ec2-public-ip
```

---

## 📥 Step 3: Download & Deploy Website using wget

### 🔽 Download Template

Replace the link with your actual template URL:

```bash id="cmd3"
wget https://example.com/template.zip
```

### 📦 Unzip Files

```bash id="cmd4"
unzip template.zip
```

### 📂 Move Files to Nginx Directory

```bash id="cmd5"
sudo rm -rf /var/www/html/*
sudo cp -r template/* /var/www/html/
```

> ⚠️ Make sure `template/` is your extracted folder name

### 🔐 Set Permissions

```bash id="cmd6"
sudo chmod -R 755 /var/www/html
```

### 🔄 Restart Nginx

```bash id="cmd7"
sudo systemctl restart nginx
```

🎉 Open browser:

```
http://your-ec2-public-ip
```

---

## 🌍 Step 4: Configure Elastic IP

1. Go to **EC2 → Elastic IPs**
2. Allocate new IP
3. Associate with your EC2 instance

📌 Why? Prevents IP change after restart

---

## 🌐 Step 5: Configure Route 53

1. Open **Route 53 → Hosted Zones**

2. Create Hosted Zone:

   * Domain: `ayushk.online`

3. Copy NS records

---

## 🔁 Step 6: Update Nameservers in Hostinger

Replace with Route 53 nameservers:

```
ns-xxx.awsdns-xx.com
ns-xxx.awsdns-xx.net
ns-xxx.awsdns-xx.org
ns-xxx.awsdns-xx.co.uk
```

⏳ Wait for DNS propagation

---

## 🧾 Step 7: Create DNS Records

### A Record

| Type | Name | Value      |
| ---- | ---- | ---------- |
| A    | @    | Elastic IP |

### WWW Record

| Type | Name | Value      |
| ---- | ---- | ---------- |
| A    | www  | Elastic IP |

---

## 🧪 Step 8: Testing

Open:

```
http://ayushk.online
```

---

## ⚠️ Troubleshooting

| Problem            | Fix                  |
| ------------------ | -------------------- |
| Site not opening   | Check Security Group |
| Domain not working | Wait DNS propagation |
| Wrong page         | Check file path      |
| wget failed        | Verify URL           |

---

## 🏗️ Architecture Diagram

```
[User]
  ↓
[Domain: ayushk.online]
  ↓
[Route 53]
  ↓
[Elastic IP]
  ↓
[EC2 + Nginx]
  ↓
[Website Files (wget)]
```

---

## 📘 Key Learnings

* EC2 setup & SSH
* Nginx configuration
* wget file download in Linux
* Route 53 DNS setup
* Domain linking

---

## 🎁 Bonus Improvements

### 🔐 Enable HTTPS

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx
```

### 🚀 CI/CD

* GitHub Actions auto deployment

### ☁️ Alternative

* S3 + CloudFront hosting

---

## 🏁 Conclusion

You successfully:

* Hosted a static website using Nginx
* Downloaded files using wget
* Connected domain via Route 53
