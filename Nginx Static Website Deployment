
# 🚀 AWS Practical – Nginx Deployment for Static Website Hosting (Ubuntu EC2)


# 📌 Project Title

**Deploying a Static Website on AWS EC2 using Nginx**


# 🎯 Objective

To deploy a simple static website using **Nginx Web Server** on an Ubuntu EC2 instance and access it via the Public IP address.


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
Static Website Files (/var/www/html)
```


# 🛠 Services Used

* Amazon Web Services
* Amazon EC2
* Nginx
* Ubuntu 22.04 LTS


# 📝 Pre-requisites

* AWS Account
* EC2 Key Pair (.pem file)
* Security Group Rules:

  * SSH (22) → My IP
  * HTTP (80) → 0.0.0.0/0

> ⚠️ Note: We are NOT using UFW firewall. AWS Security Group handles traffic control.


# 🚀 Step-by-Step Implementation


## ✅ Step 1: Launch EC2 Instance

1. Login to AWS Console.
2. Go to EC2 → Launch Instance.
3. Configure:

   * Name: `Nginx-Static-Website`
   * AMI: Ubuntu 22.04 LTS
   * Instance Type: `t2.micro`
   * Key Pair: Select existing key (.pem)
   * Security Group:

     * Allow SSH (22)
     * Allow HTTP (80)

Launch the instance.


## ✅ Step 2: Connect to EC2 via SSH

```bash
ssh -i ayush.pem ubuntu@<Public-IP>
```


## ✅ Step 3: Update System Packages

```bash
sudo apt update -y
sudo apt upgrade -y
```

## ✅ Step 4: Install Nginx

```bash
sudo apt install nginx -y
```

Check status:

```bash
sudo systemctl status nginx
```

Enable Nginx at boot:

```bash
sudo systemctl enable nginx
```


## ✅ Step 5: Verify Nginx Installation

Open browser:

```
http://<Public-IP>
```

You should see the **default Nginx welcome page**.

---

# 🌐 Deploy Static Website


## ✅ Step 6: Navigate to Web Root Directory

```bash
cd /var/www/html
```

Default root directory:

```
/var/www/html
```

## ✅ Step 7: Remove Default Page

```bash
sudo rm index.nginx-debian.html
```


## ✅ Step 8: Create Simple Static Website

```bash
sudo nano index.html
```

Paste:

html
<!DOCTYPE html>
<html>
<head>
    <title>My AWS Nginx Website</title>
</head>
<body>
    <h1>Welcome to My Static Website 🚀</h1>
    <p>Hosted on AWS EC2 using Nginx</p>
</body>
</html>


Save and exit.

---

## ✅ Step 9: Set Proper Permissions

```bash
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```


## ✅ Step 10: Restart Nginx

```bash
sudo systemctl restart nginx
```


## ✅ Step 11: Access Website

Open browser:

```
http://<Public-IP>
```

You should see:

> Welcome to My Static Website 🚀


# 📁 Important Nginx Configuration Files

Main Config:

```
/etc/nginx/nginx.conf
```

Default Site Config:

```
/etc/nginx/sites-available/default
```

Test Configuration:

```bash
sudo nginx -t
```

Reload Configuration:

```bash
sudo systemctl reload nginx
```

# 🔐 Security Best Practices

* Restrict SSH to **My IP only**
* Do not allow 0.0.0.0/0 for SSH
* Keep system updated
* Use HTTPS in production (Certbot + SSL)


# 🎓 Interview Questions

### Q1: Where are website files stored in Nginx?

/var/www/html

### Q2: How do you check if Nginx is running?

sudo systemctl status nginx

### Q3: Difference between restart and reload?

* Restart → Stops and starts service
* Reload → Applies config changes without downtime


# 🏁 Final Output

✔ EC2 Instance Running
✔ Nginx Installed
✔ Static Website Hosted
✔ Accessible via Public IP

