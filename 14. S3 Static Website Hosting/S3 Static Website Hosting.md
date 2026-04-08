# 🚀 AWS Practical Guide: Static Website Hosting using Amazon S3

---

## 📌 Project Title
**Static Website Hosting using Amazon S3 with Public Access Configuration**

---

## 🎯 Objective

Design and implement a static website hosted on Amazon S3 where:

- Website files (HTML, CSS, JS) are uploaded to an S3 bucket  
- The bucket is configured for static website hosting  
- Proper Bucket Policy is applied to allow public access  
- Users can access the website via S3 website endpoint  

---

## 📖 Introduction

### 🔹 What is Amazon S3?

**Amazon S3 (Simple Storage Service)** is an object storage service provided by AWS that allows you to store and retrieve data from anywhere on the internet.

---

### 🌐 What is Static Website Hosting?

Static website hosting means hosting websites that contain only **HTML, CSS, and JavaScript** (no backend server required).

---

### ✅ Benefits of S3 Static Hosting

| Feature | Description |
|--------|------------|
| 💰 Low Cost | Pay only for storage used |
| ⚡ High Performance | Fast content delivery |
| 🌍 High Availability | Data replicated across regions |
| 📈 Scalability | Handles unlimited traffic |

---

## 🏗️ Architecture Overview

### 🔧 Components

- **S3 Bucket** → Stores website files  
- **HTML/CSS Files** → Frontend website content  
- **Bucket Policy** → Allows public access  
- **Public Access Settings** → Controls security  

---

### 🧭 Architecture Diagram (ASCII)

```

```
    🌐 User Browser
          |
          ↓
 ┌───────────────────┐
 │   S3 Bucket       │
 │ (Static Website)  │
 └───────────────────┘
          |
  📁 HTML / CSS / JS
          |
  🔐 Bucket Policy (Public Read)
```

````

---

## 📁 Website Files

Create the following files on your local machine:

---

### 📄 index.html

```html
<!DOCTYPE html>
<html>
<head>
    <title>My S3 Website</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>🚀 Welcome to My S3 Hosted Website</h1>
    <p>This website is hosted on Amazon S3.</p>
</body>
</html>
````

---

### ❌ error.html

```html
<!DOCTYPE html>
<html>
<head>
    <title>Error</title>
</head>
<body>
    <h1>❌ Page Not Found</h1>
    <p>The page you are looking for does not exist.</p>
</body>
</html>
```

---

### 🎨 styles.css

```css
body {
    background-color: #f4f4f4;
    text-align: center;
    font-family: Arial;
}

h1 {
    color: #2c3e50;
}

p {
    color: #555;
}
```

---

## ⚙️ Step-by-Step Implementation

---

### 🧱 Step 1: Create S3 Bucket

1. Go to **AWS Console → S3**
2. Click **Create Bucket**

#### 📝 Configuration:

* **Bucket Name**: `my-static-website-12345` (must be unique)
* **Region**: Choose nearest region

---

### ⚠️ Important Setting:

* ❌ Uncheck **"Block all public access"**

👉 This is required because:

* By default, S3 blocks public access
* We need public access to host a website

---

### 📤 Step 2: Upload Website Files

1. Open your bucket
2. Click **Upload**
3. Upload:

   * `index.html`
   * `error.html`
   * `styles.css`

---

### 📁 Folder Structure

```
Bucket Root/
├── index.html
├── error.html
└── styles.css
```

---

### 🌐 Step 3: Enable Static Website Hosting

1. Go to **Properties tab**
2. Scroll to **Static Website Hosting**
3. Click **Edit**

---

### ⚙️ Configuration:

* Enable: ✅
* **Index document**: `index.html`
* **Error document**: `error.html`

---

### 📌 Copy Website Endpoint

Example:

```
http://my-static-website-12345.s3-website-us-east-1.amazonaws.com
```

---

## 🔐 Step 4: Configure Bucket Policy (VERY IMPORTANT)

Go to **Permissions → Bucket Policy** and paste:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadAccess",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-static-website-12345/*"
    }
  ]
}
```

---

### 🧠 Explanation:

| Field                    | Meaning                          |
| ------------------------ | -------------------------------- |
| "Effect": "Allow"        | Grants permission                |
| "Principal": "*"         | Allows everyone (public)         |
| "Action": "s3:GetObject" | Allows read access               |
| "Resource"               | Applies to all objects in bucket |

---

## 🔓 Step 5: Adjust Public Access Settings

1. Go to **Permissions → Block Public Access**
2. Click **Edit**
3. Disable all options

---

### ⚠️ Security Note:

* Only enable public access for **static websites**
* ❌ Never allow public write access

---

## 🌐 Step 6: Test Website

1. Open the **S3 Website Endpoint**
2. You should see your website 🎉

---

## 📊 Verification

### ✅ Check:

* Website loads in browser
* CSS styling is applied
* Error page works

👉 Test error page:

```
http://your-url/abc.html
```

---

## 🧹 Cleanup Steps

To avoid charges:

1. Delete all objects from bucket
2. Delete the bucket

---

## 📌 Best Practices

### 💡 Recommendations:

* 🌍 Use **CloudFront** for faster delivery (CDN)
* 🌐 Use **custom domain** via Route 53
* 🔐 Avoid public write access
* 🔁 Enable **versioning**

---

## 🎯 Conclusion

In this practical, you successfully:

* Created an S3 bucket
* Uploaded static website files
* Enabled static website hosting
* Configured bucket policy for public access
* Tested your live website

🎉 Your static website is now live on AWS S3!

---

## 📁 Suggested GitHub Structure

```
AWS-Practicals/
└── 006-S3-Static-Website/
    ├── README.md
    ├── index.html
    ├── error.html
    └── styles.css
```

---

## 🙌 Final Note

Amazon S3 static hosting is one of the **simplest and most cost-effective** ways to deploy websites without managing servers.

---
