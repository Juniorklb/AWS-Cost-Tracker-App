# 💰 AWS Cost Tracker App

This project helps you track your AWS spending in real time using **AWS Budgets**, **SNS notifications**, and optional **Lambda** integration. It's designed to alert you when your usage exceeds your defined monthly budget, helping you avoid unexpected charges.

---

## 🧭 Project Overview

### 🎯 Goals
- Monitor AWS monthly cost using **AWS Budgets**.
- Send email alerts via **SNS** when spending exceeds a threshold.
- (Optional) Log budget metrics with **Lambda** for reporting or dashboard integration.

---

## ⚙️ AWS Services Used

| Service         | Role                                                   |
|-----------------|--------------------------------------------------------|
| AWS Budgets     | Tracks your spending and usage                         |
| Amazon SNS       | Sends cost alerts to your email inbox                 |
| AWS IAM         | Grants secure permissions for budget monitoring        |
| AWS Lambda (Optional) | Logs budget data to CloudWatch or other services |

---

## 🛠️ Setup Instructions

### Step 1: Create an SNS Topic
1. Go to the [SNS Console](https://console.aws.amazon.com/sns)
2. Create a topic:  
   - Type: **Standard**
   - Name: `BillingAlerts`
3. Create a subscription:
   - Protocol: `Email`
   - Endpoint: *Your email address*
4. Confirm the email subscription from your inbox.

---

### Step 2: Create an AWS Budget
1. Go to the [AWS Budgets Console](https://console.aws.amazon.com/billing/home#/budgets)
2. Click **Create budget**
3. Choose **Cost budget**
4. Set budget amount (e.g., `$10/month`)
5. In “Notifications”:
   - Add an alert at 80% of budget
   - Send to SNS topic: `BillingAlerts`
6. Finish and save

---

### 📩 Step 3: Get Alerts
- When your AWS spend crosses 80% of your budget, you'll receive an email alert from AWS.

---

## 🧪 Optional: Log or Visualize Cost via Lambda

You can optionally create a Lambda function that:
- Queries the **AWS Cost Explorer API**
- Logs current usage to **CloudWatch**, **DynamoDB**, or a **dashboard**
- Sends a weekly report via email

*(Optional Lambda code examples can be added in a `/lambda` folder)*

---

## 📂 Project Structure

