# 🚀 AWS Lambda Practical: S3 Event Trigger with Email Notification

---

## 📌 Project Overview

This project demonstrates how to build a **serverless event-driven architecture** using AWS services. Whenever an action (upload, delete, update) occurs in an **Amazon S3 bucket**, it triggers an **AWS Lambda function**, which sends an **email notification** using **Amazon SNS**.

---

## 🎯 Objective

Automatically send an email notification whenever:
- A file is uploaded 📤
- A file is deleted ❌
- A file is modified 🔄  
in an S3 bucket.

---

## 🧰 Services Used

- **Amazon S3** – Storage & event source
- **AWS Lambda** – Compute service to process events
- **Amazon SNS** – Email notification service
- **IAM** – Access control and permissions
- **CloudWatch** – Logs and monitoring

---

## 🏗️ Architecture Diagram (Text Representation)

```

[S3 Bucket]
│
│ (Event: PUT / DELETE / POST)
▼
[AWS Lambda Function]
│
▼
[Amazon SNS Topic]
│
▼
[Email Notification 📧]

````

---

## ⚙️ Step-by-Step Implementation

---

### 🔹 Step 1: Create S3 Bucket

1. Go to AWS Console → S3
2. Click **Create Bucket**
3. Enter:
   - Bucket Name: `my-s3-event-bucket-123`
   - Region: Choose nearest region
4. Keep default settings → Click **Create Bucket**

📸 ![Step Screenshot]

---

### 🔹 Step 2: Create SNS Topic & Email Subscription

#### ✅ Create Topic
1. Go to SNS → Topics → Create Topic
2. Select **Standard**
3. Name: `s3-event-topic`
4. Click **Create Topic**

#### ✅ Create Subscription
1. Open created topic → Click **Create Subscription**
2. Protocol: **Email**
3. Endpoint: *Enter your email address*
4. Click **Create Subscription**

📩 **Important:** Check your email and **confirm subscription**

📸 ![Step Screenshot]

---

### 🔹 Step 3: Create IAM Role for Lambda

1. Go to IAM → Roles → Create Role
2. Select **Lambda**
3. Attach policies:
   - `AmazonS3ReadOnlyAccess`
   - `AmazonSNSFullAccess`
   - `CloudWatchLogsFullAccess`
4. Role Name: `lambda-s3-sns-role`

📸 ![Step Screenshot]

---

### 🔹 Step 4: Create Lambda Function

1. Go to Lambda → Create Function
2. Choose:
   - Author from scratch
   - Runtime: **Python 3.x**
3. Function Name: `s3-event-email-notifier`
4. Attach IAM Role: `lambda-s3-sns-role`

📸 ![Step Screenshot]

---

### 🔹 Step 5: Add S3 Trigger

1. Inside Lambda → Add Trigger
2. Select **S3**
3. Configure:
   - Bucket: `my-s3-event-bucket-123`
   - Event Types:
     - PUT
     - POST
     - DELETE
4. Enable trigger → Add

📸 ![Step Screenshot]

---

## 💻 Lambda Function Code (Python)

```python
import json
import boto3

sns_client = boto3.client('sns')
SNS_TOPIC_ARN = "YOUR_SNS_TOPIC_ARN"

def lambda_handler(event, context):
    print("Received event:", json.dumps(event))

    # ✅ Check if 'Records' exists
    if 'Records' not in event:
        return {
            'statusCode': 400,
            'body': json.dumps('No S3 event found')
        }

    try:
        for record in event['Records']:
            event_name = record['eventName']
            bucket_name = record['s3']['bucket']['name']
            object_key = record['s3']['object']['key']

            message = f"""
            🚨 S3 Event Notification 🚨
            Event Type: {event_name}
            Bucket: {bucket_name}
            File: {object_key}
            """

            sns_client.publish(
                TopicArn=SNS_TOPIC_ARN,
                Message=message,
                Subject="S3 Event Alert"
            )

        return {
            'statusCode': 200,
            'body': json.dumps('Success')
        }

    except Exception as e:
        print("Error:", str(e))
        raise e
````

---

## 🧪 Testing the Setup

### ✅ Test 1: Upload File

* Upload a file to S3 bucket

### ✅ Test 2: Delete File

* Delete any object from bucket

---

## 📧 Expected Output

You will receive an email like:

```
Subject: S3 Event Alert 🚀

🚨 S3 Event Notification 🚨

Event Type: ObjectCreated:Put
Bucket Name: my-s3-event-bucket-123
File Name: test.txt
```

---

## 📊 CloudWatch Logs Verification

1. Go to CloudWatch → Logs
2. Select `/aws/lambda/s3-event-email-notifier`
3. Check logs for:

   * Execution success
   * Errors (if any)

📸 ![Step Screenshot]

---

## ⚠️ Common Errors & Troubleshooting

| Issue                   | Cause                    | Solution                           |
| ----------------------- | ------------------------ | ---------------------------------- |
| ❌ Email not received    | SNS not confirmed        | Check inbox & confirm subscription |
| ❌ Lambda not triggered  | S3 trigger misconfigured | Re-check event types               |
| ❌ Permission denied     | IAM role issue           | Attach correct policies            |
| ❌ No logs in CloudWatch | Lambda not executed      | Check trigger                      |

---

## 🔐 Best Practices

* ✅ Use **least privilege IAM policies**
* ✅ Enable **CloudWatch logging**
* ✅ Use **environment variables** for SNS ARN
* ✅ Monitor using **CloudWatch alarms**
* ✅ Avoid unnecessary triggers to reduce cost

---

## 📜 Sample IAM Policy JSON

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["sns:Publish"],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject"],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "*"
    }
  ]
}
```

---

## 🌍 Real-World Use Cases

* 📂 File upload alerts in production systems
* 🔐 Security monitoring (unexpected file deletions)
* 📊 Data pipeline triggers
* 🧾 Invoice/document processing notifications

---

## 🎉 Conclusion

You have successfully built a **serverless event-driven notification system** using AWS!

This architecture is:

* ⚡ Scalable
* 💰 Cost-efficient
* 🔒 Secure
* 🚀 Production-ready

---

## ⭐ Bonus Tip

You can extend this project by:

* Adding **SMS notifications**
* Storing logs in **DynamoDB**
* Triggering **Step Functions workflows**

---
