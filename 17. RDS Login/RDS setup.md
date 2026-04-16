# 🚀 Amazon RDS Setup and EC2 Connectivity with SQL Operations

---

## 📌 Project Title

**Amazon RDS Setup and EC2 Connectivity with SQL Operations**

---

## 🎯 Objective

This project demonstrates how to:

* Create and configure an Amazon RDS database
* Launch an EC2 instance
* Securely connect EC2 to RDS
* Perform SQL operations using username & password authentication

---

## 🌍 Real-World Use Case

A company hosts its web application on EC2 while storing data in a managed database (RDS).
This architecture ensures:

* Scalability
* Security
* High availability

---

## 🏗️ Architecture Diagram (Text-Based)

```
        ┌────────────────────┐
        │     EC2 Instance   │
        │ (Application Layer)│
        └────────┬───────────┘
                 │
     Secure Connection (Port 3306)
                 │
        ┌────────▼───────────┐
        │    Amazon RDS      │
        │   (MySQL DB)       │
        └────────────────────┘
```

---

## ⚙️ Prerequisites

* AWS Account
* IAM permissions for EC2 & RDS
* Key Pair for EC2
* Basic knowledge of Linux & SQL

---

# 🛠️ Step-by-Step Implementation

---

## 🔹 Step 1: Create RDS Instance

1. Go to **AWS Console → RDS → Create Database**

2. Choose:

   * Engine: **MySQL**
   * Version: Default

3. Templates:

   * Select **Free Tier**

4. Settings:

   * DB Instance Identifier: `my-rds-db`
   * Master Username: `admin`
   * Password: `StrongPassword123`

5. Instance Configuration:

   * Instance type: `db.t3.micro`

6. Storage:

   * 20 GB (default)

7. Connectivity:

   * VPC: Default
   * Public Access: ❌ **Disabled** (Important for security)
   * Subnet group: Default

8. Security Group:

   * Create new SG: `rds-sg`
   * Allow:

     * Type: MySQL
     * Port: 3306
     * Source: (we will update later)

9. Click **Create Database**

---

## 🔹 Step 2: Launch EC2 Instance

1. Go to **EC2 → Launch Instance**

2. Configure:

   * AMI: Ubuntu 22.04
   * Instance Type: `t2.micro`
   * Key Pair: Create or select existing

3. Network Settings:

   * VPC: Same as RDS
   * Security Group: `ec2-sg`

4. Inbound Rules:

   ```
   SSH (22) → Your IP
   ```

5. Launch instance

---

## 🔹 Step 3: Configure Security Groups (VERY IMPORTANT)

### 🔐 Update RDS Security Group

Go to **rds-sg → Inbound Rules → Edit**

Add:

```
Type: MySQL
Port: 3306
Source: ec2-sg (NOT 0.0.0.0/0)
```

### ❗ Why this is Important

* Prevents public access to database
* Only EC2 instance can connect
* Protects from hacking attempts

---

## 🔹 Step 4: Connect EC2 to RDS

### Step 4.1: SSH into EC2

```bash
ssh -i your-key.pem ubuntu@<EC2-Public-IP>
```

---

### Step 4.2: Install MySQL Client

```bash
sudo apt update -y
sudo apt install mysql-client -y
```

---

### Step 4.3: Connect to RDS

```bash
mysql -h <RDS-ENDPOINT> -u admin -p
```

Enter password when prompted.

✅ If successful:

```
Welcome to the MySQL monitor
```

---

## 🔹 Step 5: Perform SQL Operations

### 📌 Create Database

```sql
CREATE DATABASE company;
```

---

### 📌 Use Database

```sql
USE company;
```

---

### 📌 Create Table

```sql
CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50),
    role VARCHAR(50)
);
```

---

### 📌 Insert Data

```sql
INSERT INTO employees (name, role)
VALUES ('Ayush', 'Engineer');
```

---

### 📌 Select Data

```sql
SELECT * FROM employees;
```

✅ Output:

```
+----+--------+----------+
| id | name   | role     |
+----+--------+----------+
| 1  | Ayush  | Engineer |
+----+--------+----------+
```

---

### 📌 Update Data

```sql
UPDATE employees
SET role = 'Senior Engineer'
WHERE id = 1;
```

---

### 📌 Delete Data

```sql
DELETE FROM employees WHERE id = 1;
```

---

## 🔹 Step 6: Verification

✔ Successfully connected EC2 → RDS
✔ SQL queries executed
✔ Data retrieved correctly

---

## ❗ Troubleshooting

### 🔴 Error: Can't connect to MySQL

✔ Check:

* RDS endpoint correct
* Security group rules
* EC2 and RDS in same VPC

---

### 🔴 Timeout Error

✔ Ensure:

* RDS is NOT publicly accessible
* EC2 SG is allowed in RDS SG

---

### 🔴 Access Denied

✔ Verify:

* Username/password
* MySQL command syntax

---

## 🔐 Security Best Practices

* ❌ Do NOT enable public access unless required
* ✅ Use Security Groups instead of open IP
* ✅ Rotate database passwords regularly
* ✅ Use IAM roles where possible

---

## 📊 Bonus: Common Errors & Fixes

| Issue              | Fix                      |
| ------------------ | ------------------------ |
| Connection timeout | Check SG rules           |
| Access denied      | Verify credentials       |
| Host unreachable   | Check VPC/subnet         |
| DNS issue          | Use correct RDS endpoint |

---

## 🎯 Interview Questions

1. What is Amazon RDS?
2. Difference between RDS and EC2-hosted DB?
3. Why is public access disabled?
4. What is a security group?
5. How does EC2 connect to RDS securely?
6. What is RDS endpoint?
7. Difference between MySQL and PostgreSQL in RDS?

---

## 🏁 Conclusion

You have successfully:

* Created an RDS instance
* Launched EC2
* Established secure connectivity
* Executed SQL queries

This is a **real-world cloud architecture used in production systems** 🚀

---
