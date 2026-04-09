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
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AWS S3 Static Website</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>

    <!-- Navbar -->
    <header>
        <nav class="navbar">
            <h2 class="logo">☁️ MyCloudSite</h2>
            <ul class="nav-links">
                <li><a href="#">Home</a></li>
                <li><a href="#about">About</a></li>
                <li><a href="#services">Services</a></li>
            </ul>
        </nav>
    </header>

    <!-- Hero Section -->
    <section class="hero">
        <h1>🚀 Deployed on AWS S3</h1>
        <p>Fast • Scalable • Serverless Hosting</p>
        <button onclick="showMessage()">Click Me</button>
    </section>

    <!-- About Section -->
    <section id="about" class="section">
        <h2>About This Project</h2>
        <p>This is a static website hosted on Amazon S3 using public access and bucket policy.</p>
    </section>

    <!-- Services Section -->
    <section id="services" class="section">
        <h2>Services</h2>
        <div class="cards">
            <div class="card">⚡ Fast Hosting</div>
            <div class="card">🔒 Secure Storage</div>
            <div class="card">🌍 Global Access</div>
        </div>
    </section>

    <!-- Footer -->
    <footer>
        <p>© 2026 AWS S3 Practical | Created for DevOps Learning</p>
    </footer>

    <script src="script.js"></script>
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
<body style="text-align:center; font-family:Arial; padding:50px;">
    <h1>404 - Page Not Found ❌</h1>
    <p>The page you are looking for does not exist.</p>
    <a href="index.html">Go Back Home</a>
</body>
</html>
```

---

### 🎨 styles.css

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
}

.navbar {
    display: flex;
    justify-content: space-between;
    background: #2c3e50;
    padding: 15px;
    color: white;
}

.nav-links {
    list-style: none;
    display: flex;
    gap: 15px;
}

.nav-links a {
    color: white;
    text-decoration: none;
}

.hero {
    height: 80vh;
    background: linear-gradient(to right, #3498db, #6dd5fa);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    color: white;
}

.hero button {
    margin-top: 10px;
    padding: 10px;
    border: none;
    background: #2c3e50;
    color: white;
    cursor: pointer;
}

.section {
    padding: 40px;
    text-align: center;
}

.cards {
    display: flex;
    justify-content: center;
    gap: 15px;
}

.card {
    background: #ecf0f1;
    padding: 20px;
    border-radius: 10px;
}

footer {
    background: #2c3e50;
    color: white;
    text-align: center;
    padding: 10px;
}
```

---

### 🎨 script.js

```js
function showMessage() {
    alert("🎉 Your website is live on AWS S3!");
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
