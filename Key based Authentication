## 📌 Project Title

**Implementing SSH Key-Based Authentication for Secure User Access on AWS EC2**

## 🎯 Project Objective

The goal of this project is to configure **SSH Key-Based Authentication** to securely log in to a specific user account on an AWS EC2 Ubuntu instance without using a password.

This project demonstrates:

* Secure remote login using SSH keys
* Creating and managing Linux users
* Configuring permissions for SSH authentication
* AWS EC2 instance setup
* File transfer using SCP


## 🧰 Technologies & Tools Used

* Amazon Web Services (AWS)
* Amazon EC2
* Ubuntu Linux
* SSH
* SCP
* GitHub


## 🏗️ Architecture Overview

Local Machine
   |
   |  (Private Key)
   |
   v
AWS EC2 Ubuntu Server
   |
   |-- New User (ram)
   |-- ~/.ssh/authorized_keys (Public Key)


**Working Principle:**

* The private key remains on the client machine.
* The public key is stored inside the server user's `authorized_keys` file.
* During login, SSH verifies that both keys match.
* If matched → access granted.
* If not → access denied.


# 🛠️ Practical Implementation Steps


## ✅ Step 1: Generate SSH Key Pair (Public & Private Key)

On your local machine:

ssh-keygen

This generates:

```
key        → Private Key
key.pub    → Public Key
```

🔹 **Private Key** → Keep secure on your local machine
🔹 **Public Key** → Upload to server


## ✅ Step 2: Create EC2 Instance

1. Login to AWS Console
2. Go to EC2 Dashboard
3. Launch Instance
4. Choose:

   * OS: Ubuntu
   * Instance Name: Key-Authentication
   * Key Pair: ayush.pem
5. Allow SSH (Port 22) in Security Group
6. Launch Instance


## ✅ Step 3: Transfer Public Key to EC2 Server

Use SCP to transfer `key.pub`:

scp -i ayush.pem key.pub ubuntu@<Public-IP>:/home/ubuntu


Replace `<Public-IP>` with your EC2 public IP address.


## ✅ Step 4: Verify File on Server

Login to server:

ssh -i ayush.pem ubuntu@<Public-IP>

## ✅ Step 5: Create a New User

Switch to root:

sudo su

Create user:

adduser ram


## ✅ Step 6: Create .ssh Directory for New User

mkdir /home/ram/.ssh


## ✅ Step 7: Setup Authorized Keys

Move public key as authorized_keys:

mv /home/ubuntu/key.pub /home/ram/.ssh/authorized_keys


## ✅ Step 8: Set Correct Permissions (Very Important)

chmod 700 /home/ram/.ssh
chmod 600 /home/ram/.ssh/authorized_keys
chown -R ram:ram /home/ram/


🔐 Why permissions matter?

* SSH will reject authentication if permissions are incorrect.
* `.ssh` must be 700
* `authorized_keys` must be 600

---

## ✅ Step 9: Login Using Private Key

From local machine:

ssh -i key ram@<Public-IP>

If configuration is correct:

✅ You will get remote SSH shell access
❌ No password required

---

# 🔎 Verification

After login:

whoami

Output:

ram

This confirms successful key-based authentication.


# 🔐 Security Advantages of Key-Based Authentication

* No password transmission
* Resistant to brute-force attacks
* More secure than password authentication
* Recommended in production environments
* Used in DevOps & Cloud Infrastructure


# 🚨 Common Errors & Troubleshooting

### 1️⃣ Permission Denied (Public Key)

Check:

chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys


### 2️⃣ Connection Timed Out

* Check Security Group allows Port 22
* Verify Public IP
* Ensure instance is running


### 3️⃣ Wrong Ownership

chown -R ram:ram /home/ram/

# 📁 Suggested GitHub Repository Structure

SSH-Key-Authentication/
│
├── README.md
├── screenshots/
│   ├── keygen.png
│   ├── ec2-launch.png
│   ├── ssh-login.png
│
└── commands.txt


# 📚 Interview Questions & Answers

### ❓ What is SSH Key-Based Authentication?

It is a secure authentication mechanism where a public-private key pair is used instead of passwords to authenticate users.


### ❓ Where is the public key stored?

In:

~/.ssh/authorized_keys


### ❓ Why are permissions important in SSH?

SSH refuses login if `.ssh` or `authorized_keys` permissions are too open for security reasons.


### ❓ Difference Between Public & Private Key?

| Public Key            | Private Key             |
| --------------------- | ----------------------- |
| Stored on server      | Stored on client        |
| Shared                | Kept secret             |
| Used for verification | Used for authentication |

---

# 🎯 Project Outcome

✔ Successfully configured SSH key-based authentication
✔ Created secure Linux user
✔ Implemented proper permissions
✔ Logged in without password
✔ Understood AWS EC2 networking and security


# 🏁 Conclusion

This project demonstrates how to securely configure SSH key-based authentication for a specific user on an AWS EC2 Ubuntu server.

It is a fundamental skill required for:

* DevOps Engineers
* Cloud Engineers
* System Administrators
* Linux Administrators
