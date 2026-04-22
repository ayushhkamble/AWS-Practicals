# 🌐 Hosting a Static Website using Nginx on EC2 and Configuring Custom Domain with AWS Route 53

## 📌 Project Overview

This project demonstrates how to:
- Host a static website on an AWS EC2 instance using Nginx
- Connect a custom domain (**ayushk.online**) purchased from Hostinger
- Configure DNS using AWS Route 53
- Access your website via a domain name instead of an IP address

---

## 🎯 Objective

- Deploy a static website on EC2
- Configure domain mapping using Route 53
- Make the website publicly accessible via `http://ayushk.online`

---

## 🧱 Prerequisites

- AWS Account
- Domain purchased from Hostinger (`ayushk.online`)
- Basic knowledge of:
  - EC2
  - SSH
- Key Pair (.pem file)

---

## 🧭 Architecture Overview

```

User Browser → Domain (ayushk.online) → Route 53 → EC2 Elastic IP → Nginx → Website

````

---

## 🚀 Step 1: Launch EC2 Instance

### 1. Create Instance
- Go to AWS EC2 Dashboard
- Click **Launch Instance**
- Choose:
  - AMI: Ubuntu Server 22.04
  - Instance Type: t2.micro (Free Tier)

### 2. Configure Security Group
Allow:
- SSH (22)
- HTTP (80)
- HTTPS (443)

### 3. Connect via SSH

```bash
ssh -i your-key.pem ubuntu@your-ec2-public-ip
````

---

## ⚙️ Step 2: Install and Configure Nginx

```bash
sudo apt update -y
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

### Verify Installation

Open browser:

```
http://your-ec2-public-ip
```

---

## 🌐 Step 3: Deploy Static Website

### Remove Default Page

```bash
sudo rm /var/www/html/index.nginx-debian.html
```

### Create New Website

```bash
sudo nano /var/www/html/index.html
```

### 📄 Demo Website Code (Amazon-style Clone)

```html
<!DOCTYPE html>
<html>
<head>
    <title>Ayush Store</title>
    <style>
        body {
            font-family: Arial;
            margin: 0;
            background: #f3f3f3;
        }
        header {
            background: #232f3e;
            color: white;
            padding: 15px;
            text-align: center;
        }
        nav {
            background: #37475a;
            padding: 10px;
            text-align: center;
        }
        nav a {
            color: white;
            margin: 10px;
            text-decoration: none;
        }
        .container {
            padding: 20px;
        }
        .product {
            background: white;
            padding: 15px;
            margin: 10px;
            display: inline-block;
            width: 200px;
            border-radius: 5px;
        }
        .product img {
            width: 100%;
        }
        footer {
            background: #232f3e;
            color: white;
            text-align: center;
            padding: 10px;
        }
    </style>
</head>
<body>

<header>
    <h1>🛒 Ayush Online Store</h1>
</header>

<nav>
    <a href="#">Home</a>
    <a href="#">Products</a>
    <a href="#">Deals</a>
    <a href="#">Contact</a>
</nav>

<div class="container">
    <div class="product">
        <img src="https://via.placeholder.com/200">
        <h3>Product 1</h3>
        <p>₹999</p>
    </div>

    <div class="product">
        <img src="https://via.placeholder.com/200">
        <h3>Product 2</h3>
        <p>₹1499</p>
    </div>

    <div class="product">
        <img src="https://via.placeholder.com/200">
        <h3>Product 3</h3>
        <p>₹799</p>
    </div>
</div>

<footer>
    <p>© 2026 Ayush Store | Built on AWS</p>
</footer>

</body>
</html>
```

### Save & Exit:

```
CTRL + X → Y → ENTER
```

---

## 🌍 Step 4: Configure Elastic IP

1. Go to EC2 → Elastic IPs
2. Click **Allocate Elastic IP**
3. Click **Associate Elastic IP**
4. Select your EC2 instance

---

## 🌐 Step 5: Setup AWS Route 53

1. Open Route 53 Dashboard
2. Click **Hosted Zones**
3. Click **Create Hosted Zone**

### Enter:

* Domain Name: `ayushk.online`
* Type: Public Hosted Zone

### Note:

You will see **NS Records** like:

```
ns-123.awsdns-45.com
ns-456.awsdns-78.net
```

---

## 🔄 Step 6: Update Hostinger DNS

1. Login to Hostinger
2. Go to Domain → DNS Settings
3. Replace existing **Nameservers** with Route 53 nameservers

### Example:

```
ns-123.awsdns-45.com
ns-456.awsdns-78.net
```

⏳ **DNS Propagation Time:** 5 minutes to 24 hours

---

## 🎯 Step 7: Create A Record in Route 53

1. Go to Hosted Zone → ayushk.online
2. Click **Create Record**

### Configure:

* Record Type: A
* Value: Your Elastic IP
* TTL: Default

---

## 🧪 Step 8: Testing

Open browser:

```
http://ayushk.online
```

---

## 🛠️ Troubleshooting

| Issue              | Solution                            |
| ------------------ | ----------------------------------- |
| Site not loading   | Check Security Group (port 80 open) |
| Domain not working | Wait for DNS propagation            |
| Wrong page         | Restart nginx                       |
| Timeout            | Verify Elastic IP attached          |

```bash
sudo systemctl restart nginx
```

---

## 🔐 Optional: Enable HTTPS (SSL)

Install Certbot:

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d ayushk.online
```

Follow prompts to enable HTTPS.

---

## ✅ Best Practices

* Always use Elastic IP (avoid dynamic IP issues)
* Enable HTTPS for security
* Use proper DNS TTL values
* Regularly update your server

---

## 🎉 Conclusion

You have successfully:

* Hosted a static website using Nginx on EC2
* Connected a custom domain using Route 53
* Made your website accessible globally

---
