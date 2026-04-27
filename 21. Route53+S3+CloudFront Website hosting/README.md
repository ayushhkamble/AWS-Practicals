# 🌐 Hosting a Static Website using S3 + CloudFront + Route 53 (Custom Domain: ayushk.online)

---

## 📌 Project Overview

This project demonstrates how to host a **secure, scalable, and globally distributed static website** using:

* Amazon S3 – Storage (private)
* Amazon CloudFront – CDN + HTTPS
* Amazon Route 53 – DNS routing
* Hostinger – Domain provider

---

## 🎯 Objective

Deploy a static website and make it accessible via:

👉 **[https://ayushk.online](https://ayushk.online)**
👉 With **HTTPS, high availability, and fast global delivery**

---

## 🏗️ Architecture Diagram

```
User (Browser)
      │
      ▼
Route 53 (DNS Resolution)
      │
      ▼
CloudFront (CDN + SSL)
      │
      ▼
S3 Bucket (Private Content)
```

---

## 📖 Introduction

### 🔹 What is Static Website Hosting?

Serving static files (HTML, CSS, JS) directly without backend processing.

---

### 🔹 Why This Architecture?

| Service    | Role                            |
| ---------- | ------------------------------- |
| S3         | Stores website files            |
| CloudFront | Improves speed + provides HTTPS |
| Route 53   | Connects domain                 |
| ACM        | Provides SSL certificate        |

---

## 🔒 Architecture Decision (IMPORTANT)

This setup uses:

✅ Private S3 bucket
✅ CloudFront OAC (Origin Access Control)
❌ No public access
❌ No S3 static website endpoint

👉 Only CloudFront can access S3

---

# ⚙️ Step 1: Create S3 Bucket

### 🔹 1.1 Create Bucket

Go to:
**AWS Console → S3 → Create Bucket**

* Bucket Name: `ayushk.online`
* Region: Any (e.g., eu-north-1)

📸 ![Create Bucket](image-link)

---

### 🔹 1.2 Block Public Access

✔ KEEP **“Block all public access” ENABLED**

---

### 🔹 1.3 Upload Website Files (Console Method)

Go to:
**S3 → Bucket → Objects → Upload**

Upload:

* `index.html`
* `error.html` (optional)

Click **Upload**

📸 ![Upload Files](image-link)

---

### 🔹 1.4 Verify Files

✔ File must be:

```
index.html
```

---

# 🌍 Step 2: Configure CloudFront

### 🔹 2.1 Create Distribution

Go to:
**CloudFront → Create Distribution**

---

### 🔹 2.2 Origin Configuration

* Origin Domain → Select S3 bucket
* It should look like:

```
ayushk.online.s3.amazonaws.com
```

❌ DO NOT use:

```
s3-website endpoint
```

---

### 🔹 2.3 Create OAC

* Click **Create Origin Access Control**
* Settings:

  * Signing behavior → **Sign requests**
  * Signing protocol → **SigV4**

---

### 🔹 2.4 Attach OAC

* Attach to origin
* Choose:

✔ **Update bucket policy automatically**

---

### 🔹 2.5 Distribution Settings

* Viewer Protocol Policy → Redirect HTTP to HTTPS
* Default Root Object → `index.html`

---

### 🔹 2.6 Create Distribution

⏳ Wait 5–10 minutes

📸 ![CloudFront Setup](image-link)

---

# 🔐 Step 3: Request SSL Certificate (ACM)

Go to:
**AWS Certificate Manager**

---

### 🔹 IMPORTANT

✔ Region MUST be:

```
us-east-1 (N. Virginia)
```

---

### 🔹 Request Certificate

Add domains:

```
ayushk.online
www.ayushk.online
```

---

### 🔹 DNS Validation

* Choose DNS validation
* Click “Create records in Route 53”

📸 ![ACM Setup](image-link)

---

# 🌐 Step 4: Configure Route 53

### 🔹 4.1 Create Hosted Zone

* Domain: `ayushk.online`

---

### 🔹 4.2 Create Records

### Root Domain

* Type: A
* Alias → Yes
* Target → CloudFront

---

### WWW Domain

* Name: `www`
* Type: A (Alias → CloudFront)

📸 ![Route53 Setup](image-link)

---

# 🔗 Step 5: Connect Hostinger Domain

### 🔹 5.1 Copy Nameservers

From Route 53:

```
ns-xxxx.awsdns-xx.com
ns-xxxx.awsdns-xx.net
ns-xxxx.awsdns-xx.org
ns-xxxx.awsdns-xx.co.uk
```

---

### 🔹 5.2 Update in Hostinger

Go to:

* Domains → ayushk.online
* Nameservers → Replace with AWS NS

📸 ![Hostinger Setup](image-link)

---

### 🔹 Wait for Propagation

⏳ 5 min – 24 hrs

---

# 🔄 Step 6: Attach Domain & SSL to CloudFront

Go to:
CloudFront → Distribution → Edit

---

### 🔹 Add Alternate Domain Names

```
ayushk.online
www.ayushk.online
```

---

### 🔹 Attach SSL Certificate

Select ACM certificate

---

### 🔹 Save Changes

---

# 🔐 Step 7: Manual Bucket Policy (If Needed)

Go to:
S3 → Permissions → Bucket Policy

Paste:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowCloudFrontServicePrincipal",
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudfront.amazonaws.com"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::ayushk.online/*",
      "Condition": {
        "StringEquals": {
          "AWS:SourceArn": "arn:aws:cloudfront::YOUR_ACCOUNT_ID:distribution/YOUR_DISTRIBUTION_ID"
        }
      }
    }
  ]
}
```

---

# 🧪 Step 8: Testing

### 🔹 Test URLs

CloudFront:

```
https://dxxxxx.cloudfront.net
```

Custom Domain:

```
https://ayushk.online
```

---

# ❗ Troubleshooting

### 🔴 403 Access Denied

* Check OAC attached
* Check bucket policy
* Ensure correct origin

---

### 🔴 SSL Not Working

* ACM must be in us-east-1
* Certificate must be validated

---

### 🔴 DNS Issue

* Check nameservers in Hostinger
* Wait for propagation

---

### 🔴 Old Content

Fix:

* CloudFront → Invalidations → `/*`

---

# 💰 Cost Optimization

### Free Tier

* S3 → 5GB
* CloudFront → limited usage
* Route 53 → minimal cost

---

### Tips

* Delete unused distributions
* Use small files
* Enable caching

---

# 📌 Conclusion

Successfully deployed a secure website using:

* Amazon S3
* Amazon CloudFront
* Amazon Route 53
* Hostinger

---

# 🚀 Final Output

✅ [https://ayushk.online](https://ayushk.online)
✅ HTTPS Enabled 🔒
✅ Fast Global Delivery 🌍
✅ Highly Available

---
