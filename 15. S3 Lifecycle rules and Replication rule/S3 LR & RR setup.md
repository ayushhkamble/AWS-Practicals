# 🚀 AWS Practical Guide: S3 Lifecycle Management & Same-Region Replication (SRR)

---

## 📌 Project Title
**Amazon S3 Lifecycle Management and Same-Region Replication (SRR) Implementation**

---

## 🎯 Objective

Design and implement an AWS S3 setup where:

- Objects automatically transition between storage classes using Lifecycle Rules  
- Old objects are expired automatically  
- Data is replicated from a source bucket to a destination bucket (Same Region)  
- Versioning is enabled to support replication  

---

## 📖 Introduction

### 🔹 What is S3 Lifecycle Management?

S3 Lifecycle Management automates:

- Moving objects to cheaper storage classes  
- Deleting unused or old data  

👉 Helps reduce storage cost.

---

### 🔹 What is S3 Replication?

S3 Replication automatically copies objects from one bucket to another.

---

### 🔄 Types of Replication

| Type | Description |
|------|------------|
| 🔁 SRR | Same region replication |
| 🌍 CRR | Cross-region replication |

---

### 🔍 Difference

| Feature | Lifecycle | Replication |
|--------|----------|-------------|
| Purpose | Cost optimization | Data duplication |
| Action | Move/Delete | Copy |
| Use Case | Archival | Backup/Sync |

---

### ✅ Benefits

- 💰 Cost Optimization  
- 🔒 High Durability  
- ⚡ Fast Synchronization  

---

## 🏗️ Architecture Overview

### 🔧 Components

- Source Bucket  
- Destination Bucket  
- Lifecycle Rules  
- Replication Rule (SRR)  
- IAM Role (Auto-created)  

---

### 🧭 Architecture Diagram

```

```
   👤 User Uploads File
           |
           ↓
┌────────────────────┐
│   Source Bucket    │
└────────────────────┘
     |         \
     |          \
```

📦 Lifecycle        🔁 SRR Replication
(30d → IA)           |
(60d → Glacier)      ↓
(90d → Delete)  ┌────────────────────┐
│ Destination Bucket │
│   (Same Region)    │
└────────────────────┘

```

---

# 🔹 Part A: Lifecycle Management

---

## 🧱 Step 1: Create S3 Bucket

1. Go to **AWS Console → S3**
2. Click **Create Bucket**

### 📝 Configuration:

- Bucket Name: `lifecycle-demo-bucket-12345`
- Region: `ap-south-1`

---

## 📤 Step 2: Upload Sample Files

Upload:

- `.txt` files  
- `.jpg`, `.png` images  

---

## ⚙️ Step 3: Create Lifecycle Rule

1. Go to **Bucket → Management**
2. Click **Create Lifecycle Rule**

---

### 🔧 Configuration:

- Rule Name: `Lifecycle-Rule-1`
- Scope: Apply to all objects  

---

### 📦 Actions:

- Transition to **Standard-IA** after 30 days  
- Transition to **Glacier** after 60 days  
- Expire objects after 90 days  

---

## 📦 Storage Classes

| Storage Class | Use Case | Cost | Access |
|--------------|--------|------|--------|
| Standard | Frequent access | High | Instant |
| Standard-IA | Infrequent | Medium | Instant |
| Glacier | Archive | Very Low | Slow |

---

# 🔹 Part B: Same-Region Replication (SRR)

---

## 🏗️ What is SRR?

Same-Region Replication copies objects between S3 buckets **within the same region**.

---

## 🌍 Step 4: Create Destination Bucket

- Bucket Name: `replication-destination-bucket-12345`
- Region: Same as source bucket (`ap-south-1`)

---

## 🔄 Step 5: Enable Versioning

Enable versioning on both buckets:

- Go to **Properties → Versioning → Enable**

---

## ❗ Why Versioning?

- Required for replication  
- Maintains object versions  
- Prevents overwrite  

---

## 🔐 Step 6: Create Replication Rule (Auto IAM Role)

1. Go to **Source Bucket → Management**
2. Click **Replication → Create Rule**

---

### 🔧 Configuration:

- Rule Name: `SRR-Rule`  
- Status: Enabled  
- Source: Entire bucket  
- Destination Bucket: Select destination bucket  
- IAM Role: ✅ Select **"Create new role"**  

---

### 🤖 What AWS Does:

- Automatically creates IAM Role  
- Assigns required permissions  
- Sets trust relationship  

---

### 📌 Additional Options:

- Replicate existing objects (optional)  
- Replicate delete markers (optional)  
- Replicate metadata and tags  

---

## 🧪 Step 7: Test Replication

1. Upload file to source bucket  
2. Wait a few seconds  
3. Check destination bucket  

✅ File should appear automatically  

---

## 📊 Verification

---

### 🔍 Lifecycle Verification

- Check object storage class over time  

---

### 🔁 Replication Verification

- File appears in destination bucket  
- Same content and metadata  

---

### 🔄 Versioning Check

- Upload same file multiple times  
- Verify versions exist  

---

## 🧹 Cleanup Steps

- Delete all objects from both buckets  
- Delete both buckets  
- (Optional) Delete auto-created IAM role  

---

## 📌 Best Practices

- Use lifecycle rules to reduce cost  
- Use Glacier for archival  
- Always enable versioning  
- Monitor replication  
- Use tags for better control  

---

## 🎯 Conclusion

In this practical, you:

- Implemented Lifecycle Management  
- Optimized storage cost  
- Configured Same-Region Replication (SRR)  
- Used auto IAM role for easy setup  

---
