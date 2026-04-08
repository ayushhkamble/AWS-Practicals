# 🚀 AWS Practical Guide: Auto Scaling Group (ASG) with Dynamic Scaling & CloudWatch Monitoring

---

## 📌 Project Title
**Auto Scaling Group (ASG) Setup with Dynamic Scaling using CloudWatch Monitoring**

---

## 🎯 Objective

Design and implement an AWS architecture where:

- EC2 instances scale automatically using Auto Scaling Group (ASG)
- CPU utilization is monitored using Amazon CloudWatch
- Dynamic scaling is configured using Target Tracking Policy
- CloudWatch Alarm triggers scaling actions

---

## 📖 Introduction

### 🔹 What is Auto Scaling Group (ASG)?

An **Auto Scaling Group (ASG)** is a service in AWS that automatically manages and scales EC2 instances based on demand.

It ensures:
- The required number of instances are always running
- Automatic scaling (scale-in & scale-out)
- Replacement of unhealthy instances

---

### ✅ Benefits of ASG

| Feature | Description |
|--------|------------|
| 🟢 High Availability | Runs across multiple Availability Zones |
| 🔁 Fault Tolerance | Automatically replaces failed instances |
| 💰 Cost Optimization | Scales down when demand is low |
| ⚡ Performance | Scales up during high traffic |

---

## 🏗️ Architecture Overview

### 🔧 Components Used

- **Launch Template** → Blueprint for EC2 instances
- **Auto Scaling Group (ASG)** → Manages instances
- **CloudWatch** → Monitors CPU utilization
- **Scaling Policy** → Controls scaling behavior

---

### 🧭 Architecture Diagram (ASCII)

```

```
            🌐 Internet
                 |
         ┌───────────────┐
         │ Load Balancer │ (Optional)
         └──────┬────────┘
                |
    ┌───────────┼───────────┐
    |                       |
```

🖥️ EC2 Instance        🖥️ EC2 Instance
|                       |
└──── Auto Scaling Group ────┘
|
📊 CloudWatch
|
⚙️ Scaling Policies

````

---

## ⚙️ Step-by-Step Implementation

---

### 🧱 Step 1: Create Launch Template

1. Go to **EC2 Dashboard → Launch Templates**
2. Click **Create Launch Template**

#### 📝 Configuration:

- **Name**: `ASG-Template`
- **AMI**: Ubuntu Server 22.04
- **Instance Type**: `t2.micro`
- **Key Pair**: Select existing key
- **Security Group**:
  - Allow **SSH (22)**
  - Allow **HTTP (80)**

---

### 📜 User Data Script (Important)

```bash
#!/bin/bash
apt update -y
apt install nginx -y
systemctl start nginx
systemctl enable nginx

echo "<h1>Instance: $(hostname)</h1>" > /var/www/html/index.html
````

👉 This installs Nginx and shows instance hostname.

---

### 🚀 Step 2: Create Auto Scaling Group (ASG)

1. Go to **EC2 → Auto Scaling Groups**
2. Click **Create ASG**

#### 🔧 Configuration:

* **ASG Name**: `My-ASG`
* **Launch Template**: Select `ASG-Template`

#### 🌐 Network:

* Choose your **VPC**
* Select **2+ subnets** (multi-AZ recommended)

#### 📊 Group Size:

* **Desired Capacity**: 2
* **Minimum Capacity**: 1
* **Maximum Capacity**: 4

---

### ⚖️ (Optional but Recommended) Attach Load Balancer

* Select **Application Load Balancer (ALB)**
* Route traffic to instances

---

### 📈 Step 3: Configure Target Tracking Scaling Policy

1. In ASG → Go to **Automatic Scaling**
2. Click **Add Policy**

#### 🔧 Configuration:

* **Policy Type**: Target Tracking
* **Metric Type**: Average CPU Utilization
* **Target Value**: `50%`

---

### 🤖 How it Works

* If CPU > 50% → Scale OUT (add instances)
* If CPU < 50% → Scale IN (remove instances)

---

## 📊 Step 4: Enable CloudWatch Monitoring

### 🔍 What CloudWatch Provides

* CPU Utilization
* Network In/Out
* Disk Usage
* Instance Health

---

### 📈 View CPU Graph

1. Go to **CloudWatch → Metrics**
2. Select:

   * EC2 → Per Instance Metrics
   * Click **CPUUtilization**

---

## 🚨 Step 5: Create CloudWatch Alarm

1. Go to **CloudWatch → Alarms**
2. Click **Create Alarm**

---

### ⚙️ Configuration:

* **Metric**: CPU Utilization
* **Threshold**: `> 70%`
* **Evaluation Period**: 2

---

### 🔔 Alarm Actions:

* Trigger **Auto Scaling Action**
* Choose your ASG

---

### 🚦 Alarm States

| State               | Meaning            |
| ------------------- | ------------------ |
| 🟢 OK               | Everything normal  |
| 🔴 ALARM            | Threshold exceeded |
| ⚪ INSUFFICIENT_DATA | Not enough data    |

---

## 🧪 Step 6: Test Auto Scaling

### 🔥 Generate Load

SSH into EC2 and run:

```bash
sudo apt update
sudo apt install stress -y
stress --cpu 2 --timeout 300
```

---

### 👀 Observe:

* CPU usage increases
* CloudWatch Alarm triggers
* ASG launches new instance(s)

---

## 📊 Verification

### ✅ Check Scaling

* Go to **EC2 → Instances**
* New instances should appear

---

### 📈 Check CloudWatch

* CPU graph spikes
* Alarm changes state

---

### 📜 Check ASG Activity

* Go to **ASG → Activity**
* See scaling events

---

## 🧹 Cleanup Steps

To avoid charges:

1. Delete **Auto Scaling Group**
2. Delete **Launch Template**
3. Delete **CloudWatch Alarms**
4. Terminate remaining EC2 instances

---

## 📌 Best Practices

### 💡 Recommendations:

* ✅ Use **Multiple Availability Zones**
* ✅ Attach **Application Load Balancer**
* ✅ Enable **Health Checks**
* ✅ Use **Target Tracking (recommended)**
* ✅ Set proper min/max limits

---

### 💰 Cost Optimization Tips

* Use **t2.micro / t3.micro (Free Tier)**
* Set **minimum instances = 1**
* Avoid over-scaling

---

## 🎯 Conclusion

In this practical, you successfully:

* Created a Launch Template
* Configured an Auto Scaling Group
* Enabled CloudWatch Monitoring
* Implemented Target Tracking Scaling Policy
* Created CloudWatch Alarm
* Tested dynamic scaling using CPU load

🎉 Your AWS infrastructure is now **scalable, resilient, and cost-efficient!**

---
