# 🚀 AWS Practical Guide: Custom Metrics & Log Monitoring using CloudWatch Agent

---

## 📌 Project Title

**Custom Metrics and Nginx Log Monitoring using Amazon CloudWatch Agent**

---

## 🎯 Objective

Design and implement a monitoring solution where:

* Memory utilization is collected as a custom metric
* Nginx logs are monitored in real time
* Data is sent to **Amazon CloudWatch**
* Logs and metrics are centralized for analysis

---

## 🏗️ Architecture Overview

* **Amazon EC2** → Hosts Nginx server
* **Nginx** → Generates access & error logs
* **CloudWatch Agent** → Collects metrics & logs
* **Amazon CloudWatch** → Stores and visualizes data

---

## ⚙️ Prerequisites

* AWS Account
* Ubuntu EC2 Instance
* IAM Role with `CloudWatchAgentServerPolicy`
* Basic knowledge of Linux commands

---

## 🪜 Step 1: Launch EC2 Instance

1. Go to AWS Console
2. Launch **Ubuntu EC2 instance**
3. Configure Security Group:

   * Allow **SSH (22)**
   * Allow **HTTP (80)**

👉 **In Advanced Details → User Data, paste the following script:**

```bash
#!/bin/bash

apt update -y
apt install nginx -y

systemctl start nginx
systemctl enable nginx

echo "<h1>CloudWatch Custom Metrics Monitoring</h1>" > /var/www/html/index.html
echo "<p>Nginx server is running successfully.</p>" >> /var/www/html/index.html
echo "<p>Custom metrics will be visible in AWS CloudWatch.</p>" >> /var/www/html/index.html
```

---

## 🌐 Step 2: Verify Nginx Setup

After instance launches:

* Copy **Public IP**
* Open in browser

✅ You should see:

```
CloudWatch Custom Metrics Monitoring
Nginx server is running successfully.
```

---

## 🔐 Step 3: Attach IAM Role

Attach an IAM Role to EC2 with:

* `CloudWatchAgentServerPolicy`

---

## 📦 Step 4: Install CloudWatch Agent

SSH into EC2 and run:

```bash
sudo apt update -y
sudo apt install -y curl unzip

curl -O https://s3.amazonaws.com/amazoncloudwatch-agent/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb

sudo dpkg -i -E ./amazon-cloudwatch-agent.deb
```

---

## 📝 Step 5: Create Configuration File

```bash
sudo vim /opt/aws/amazon-cloudwatch-agent/bin/cwagent-config.json
```

---

## 📌 Step 6: Add Configuration

Paste:

```json
{
  "metrics": {
    "namespace": "Custom/EC2",
    "metrics_collected": {
      "mem": {
        "measurement": [
          "mem_used_percent"
        ],
        "metrics_collection_interval": 60
      }
    }
  },
  "logs": {
    "logs_collected": {
      "files": {
        "collect_list": [
          {
            "file_path": "/var/log/nginx/access.log",
            "log_group_name": "nginx-access-log",
            "log_stream_name": "{instance_id}"
          },
          {
            "file_path": "/var/log/nginx/error.log",
            "log_group_name": "nginx-error-log",
            "log_stream_name": "{instance_id}"
          }
        ]
      }
    }
  }
}
```

---

## ▶️ Step 7: Start CloudWatch Agent

```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
-a fetch-config \
-m ec2 \
-c file:/opt/aws/amazon-cloudwatch-agent/bin/cwagent-config.json \
-s
```

---

## 🔍 Step 8: Verify Metrics

Go to:

```
CloudWatch → Metrics → All Metrics → Custom Namespaces
```

Open:

```
Custom/EC2
```

✅ Metric visible:

* `mem_used_percent`

---

## 📜 Step 9: Verify Logs

Go to:

```
CloudWatch → Log Groups
```

Check:

* `nginx-access-log`
* `nginx-error-log`

✅ Open log stream → View real-time logs

---

## 🧪 Step 10: Generate Logs

```bash
curl http://localhost
```

---

## 🧪 Step 11: Troubleshooting

### Check Agent Status

```bash
sudo systemctl status amazon-cloudwatch-agent
```

### Restart Agent

```bash
sudo systemctl restart amazon-cloudwatch-agent
```

### View Logs

```bash
cat /opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log
```

---

## ❗ Common Issues

| Issue               | Fix                     |
| ------------------- | ----------------------- |
| Metrics not visible | Wait 2–3 minutes        |
| Logs not appearing  | Ensure Nginx is running |
| Permission error    | Check IAM role          |
| Wrong log path      | Verify file path        |

---

## 🎯 Outcome

✅ Nginx hosted successfully
✅ Custom memory metric created
✅ Logs monitored in real time
✅ Centralized monitoring using CloudWatch

---

## 🚀 Future Enhancements

* Add CPU & Disk metrics
* Create CloudWatch Dashboard
* Setup CloudWatch Alarms
* Integrate SNS notifications

---

## 🏁 Conclusion

This project demonstrates how to build a **complete monitoring system in AWS** by combining:

* Compute (**EC2**)
* Web Server (**Nginx**)
* Monitoring (**CloudWatch Agent**)

Resulting in **real-time visibility of system performance and logs**.

---
