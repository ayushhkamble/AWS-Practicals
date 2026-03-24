# 🚀 AWS Practical – Nginx Deployment for Advanced Static Website Hosting (Ubuntu EC2)

---

# 📌 Project Title

**Deploying an Advanced Static Website on AWS EC2 using Nginx**

---

# 🎯 Objective

To deploy a **modern, interactive static website** using **Nginx Web Server** on an Ubuntu EC2 instance and access it via the Public IP address.

---

# 🏗 Architecture Overview

```
User (Browser)
      ↓
Internet
      ↓
AWS Security Group (Allow Port 80)
      ↓
EC2 Instance (Ubuntu 22.04)
      ↓
Nginx Web Server
      ↓
Advanced Static Website (/var/www/html)
```

---

# 🛠 Services Used

* Amazon Web Services (AWS)
* Amazon EC2
* Nginx Web Server
* Ubuntu 22.04 LTS

---

# 📝 Pre-requisites

* AWS Account
* EC2 Key Pair (.pem file)
* Security Group Rules:

  * SSH (22) → My IP
  * HTTP (80) → 0.0.0.0/0

> ⚠️ Note: AWS Security Group handles firewall rules (UFW not required)

---

# 🚀 Step-by-Step Implementation

---

## ✅ Step 1: Launch EC2 Instance

1. Login to AWS Console
2. Navigate to EC2 → Launch Instance
3. Configure:

   * Name: `Nginx-Advanced-Website`
   * AMI: Ubuntu 22.04 LTS
   * Instance Type: `t3.micro`
   * Key Pair: Select existing key
   * Security Group:

     * Allow SSH (22)
     * Allow HTTP (80)

Launch the instance.

---

## ✅ Step 2: Connect via SSH

```bash
ssh -i ayush.pem ubuntu@<Public-IP>
```

---

## ✅ Step 3: Update System

```bash
sudo apt update -y
sudo apt upgrade -y
```

---

## ✅ Step 4: Install Nginx

```bash
sudo apt install nginx -y
```

Check status:

```bash
sudo systemctl status nginx
```

Enable at boot:

```bash
sudo systemctl enable nginx
```

---

## ✅ Step 5: Verify Installation

Open in browser:

```
http://<Public-IP>
```

You should see the default Nginx page.

---

# 🌐 Deploy Advanced Website

---

## ✅ Step 6: Navigate to Web Directory

```bash
cd /var/www/html
```

---

## ✅ Step 7: Remove Default Files

```bash
sudo rm -rf *
```

---

## ✅ Step 8: Create Website Files

Create HTML:

```bash
sudo vim index.html
```

Paste:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Advanced Demo Website</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

<header>
    <nav class="navbar">
        <h1 class="logo">MyDemo</h1>
        <ul class="nav-links">
            <li><a href="#home">Home</a></li>
            <li><a href="#features">Features</a></li>
            <li><a href="#about">About</a></li>
        </ul>
        <button id="themeToggle">🌙</button>
    </nav>
</header>

<section id="home" class="hero">
    <h2 id="typing"></h2>
    <p>Hosted on AWS EC2 using Nginx 🚀</p>
    <button onclick="openModal()">Explore</button>
</section>

<section id="features" class="features">
    <h2>Features</h2>
    <div class="card-container">
        <div class="card" onclick="showAlert('Fast Performance ⚡')">
            <h3>Speed</h3>
        </div>
        <div class="card" onclick="showAlert('Secure 🔐')">
            <h3>Security</h3>
        </div>
        <div class="card" onclick="showAlert('Responsive 📱')">
            <h3>Responsive</h3>
        </div>
    </div>
</section>

<div id="modal" class="modal">
    <div class="modal-content">
        <span onclick="closeModal()" class="close">&times;</span>
        <h2>Welcome 🎉</h2>
    </div>
</div>

<footer>
    <p>© 2026 AWS Nginx Project</p>
</footer>

<script src="script.js"></script>
</body>
</html>
```

---

## ✅ Step 9: Create CSS File

```bash
sudo vim style.css
```

Paste:

```css
body {
    margin: 0;
    font-family: Arial;
}

.navbar {
    display: flex;
    justify-content: space-between;
    padding: 15px;
    background: #111;
    color: white;
}

.hero {
    height: 90vh;
    display: flex;
    justify-content: center;
    align-items: center;
    background: linear-gradient(135deg, #667eea, #764ba2);
    color: white;
}

.card {
    padding: 20px;
    margin: 10px;
    background: #f4f4f4;
    cursor: pointer;
}
```

---

## ✅ Step 10: Create JavaScript File

```bash
sudo vim script.js
```

Paste:

```javascript
let text = "Welcome to My Advanced Website 🚀";
let i = 0;

function typing() {
    if (i < text.length) {
        document.getElementById("typing").innerHTML += text.charAt(i);
        i++;
        setTimeout(typing, 50);
    }
}
typing();

function openModal() {
    document.getElementById("modal").style.display = "block";
}

function closeModal() {
    document.getElementById("modal").style.display = "none";
}

function showAlert(msg) {
    alert(msg);
}
```

---

## ✅ Step 11: Set Permissions

```bash
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

---

## ✅ Step 12: Restart Nginx

```bash
sudo systemctl restart nginx
```

---

## ✅ Step 13: Access Website

Open:

```
http://<Public-IP>
```

🎉 You will see your **Advanced Interactive Website**

---

# 📁 Important Nginx Configuration

Main Config:

```
/etc/nginx/nginx.conf
```

Default Site:

```
/etc/nginx/sites-available/default
```

Test Config:

```bash
sudo nginx -t
```

Reload:

```bash
sudo systemctl reload nginx
```

---

# 🔐 Security Best Practices

* Restrict SSH to **My IP only**
* Avoid `0.0.0.0/0` for SSH
* Keep system updated
* Use HTTPS (SSL with Certbot in production)

---

# 🏁 Final Output

✔ EC2 Instance Running
✔ Nginx Installed
✔ Advanced Static Website Deployed
✔ Interactive UI Working
✔ Accessible via Public IP

---
