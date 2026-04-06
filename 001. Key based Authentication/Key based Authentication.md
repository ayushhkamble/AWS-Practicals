# 🚀 AWS Practical Guide

## 🔐 SSH Key-Based Authentication for Secure User Access on EC2

---

## 📌 Project Objective

This practical demonstrates how to configure **SSH Key-Based Authentication** on an AWS EC2 Ubuntu instance to allow secure login to a specific user **without using passwords**.

---

## 🧰 Tools & Technologies

* Amazon Web Services (AWS)
* Amazon EC2
* Ubuntu Linux
* SSH
* SCP

---

## 🏗️ Architecture Overview

```
Local Machine
   |
   | (Private Key)
   |
   v
EC2 Instance (Ubuntu)
   |
   ├── User: ram
   └── /home/ram/.ssh/authorized_keys (Public Key)
```

---

## ⚙️ Working Principle

* Private key → stays on local machine
* Public key → stored on server
* SSH verifies both keys during login
* If matched → access granted
* Else → access denied

---

# 🛠️ Practical Implementation

---

## ✅ Step 1: Generate SSH Key Pair (Local Machine)

```bash
ssh-keygen
```

Generated files:

```
key      → Private Key
key.pub  → Public Key
```

---

## ✅ Step 2: Launch EC2 Instance

* OS: Ubuntu
* Instance Type: t2.micro
* Key Pair: ayush.pem
* Allow **Port 22 (SSH)** in Security Group

---

## ✅ Step 3: Connect to EC2

```bash
ssh -i ayush.pem ubuntu@<Public-IP>
```

---

## ✅ Step 4: Transfer Public Key

```bash
scp -i ayush.pem key.pub ubuntu@<Public-IP>:/home/ubuntu/
```

---

## ✅ Step 5: Create New User

```bash
sudo adduser ram
```

---

## ✅ Step 6: Setup SSH Directory

```bash
sudo mkdir -p /home/ram/.ssh
```

---

## ✅ Step 7: Configure Authorized Keys

```bash
sudo cp /home/ubuntu/key.pub /home/ram/.ssh/authorized_keys
```

---

## ✅ Step 8: Set Permissions

```bash
sudo chmod 700 /home/ram/.ssh
sudo chmod 600 /home/ram/.ssh/authorized_keys
sudo chown -R ram:ram /home/ram/.ssh
```

---

## ✅ Step 9: Login Using Key

```bash
ssh -i key ram@<Public-IP>
```

✅ Login successful without password

---

# 🔍 Real-Time Troubleshooting (From Practical Execution)

During the practical, multiple attempts were made to correctly configure SSH access. Below are the refined steps based on actual execution:

---

## 🔁 User Recreation (Fixing Setup Issues)

```bash
adduser ram
deluser ram
adduser ram
```

👉 Recreating the user helped fix incorrect initial configurations.

---

## 📂 File Movement Corrections

```bash
mv key.pub /home/ubuntu/
cp /home/ubuntu/key.pub /home/ram/.ssh/authorized_keys
```

---

## ⚠️ Common Typing Error

```bash
chwon -R ram:ram /home/ram/
```

❌ Incorrect command

✅ Correct command:

```bash
chown -R ram:ram /home/ram/
```

---

## ✅ Final Working Commands

```bash
mkdir -p /home/ram/.ssh
cp /home/ubuntu/key.pub /home/ram/.ssh/authorized_keys
chmod 700 /home/ram/.ssh
chmod 600 /home/ram/.ssh/authorized_keys
chown -R ram:ram /home/ram/.ssh
```

---

# 🔎 Verification

```bash
whoami
```

Output:

```
ram
```

---

# 🔐 Security Advantages

* No password transmission
* Strong encryption
* Prevents brute-force attacks
* Industry-standard authentication

---

# 🚨 Troubleshooting

### ❌ Permission Denied

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

### ❌ Connection Timeout

* Check Security Group (Port 22 open)
* Verify Public IP
* Ensure instance is running

---

### ❌ Ownership Issue

```bash
chown -R ram:ram /home/ram/
```

---

# 🎯 Project Outcome

✔ Configured SSH key-based authentication
✔ Created secure Linux user
✔ Implemented correct permissions
✔ Achieved passwordless login
✔ Understood real-world troubleshooting

---

# 🏁 Conclusion

This practical demonstrates secure SSH access using key-based authentication on AWS EC2. It also highlights real-world debugging scenarios, making it highly relevant for **DevOps and Cloud Engineering practices**.

---
