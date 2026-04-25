# 🚀 AWS Practical Guide: Amazon EBS Volume Attachment and Mounting (200 GB)

## 📌 Project Overview

This practical demonstrates how to use **Amazon Elastic Block Store (EBS)** with an **EC2 instance**.

**Amazon EBS** provides **persistent block storage** that can be attached to EC2 instances just like a hard drive attached to a computer.

In this project, we will learn how to:

✔ Launch an EC2 Instance
✔ Create a **200 GB EBS Volume**
✔ Attach the Volume to EC2
✔ Connect to the server using SSH
✔ Partition the volume
✔ Format the partition
✔ Mount the storage to a directory

---

# 🏗 Architecture Diagram

```
             User (CMD / Terminal)
                     │
                     │ SSH Connection
                     │
              EC2 Instance (Linux)
                     │
                     │ Attached Storage (200 GB)
                     │
              Amazon EBS Volume
```

---

# ⚙ Prerequisites

Before starting, ensure you have:

* ✅ AWS Account
* ✅ Basic Linux knowledge
* ✅ EC2 Key Pair (.pem file)
* ✅ Command Prompt / Git Bash / Terminal
* ✅ Internet connection

---

# 🖥 Step 1: Launch an EC2 Instance

1️⃣ Login to **AWS Console**

2️⃣ Navigate to:

```
EC2 → Instances → Launch Instance
```

3️⃣ Configure the instance:

| Setting       | Value                 |
| ------------- | --------------------- |
| Instance Name | EBS-Practical         |
| AMI           | Amazon Linux / Ubuntu |
| Instance Type | t3.micro (Free Tier)  |
| Key Pair      | Create or Select      |
| Network       | Default               |
| Storage       | Default               |

4️⃣ Click **Launch Instance**

⏳ Wait until the instance state becomes **Running**.

---

# 💾 Step 2: Create a 200 GB EBS Volume

1️⃣ Navigate to:

```
EC2 → Elastic Block Store → Volumes
```

2️⃣ Click **Create Volume**

Configure the volume:

| Setting           | Value                |
| ----------------- | -------------------- |
| Volume Type       | gp3                  |
| Size              | **200 GB**           |
| Availability Zone | Same as EC2 Instance |

⚠ Important:
The **Availability Zone must match your EC2 instance**.

3️⃣ Click **Create Volume**

---

# 🔗 Step 3: Attach the Volume to EC2

1️⃣ Select the created **EBS Volume**

2️⃣ Click:

```
Actions → Attach Volume
```

3️⃣ Configure attachment:

| Setting     | Value                    |
| ----------- | ------------------------ |
| Instance    | Select your EC2 instance |
| Device Name | /dev/xvdbz                 |

4️⃣ Click **Attach Volume**

---

# 🔐 Step 4: Connect to EC2 Using SSH

Open terminal and run:

```
ssh -i your-key.pem ec2-user@your-public-ip
```

For Ubuntu:

```
ssh -i your-key.pem ubuntu@your-public-ip
```

---

# 🔍 Step 5: Check the Attached Volume

```
lsblk
```

Expected output:

```
xvda      8:0    0   8G    0 disk
└─xvda1

nvme1n1    8:80   0   200G  0 disk
```

✔ `nvme1n1` is your new **200 GB EBS disk**

---

# 📦 Step 6: Create a Partition

```
sudo fdisk /dev/nvme1n1
```

Inside fdisk:

```
n → New partition  
p → Primary  
1 → Partition number  
Enter → Default start  
Enter → Use full 200 GB  
w → Write changes  
```

Verify:

```
lsblk
```

Now you will see:

```
nvme1n1p1   200G
```

---

# 🧱 Step 7: Format the Partition

```
sudo mkfs -t ext4 /dev/nvme1n1p1
```

---

# 📁 Step 8: Create Mount Directory

```
sudo mkdir /data
```

---

# 🔌 Step 9: Mount the Volume

```
sudo mount /dev/nvme1n1p1 /data
```

Check:

```
df -h
```

Expected:

```
Filesystem      Size   Used  Avail Mounted on
/dev/nvme1n1p1      200G   0     200G  /data
```

---

# 🧪 Step 10: Test Storage

```
cd /data
sudo touch testfile.txt
ls
```

✔ File created → Storage working ✅

---

# 🔄 Step 11: Make Mount Permanent

```
sudo nano /etc/fstab
```

Add:

```
/dev/nvme1n1p1   /data   ext4   defaults,nofail   0   2
```

Save and exit.

---
mount -av

---

# 🎯 Practical Outcome

✔ Successfully attached a **200 GB EBS volume**
✔ Performed partitioning and formatting
✔ Mounted storage to Linux directory
✔ Verified persistent storage

---

# 📚 Real World Use Cases

* 🗄 Large database storage
* 💾 Backup volumes
* 📦 File storage systems
* ⚡ High-performance applications
* 🖥 Persistent EC2 storage

---

# 🏁 Conclusion

In this practical, we successfully configured a **200 GB Amazon EBS volume** with an EC2 instance and performed essential Linux disk operations like **partitioning, formatting, and mounting**.

This gives a strong foundation in **AWS storage management and real-world cloud deployments**.

---
