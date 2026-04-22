# 🌐 Hosting a Static Website using Nginx on EC2 and Configuring Custom Domain with AWS Route 53

## 📌 Project Overview

This project demonstrates how to:

* Host a modern static website on an AWS EC2 instance using Nginx
* Connect a custom domain (**ayushk.online**) purchased from Hostinger
* Configure DNS using AWS Route 53
* Access your website globally using a domain name

---

## 🎯 Objective

* Deploy a static website on EC2
* Configure domain mapping using Route 53
* Make the website publicly accessible via:
  👉 **[http://ayushk.online](http://ayushk.online)**

---

## 🧱 Prerequisites

* AWS Account
* Domain purchased from Hostinger (`ayushk.online`)
* Basic knowledge of EC2 & SSH
* Key Pair (.pem file)

---

## 🧭 Architecture Overview

```
User Browser → Domain → Route 53 → Elastic IP → EC2 → Nginx → Website
```

---

## 🚀 Step 1: Launch EC2 Instance

* AMI: Ubuntu 22.04
* Instance Type: t2.micro (Free Tier)

### 🔐 Security Group Rules

* SSH (22)
* HTTP (80)
* HTTPS (443)

### Connect to Instance

```bash
ssh -i your-key.pem ubuntu@your-ec2-public-ip
```

---

## ⚙️ Step 2: Install Nginx

```bash
sudo apt update -y
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

## 🌐 Step 3: Deploy Static Website (Advanced UI)

### Remove Default Files

```bash
sudo rm -rf /var/www/html/*
cd /var/www/html
```

---

### 📄 Create index.html

```bash
sudo nano index.html
```

Paste:

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ayush Store</title>
<link rel="stylesheet" href="style.css">
</head>
<body>

<header>
<div class="logo">🛒 Ayush Store</div>
<nav>
<a href="#">Home</a>
<a href="#">Products</a>
<a href="#">Cart</a>
</nav>
</header>

<section class="hero">
<h1>Big Deals Are Live 🚀</h1>
<p>Upgrade your lifestyle with trending products</p>
<button onclick="showAlert()">Shop Now</button>
</section>

<section class="products">
<div class="card">
<img src="https://via.placeholder.com/200">
<h3>Headphones</h3>
<p>₹999</p>
<button onclick="addToCart('Headphones')">Add to Cart</button>
</div>

<div class="card">
<img src="https://via.placeholder.com/200">
<h3>Smart Watch</h3>
<p>₹1999</p>
<button onclick="addToCart('Smart Watch')">Add to Cart</button>
</div>

<div class="card">
<img src="https://via.placeholder.com/200">
<h3>Shoes</h3>
<p>₹1499</p>
<button onclick="addToCart('Shoes')">Add to Cart</button>
</div>
</section>

<footer>
<p>© 2026 Ayush Store | Hosted on AWS</p>
</footer>

<script src="script.js"></script>
</body>
</html>
```

---

### 🎨 Create style.css

```bash
sudo nano style.css
```

```css
body {
margin: 0;
font-family: Arial;
background: #f5f5f5;
}

header {
display: flex;
justify-content: space-between;
padding: 15px 30px;
background: #131921;
color: white;
}

nav a {
color: white;
margin-left: 15px;
text-decoration: none;
}

.hero {
text-align: center;
padding: 60px;
background: linear-gradient(to right, #ff9900, #ff6600);
color: white;
}

.products {
display: flex;
justify-content: center;
flex-wrap: wrap;
}

.card {
background: white;
margin: 15px;
padding: 15px;
width: 220px;
text-align: center;
border-radius: 10px;
transition: 0.3s;
}

.card:hover {
transform: translateY(-10px);
box-shadow: 0 4px 15px rgba(0,0,0,0.2);
}
```

---

### ⚙️ Create script.js

```bash
sudo nano script.js
```

```javascript
function showAlert() {
alert("Welcome to Ayush Store 🚀");
}

function addToCart(product) {
alert(product + " added to cart!");
}
```

---

### Restart Nginx

```bash
sudo systemctl restart nginx
```

---

## 🌍 Step 4: Configure Elastic IP

* Go to EC2 → Elastic IPs
* Allocate Elastic IP
* Associate with your EC2 instance

---

## 🌐 Step 5: Setup Route 53

* Create Hosted Zone: `ayushk.online`
* Copy NS records

---

## 🔄 Step 6: Update Hostinger DNS

* Replace nameservers with Route 53 NS

⏳ Wait for propagation (5 min – 24 hrs)

---

## 🎯 Step 7: Create A Record

* Type: A
* Value: Elastic IP

---

## 🧪 Step 8: Test Website

Open in browser:

```
http://ayushk.online
```

---

## 🔐 Optional: Enable HTTPS

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d ayushk.online
```

---

## 🛠️ Troubleshooting

| Issue              | Fix              |
| ------------------ | ---------------- |
| Site not loading   | Check port 80    |
| Domain not working | Wait for DNS     |
| Timeout            | Check Elastic IP |
| Wrong page         | Restart Nginx    |

---

## ✅ Best Practices

* Use Elastic IP
* Enable HTTPS
* Keep security groups minimal
* Regularly update server

---

## 🎉 Conclusion

You have successfully:

* Hosted a website on EC2
* Configured Nginx
* Connected custom domain via Route 53
* Made your site live globally 🌍

---
