# 🚀 AWS Practical Guide: Advanced Static Website Hosting using Amazon S3

---

## 📌 Project Title
**Advanced Static Website Hosting using Amazon S3 (Prism Flux Template)**

---

## 🎯 Objective

Design and deploy a modern, interactive static website on Amazon S3 using an advanced UI template.

- Upload HTML, CSS, and JavaScript files  
- Configure S3 for static website hosting  
- Enable public access securely  
- Host a fully functional modern UI website  

---

## 📖 Introduction

### 🔹 What is Amazon S3?

Amazon S3 (Simple Storage Service) is an object storage service that allows you to store and retrieve files from anywhere on the internet.

---

### 🌐 What is Static Website Hosting?

Static website hosting means hosting websites that use:

- HTML  
- CSS  
- JavaScript  

❌ No backend server required

---

### ✅ Benefits

| Feature | Description |
|--------|------------|
| 💰 Low Cost | Pay only for storage |
| ⚡ High Performance | Fast loading |
| 🌍 Highly Available | Multi-region support |
| 📈 Scalable | Handles unlimited users |

---

## 🏗️ Architecture Overview

```

🌐 User Browser
|
↓
┌────────────────────────┐
│   Amazon S3 Bucket     │
│  (Static Website Host) │
└────────────────────────┘
|
├── index.html
├── CSS
├── JS
└── Images

```

## ⚙️ Step-by-Step Implementation

---

### 🧱 Step 1: Create S3 Bucket

1. Open AWS Console → S3
2. Click **Create Bucket**

#### Configuration:

* Bucket Name: `my-prism-website-12345`
* Region: Nearest region

---

### ⚠️ Disable Public Access Block

Uncheck:

* ✅ Block all public access

---

### 📤 Step 2: Upload Files

1. Open bucket
2. Click **Upload**
3. Upload:

* index.html
* templatemo-prism-flux.css
* templatemo-prism-scripts.js
* images folder

---

### 🌐 Step 3: Enable Static Website Hosting

1. Go to **Properties**
2. Scroll to **Static Website Hosting**
3. Click **Edit**

#### Settings:

* Enable: ✅
* Index document: `index.html`

---

### 📌 Copy Website Endpoint

Example:

```

http://my-prism-website-12345.s3-website-us-east-1.amazonaws.com

```

---

## 🔐 Step 4: Bucket Policy

Go to **Permissions → Bucket Policy**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadAccess",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-prism-website-12345/*"
    }
  ]
}
```

---

## 🔓 Step 5: Allow Public Access

1. Go to **Permissions**
2. Click **Block Public Access → Edit**
3. Disable all options

---

## 🌐 Step 6: Test Website

Open your S3 endpoint.

🎉 You should see:

* Animated UI
* 3D carousel
* Interactive sections

---

## 📊 Verification Checklist

* ✅ Website loads
* ✅ CSS applied
* ✅ JavaScript working
* ✅ Images visible

---

## 🧹 Cleanup

* Delete objects
* Delete bucket

---

## 📌 Best Practices

* Use CloudFront (CDN)
* Add custom domain (Route 53)
* Enable versioning
* Never allow public write

---

## 🎯 Conclusion

You successfully:

* Hosted an advanced UI website on S3
* Configured public access
* Enabled static hosting
* Deployed a real-world modern frontend

---

## 🙌 Final Note

This project demonstrates how powerful Amazon S3 can be for hosting **modern frontend applications without servers**.

🔥 Perfect for:

* DevOps portfolios
* Cloud practicals
* Frontend deployment

---
