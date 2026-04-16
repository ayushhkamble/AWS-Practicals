# 🚀 AWS Practical Guide: Custom Metrics & Log Monitoring using CloudWatch Agent

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
* IAM Role attached (CloudWatch permissions)
* Nginx installed on EC2

---

## 🪜 Step 1: Launch EC2 Instance

1. Go to AWS Console
2. Launch Ubuntu EC2 instance
3. Allow SSH (Port 22)

---

## 🌐 Step 2: Install Nginx (Log Source)

```bash
sudo apt update -y
sudo apt install nginx -y

sudo systemctl start nginx
sudo systemctl enable nginx
```

👉 This generates logs at:

* `/var/log/nginx/access.log`
* `/var/log/nginx/error.log`

---

## 🔐 Step 3: Attach IAM Role

Attach IAM Role with:

* `CloudWatchAgentServerPolicy`
  *(or equivalent custom permissions)*

---

## 📦 Step 4: Install CloudWatch Agent

```bash
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

## 📌 Step 6: Add Your Custom Configuration

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

## 🧠 Configuration Explanation (IMPORTANT for Viva)

### 🔹 Metrics Section

* **namespace** → `Custom/EC2` (your custom metric group)
* **mem_used_percent** → Tracks memory usage percentage
* **interval = 60 sec** → Data sent every minute

---

### 🔹 Logs Section

* **access.log** → Tracks user requests
* **error.log** → Tracks server errors
* **log_group_name** → Created in CloudWatch
* **log_stream_name** → Uses EC2 instance ID dynamically

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

1. Go to **CloudWatch Console**
2. Navigate:

   ```
   Metrics → All Metrics → Custom Namespaces
   ```
3. Open:

   ```
   Custom/EC2
   ```

✅ You should see:

* `mem_used_percent`

---

## 📜 Step 9: Verify Logs

1. Go to **CloudWatch → Log Groups**
2. Check:

* `nginx-access-log`
* `nginx-error-log`

3. Open log stream (instance ID)

✅ You should see real-time logs

---

## 🧪 Step 10: Generate Test Logs

Run:

```bash
curl http://localhost
```

👉 This generates entries in access log

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

### Check Logs

```bash
cat /opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log
```

---

## ❗ Common Issues

| Issue              | Fix                     |
| ------------------ | ----------------------- |
| Logs not appearing | Ensure Nginx is running |
| Metrics missing    | Wait 2–3 minutes        |
| Permission error   | Check IAM role          |
| Wrong log path     | Verify file paths       |

---

## 🎯 Outcome

✅ Custom memory metric created
✅ Nginx logs monitored in real time
✅ Centralized monitoring using CloudWatch

---

## 📌 Future Enhancements

* Create **CloudWatch Alarms** (e.g., high memory usage)
* Integrate with SNS for email alerts
* Build dashboards
* Add CPU, Disk metrics

---

## 🏁 Conclusion

This project demonstrates how to extend AWS monitoring by combining **custom metrics and log tracking**, providing deep visibility into system performance and application behavior.

---
