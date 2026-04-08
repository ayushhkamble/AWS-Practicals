# 🚀 AWS Practical Guide: S3 Lifecycle Management & Cross-Region Replication (CRR)

---

## 📌 Project Title
**Amazon S3 Lifecycle Management and Cross-Region Replication (CRR) Implementation**

---

## 🎯 Objective

Design and implement an AWS S3 setup where:

- Objects automatically transition between storage classes using Lifecycle Rules  
- Old objects are expired automatically  
- Data is replicated from a source bucket to a destination bucket (CRR)  
- Versioning is enabled to support replication  

---

## 📖 Introduction

### 🔹 What is S3 Lifecycle Management?

**S3 Lifecycle Management** allows you to automate object transitions between storage classes and delete old data to reduce costs.

---

### 🔹 What is S3 Replication?

S3 Replication automatically copies objects from one bucket to another.

---

### 🔄 Types of Replication

| Type | Description |
|------|------------|
| 🔁 SRR (Same Region Replication) | Replicates data within same region |
| 🌍 CRR (Cross-Region Replication) | Replicates data across regions |

---

### 🔍 Difference

| Feature | Lifecycle Rules | Replication |
|--------|----------------|-------------|
| Purpose | Cost optimization | Data duplication |
| Action | Move/delete objects | Copy objects |
| Use Case | Archival | Backup/DR |

---

### ✅ Benefits

- 💰 Cost Optimization (Lifecycle)
- 🔒 Data Durability (Replication)
- 🌍 Disaster Recovery (CRR)

---

## 🏗️ Architecture Overview

### 🔧 Components

- **Source Bucket** → Original data storage  
- **Destination Bucket** → Replicated data  
- **Lifecycle Rules** → Transition & expiration  
- **Replication Rules** → Data copying  
- **IAM Role** → Permission for replication  

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

📦 Lifecycle        🔁 Replication
(30d → IA)           |
(60d → Glacier)      ↓
(90d → Delete)  ┌────────────────────┐
│ Destination Bucket │ (Different Region)
└────────────────────┘

```

---

## ⚙️ Step-by-Step Implementation

---

# 🔹 Part A: Lifecycle Rules

---

### 🧱 Step 1: Create S3 Bucket

1. Go to **AWS Console → S3**
2. Click **Create Bucket**

#### 📝 Configuration:

- **Bucket Name**: `lifecycle-demo-bucket-12345`
- **Region**: Choose region (e.g., ap-south-1)

---

### 📤 Step 2: Upload Sample Files

Upload test files:

- `.txt` files  
- images (.jpg, .png)  

👉 Purpose:
- To observe lifecycle transitions and expiration

---

### ⚙️ Step 3: Create Lifecycle Rule

1. Go to **Bucket → Management**
2. Click **Create Lifecycle Rule**

---

### 🔧 Configuration:

- **Rule Name**: `Lifecycle-Rule-1`
- **Scope**: Apply to all objects

---

### 📦 Actions:

- Transition to **Standard-IA** after **30 days**
- Transition to **Glacier** after **60 days**
- Expire objects after **90 days**

---

## 📦 Step 4: Storage Classes Explained

| Storage Class | Use Case | Cost | Access Speed |
|--------------|---------|------|--------------|
| 🟢 Standard | Frequently accessed data | High | Instant |
| 🟡 Standard-IA | Infrequent access | Medium | Instant |
| 🔵 Glacier | Archival data | Very Low | Minutes/Hours |

👉 Trade-off:
- Lower cost = slower access

---

# 🔹 Part B: Cross-Region Replication (CRR)

---

### 🌍 Step 5: Create Destination Bucket

- Name: `replication-destination-bucket-12345`
- Region: **Different region** (e.g., us-east-1)

👉 Important:
CRR requires different regions

---

### 🔄 Step 6: Enable Versioning

Enable versioning on both buckets:

1. Go to **Properties**
2. Enable **Versioning**

---

### ❗ Why Versioning is Required?

- Tracks object versions
- Prevents overwrite issues
- Required for replication to work

---

### 🔐 Step 7: Create IAM Role for Replication

1. Go to **IAM → Roles**
2. Click **Create Role**

---

### 🔧 Configuration:

- Service: **S3**
- Use case: **S3 Replication**

---

### 📜 Permissions:

Attach policy allowing:

- Read from source bucket  
- Write to destination bucket  

---

### 🤝 Trust Relationship (Concept)

- Allows S3 service to assume this role
- Enables replication process

---

### 🔁 Step 8: Configure Replication Rule

1. Go to **Source Bucket → Management**
2. Click **Replication → Create Rule**

---

### 🔧 Configuration:

- **Rule Name**: `CRR-Rule`
- **Status**: Enabled
- **Source**: Entire bucket
- **Destination Bucket**: Select destination bucket
- **IAM Role**: Select created role

---

### 📌 Option:

- Replicate existing objects (optional)

---

### 🧪 Step 9: Test Replication

1. Upload a file to **Source Bucket**
2. Wait a few seconds
3. Check **Destination Bucket**

✅ File should appear automatically

---

## 📊 Verification

---

### 🔍 Lifecycle Verification

- Check object properties
- Verify storage class changes over time

---

### 🔁 Replication Verification

- File appears in destination bucket  
- Same file name and content  

---

### 🔄 Versioning Check

- Upload same file multiple times  
- Check versions tab  

---

## 🧹 Cleanup Steps

To avoid charges:

1. Delete all objects (both buckets)
2. Disable replication
3. Delete source & destination buckets
4. Delete IAM role

---

## 📌 Best Practices

### 💡 Recommendations:

- ✅ Use Lifecycle rules for cost saving  
- ✅ Use Glacier for archival  
- ✅ Always enable versioning  
- ✅ Monitor replication status  
- ✅ Use tags for fine-grained lifecycle rules  

---

## 🎯 Conclusion

In this practical, you successfully:

- Configured Lifecycle Rules for automatic storage optimization  
- Transitioned objects across storage classes  
- Set up Cross-Region Replication (CRR)  
- Ensured data durability and disaster recovery  

🎉 Your S3 storage is now **automated, optimized, and highly resilient!**

---
