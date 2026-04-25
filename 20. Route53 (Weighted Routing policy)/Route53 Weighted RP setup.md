# 🌐 AWS Route 53 Practical: Implementing Weighted Routing Policy with EC2 Instances and Health Checks (with Hostinger Domain Setup)

---

## 📌 Project Overview

This project demonstrates how to build a **highly available web architecture** using:

- AWS EC2 (Ubuntu Instances)
- Nginx Web Server
- Route 53 Hosted Zone
- Weighted Routing Policy
- Health Checks
- Domain Integration via Hostinger

---

## 🎯 Objective

- Launch **two EC2 instances**
- Deploy **Nginx servers** with unique messages
- Configure **Route 53 Weighted Routing**
- Add **Health Checks**
- Connect domain from **Hostinger to Route 53**
- Distribute traffic intelligently

---

## 🧱 Architecture Diagram

```

```
            User Request
                  |
                  ▼
        Route 53 (DNS Server)
    Weighted Routing + Health Check
        /                     \
       /                       \
      ▼                         ▼
```

EC2 Instance 1            EC2 Instance 2
(Server 1)                (Server 2)
|                         |
Nginx                    Nginx

````

---

## ⚙️ Prerequisites

- AWS Account
- Domain purchased from Hostinger
- Basic EC2 knowledge

---

## 🪜 Step 1: Launch EC2 Instances

### 🔹 Configuration

- AMI: Ubuntu
- Instance Type: t2.micro
- Security Group:
  - SSH (22) → Your IP
  - HTTP (80) → Anywhere

---

### 🔹 User Data Script (Instance 1)

```bash
#!/bin/bash
apt update -y
apt install nginx -y
systemctl start nginx
systemctl enable nginx
echo "<h1>This is Server 1</h1>" > /var/www/html/index.html
````

---

### 🔹 User Data Script (Instance 2)

```bash
#!/bin/bash
apt update -y
apt install nginx -y
systemctl start nginx
systemctl enable nginx
echo "<h1>This is Server 2</h1>" > /var/www/html/index.html
```

---

## 🌐 Step 2: Verify Web Servers

Open in browser:

* `http://<EC2-IP-1>`
* `http://<EC2-IP-2>`

✅ Output:

* Server 1 and Server 2 pages load correctly

---

## 🔗 Step 3: Allocate Elastic IPs

1. EC2 → Elastic IPs → Allocate
2. Associate:

   * EIP 1 → Instance 1
   * EIP 2 → Instance 2

📌 Prevents IP change after restart

---

## 🌍 Step 4: Create Hosted Zone in Route 53

1. Go to Route 53 → Hosted Zones
2. Click **Create Hosted Zone**
3. Enter domain (e.g., `yourdomain.com`)
4. Type: Public Hosted Zone

---

## 🌐 Step 5: Update Name Servers in Hostinger (VERY IMPORTANT)

After creating Hosted Zone in Route 53:

### 🔹 Copy Name Servers from AWS

You will see 4 NS records like:

```
ns-123.awsdns-45.com
ns-678.awsdns-90.net
ns-111.awsdns-22.org
ns-222.awsdns-33.co.uk
```

---

### 🔹 Login to Hostinger

1. Go to **Hostinger Dashboard**
2. Click **Domains**
3. Select your domain
4. Go to **DNS / Nameservers**

---

### 🔹 Replace Nameservers

* Choose **Use custom nameservers**
* Remove existing Hostinger nameservers
* Add AWS nameservers

Example:

```
ns-123.awsdns-45.com
ns-678.awsdns-90.net
ns-111.awsdns-22.org
ns-222.awsdns-33.co.uk
```

---

### 🔹 Save Changes

⏳ DNS Propagation Time:

* Usually: 5–30 minutes
* Max: 24–48 hours

---

📌 IMPORTANT:
Without this step, Route 53 will NOT control your domain.

---

## ⚖️ Step 6: Create Weighted Routing Records

### Record 1 (Server 1)

* Name: `www.yourdomain.com`
* Type: A
* Value: Elastic IP 1
* Routing Policy: Weighted
* Weight: 50
* TTL: 60

---

### Record 2 (Server 2)

* Name: `www.yourdomain.com`
* Type: A
* Value: Elastic IP 2
* Routing Policy: Weighted
* Weight: 50
* TTL: 60

---

📌 Both records must have SAME NAME

---

## ❤️ Step 7: Configure Health Checks

### Create Health Check

* Protocol: HTTP
* Endpoint: EC2 Elastic IP
* Path: `/`
* Interval: 30 sec
* Failure Threshold: 3

---

### Attach to Records

* Edit each A record
* Enable **Associate Health Check**
* Select correct health check

---

## 🧪 Step 8: Testing

### 🔁 Check Traffic Distribution

```bash
curl http://www.yourdomain.com
```

Run multiple times → alternating responses

---

### ⚖️ Change Weights

* 80 → Server 1
* 20 → Server 2

Observe traffic shift

---

### ❌ Failover Test

1. Stop one EC2 instance
2. Wait for health check failure

✅ Traffic shifts to healthy instance

---

## 📊 Weighted Routing Explained

* DNS distributes traffic based on weights
* Example:

  * 50/50 → equal traffic
  * 80/20 → biased routing

---

## 🚀 Benefits

* No load balancer cost
* Easy traffic control
* Failover support
* A/B testing capability

---

## 🌍 Real-World Use Cases

* Blue-Green Deployment
* Canary Releases
* Multi-version testing

---

## 🛠️ Troubleshooting

### ❌ Website Not Opening

* Check:

  * EC2 running
  * Security group (port 80)

---

### ❌ Domain Not Working

* Check:

  * Nameservers updated in Hostinger
  * Wait for DNS propagation

---

### ❌ Health Check Failed

* Ensure:

  * HTTP 200 response
  * Nginx running

```bash
sudo systemctl restart nginx
```

---

## 📸 Screenshot Placeholders

```markdown
![EC2 Setup](images/ec2.png)
![Elastic IP](images/eip.png)
![Route53 Hosted Zone](images/hostedzone.png)
![Nameserver Setup Hostinger](images/hostinger-ns.png)
![Weighted Records](images/records.png)
![Health Check](images/health.png)
```

---

## ✅ Expected Output

* Domain resolves successfully
* Traffic distributed across servers
* Automatic failover working

---

## 🎉 Conclusion

You have successfully built:

* A domain-connected AWS setup
* Weighted routing using Route 53
* Health check-based failover
* Nginx hosted multi-server architecture

This setup is **simple, scalable, and industry-relevant** 🚀

---
