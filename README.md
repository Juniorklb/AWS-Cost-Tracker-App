# 💰 AWS Cost Tracker App ![AWS](https://img.shields.io/badge/Built%20with-AWS-orange?style=flat&logo=amazonaws)![Project Status](https://img.shields.io/badge/status-in--progress-yellow)

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
  - You should get something like this
    ![image alt](https://github.com/Juniorklb/AWS-Cost-Tracker-App/blob/124ffbd8ba08cc04d008d4c715f2fce659188815/images/IMG_6838.jpeg)
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

## Optional Lambda Function: Log Monthly AWS Costs

This Lambda function uses Cost Explorer to fetch current month cost and logs it to CloudWatch Logs

    import boto3
    import datetime
    import logging

    # Set up logging
    logger = logging.getLogger()
    logger.setLevel(logging.INFO)

    # Initialize Cost Explorer client
    ce = boto3.client('ce')

    def lambda_handler(event, context):
    today = datetime.date.today()
    start = today.replace(day=1).isoformat()
    end = today.isoformat()
    
    try:
        response = ce.get_cost_and_usage(
            TimePeriod={'Start': start, 'End': end},
            Granularity='MONTHLY',
            Metrics=['UnblendedCost']
        )
        
        cost = response['ResultsByTime'][0]['Total']['UnblendedCost']['Amount']
        currency = response['ResultsByTime'][0]['Total']['UnblendedCost']['Unit']
        
        message = f"AWS usage from {start} to {end} is {cost} {currency}"
        logger.info(message)
        
        # Optionally return the value
        return {
            'statusCode': 200,
            'message': message
        }
    
    except Exception as e:
        logger.error(f"Error retrieving cost: {str(e)}")
        raise
## IAM Permissions Required for Lambda:
Attach this inline policy or add permissions via rol

    {
      "Version": "2012-10-17",
     "Statement": [
    {
      "Effect": "Allow",
      "Action": "ce:GetCostAndUsage",
      "Resource": "*"
      }
      ]
    }


## 2. Architecture Diagram

---

## 👤 Author

*Junior Kalomba*    
Cloud & Security Enthusiast | Aspiring AWS Cloud Engineer
