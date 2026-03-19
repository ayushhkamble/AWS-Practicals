# 🚀 AWS Practical Guide: Amazon EBS Volume Attachment and Mounting

## 📌 Project Overview

This practical demonstrates how to use **Amazon Elastic Block Store (EBS)** with an **EC2 instance**.

**Amazon EBS** provides **persistent block storage** that can be attached to EC2 instances just like a hard drive attached to a computer.

In this project, we will learn how to:

✔ Launch an EC2 Instance
✔ Create an EBS Volume
✔ Attach the Volume to EC2
✔ Connect to the server using SSH
✔ Partition the volume
✔ Format the partition
✔ Mount the storage to a directory

This practical helps beginners understand **AWS storage management and Linux disk operations**.

---

# 🏗 Architecture Diagram

```
             User (CMD / Terminal)
                     │
                     │ SSH Connection
                     │
              EC2 Instance (Linux)
                     │
                     │ Attached Storage
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
| Instance Type | t2.micro (Free Tier)  |
| Key Pair      | Create or Select      |
| Network       | Default               |
| Storage       | Default               |

4️⃣ Click **Launch Instance**

⏳ Wait until the instance state becomes **Running**.

---

# 💾 Step 2: Create an EBS Volume

1️⃣ Navigate to:

```
EC2 → Elastic Block Store → Volumes
```

2️⃣ Click **Create Volume**

Configure the volume:

| Setting           | Value                |
| ----------------- | -------------------- |
| Volume Type       | gp3                  |
| Size              | 5 GB                 |
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
| Device Name | /dev/sdf                 |

4️⃣ Click **Attach Volume**

Your additional storage is now attached to the EC2 instance.

---

# 🔐 Step 4: Connect to EC2 Using SSH

Open **Command Prompt / Git Bash / Terminal**

Run:

```
ssh -i your-key.pem ec2-user@your-public-ip
```

Example:

```
ssh -i ayush.pem ec2-user@13.xxx.xxx.xxx
```

For Ubuntu:

```
ssh -i your-key.pem ubuntu@your-public-ip
```

After login, you will get **remote access to your EC2 server**.

---

# 🔍 Step 5: Check the Attached Volume

Run the command:

```
lsblk
```

Example output:

```
xvda      8:0    0   8G   0 disk
└─xvda1

xvdf      8:80   0   5G   0 disk
```

Explanation:

| Device | Meaning                 |
| ------ | ----------------------- |
| xvda   | Root disk               |
| xvdf   | Newly attached EBS disk |

---

# 📦 Step 6: Create a Partition

Run:

```
sudo fdisk /dev/xvdf
```

Inside **fdisk**, press:

```
n → New partition
p → Primary partition
1 → Partition number
Enter → Default start
Enter → Default end
w → Write changes
```

Verify partition:

```
lsblk
```

You should now see:

```
xvdf1
```

---

# 🧱 Step 7: Format the Partition

Now format the partition using **ext4 filesystem**.

```
sudo mkfs.ext4 /dev/xvdf1
```

This prepares the disk for storing files.

---

# 📁 Step 8: Create a Mount Directory

Create a directory where the storage will be mounted.

```
sudo mkdir /data
```

---

# 🔌 Step 9: Mount the Volume

Mount the partition:

```
sudo mount /dev/xvdf1 /data
```

Check the mounted storage:

```
df -h
```

Example output:

```
Filesystem      Size  Used  Avail Mounted on
/dev/xvdf1      5G    0     5G    /data
```

This means the **EBS volume is successfully mounted**.

---

# 🧪 Step 10: Test the Storage

Move into the directory:

```
cd /data
```

Create a test file:

```
sudo touch testfile.txt
```

Verify:

```
ls
```

If the file appears, the storage is working correctly.

---

# 🔄 (Optional) Make Mount Permanent

To mount the disk automatically after reboot:

Open **fstab file**

```
sudo nano /etc/fstab
```

Add the following line:

```
/dev/xvdf1   /data   ext4   defaults,nofail   0   2
```

Save and exit.

Now the volume will **automatically mount after reboot**.

---

# 🎯 Practical Outcome

After completing this practical, you have learned how to:

✔ Launch an EC2 instance
✔ Create an Amazon EBS volume
✔ Attach the volume to EC2
✔ Connect to the server using SSH
✔ Create disk partitions
✔ Format the partition
✔ Mount storage in Linux

---

# 📚 Real World Use Cases

Amazon EBS is commonly used for:

* 🗄 Database storage
* 💾 Application storage
* 📦 Backup volumes
* ⚡ High-performance workloads
* 🖥 Persistent storage for EC2

---

# 🏁 Conclusion

In this practical, we successfully configured **Amazon EBS storage with an EC2 instance** and performed Linux disk management tasks such as **partitioning, formatting, and mounting**.

This practical provides a strong foundation for understanding **AWS storage services and Linux file system management**.
