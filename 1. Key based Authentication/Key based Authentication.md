# 🚀 AWS Practical Guide

## 🔐 Implementing SSH Key-Based Authentication for Secure User Access on EC2

---

## 📌 Project Objective

This practical demonstrates how to configure **SSH Key-Based Authentication** on an AWS EC2 Ubuntu instance to securely allow login to a specific user **without using passwords**.

---

## 🧰 Technologies Used

* Amazon Web Services (AWS)
* Amazon EC2
* Ubuntu Linux
* SSH
* SCP
* GitHub

---

## 🏗️ Architecture Overview

```
Local Machine
   |
   | (Private Key: key)
   |
   v
EC2 Ubuntu Instance
   |
   ├── User: ram
   └── ~/.ssh/authorized_keys (Public Key)
```

---

## ⚙️ Working Principle

* Private key stays on **client machine**
* Public key is stored in **server (`authorized_keys`)**
* SSH matches keys during login:

  * ✅ Match → Access Granted
  * ❌ No Match → Access Denied

---

# 🛠️ Step-by-Step Implementation

---

## ✅ Step 1: Generate SSH Key Pair (Local Machine)

```bash
ssh-keygen -t rsa -b 2048 -f key
```

This creates:

```
key       → Private Key
key.pub   → Public Key
```

🔐 Keep the **private key secure**
📤 Public key will be uploaded to the server

---

## ✅ Step 2: Launch EC2 Instance

1. Go to AWS Console → EC2
2. Click **Launch Instance**
3. Configure:

   * **Name:** Key-Authentication
   * **AMI:** Ubuntu
   * **Instance Type:** t2.micro (Free Tier)
   * **Key Pair:** ayush.pem
4. In **Security Group**:

   * Allow **SSH (Port 22)** from your IP
5. Launch the instance

---

## ✅ Step 3: Connect to EC2 Instance

```bash
ssh -i ayush.pem ubuntu@<Public-IP>
```

---

## ✅ Step 4: Transfer Public Key to Server

From your local machine:

```bash
scp -i ayush.pem key.pub ubuntu@<Public-IP>:/home/ubuntu/
```

---

## ✅ Step 5: Create a New User

```bash
sudo adduser ram
```

Set password (optional, not used for SSH login)

---

## ✅ Step 6: Create SSH Directory for User

```bash
sudo mkdir -p /home/ram/.ssh
```

---

## ✅ Step 7: Add Public Key to Authorized Keys

```bash
sudo cp /home/ubuntu/key.pub /home/ram/.ssh/authorized_keys
```

---

## ✅ Step 8: Set Correct Permissions (IMPORTANT)

```bash
sudo chmod 700 /home/ram/.ssh
sudo chmod 600 /home/ram/.ssh/authorized_keys
sudo chown -R ram:ram /home/ram/.ssh
```

---

## ✅ Step 9: Disable Password Authentication (Optional but Recommended)

Edit SSH config:

```bash
sudo nano /etc/ssh/sshd_config
```

Change:

```
PasswordAuthentication no
```

Restart SSH:

```bash
sudo systemctl restart ssh
```

---

## ✅ Step 10: Login Using Key-Based Authentication

From local machine:

```bash
ssh -i key ram@<Public-IP>
```

✅ Login successful without password

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

* No password exposure
* Protection against brute-force attacks
* Strong encryption
* Industry-standard authentication
* Widely used in DevOps environments

---

# 🚨 Troubleshooting

## ❌ Permission Denied (Public Key)

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

## ❌ Connection Timeout

* Check Security Group → Port 22 open
* Verify Public IP
* Ensure instance is running

---

## ❌ Wrong Ownership

```bash
sudo chown -R ram:ram /home/ram/.ssh
```

---

### ❓ Public vs Private Key?

| Public Key       | Private Key      |
| ---------------- | ---------------- |
| Stored on server | Stored on client |
| Shared           | Secret           |
| Verifies         | Authenticates    |

---

# 🎯 Project Outcome

✔ Secure SSH login without password
✔ Created and configured Linux user
✔ Implemented correct permissions
✔ Understood EC2 security and access control

---

# 🏁 Conclusion

This practical demonstrates how to securely configure SSH key-based authentication for user access on an AWS EC2 instance. It is a **fundamental DevOps and Cloud security skill** used in real-world production environments.

---
