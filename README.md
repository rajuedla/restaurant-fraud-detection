# 🛡️ Restaurant Order Fraud Detection Platform

## Overview

A cloud-native, serverless fraud detection system built on AWS to identify and prevent fraudulent restaurant orders in real time.  
This project demonstrates **enterprise-grade cloud architecture**, **security best practices**, **infrastructure as code**, and **serverless design patterns** using AWS.

The platform ingests restaurant orders via an API, evaluates fraud risk using configurable rules, stores results securely, and exposes historical fraud insights through a query API.

---

## Live Demo (Deployed on AWS)

**Base URL (dev environment):**  
https://uk9w8ve1ea.execute-api.us-east-1.amazonaws.com/dev

---

## API Demo (Live & Working)

**Base URL (dev environment):**  
https://uk9w8ve1ea.execute-api.us-east-1.amazonaws.com/dev

---

### 1️⃣ Submit an Order

```bash
API="https://uk9w8ve1ea.execute-api.us-east-1.amazonaws.com/dev"

curl -s -X POST "$API/orders" \
  -H "Content-Type: application/json" \
  -d '{
    "order_id":"o-3003",
    "restaurant_id":"r-01",
    "amount":49.99,
    "card_hash":"card-low"
  }'
```
**Response**
```json
{
  "message": "Order received",
  "order_id": "o-3003"
}
```


## 🏗️ Architecture

### High-Level Architecture

```
Client / UI
   │
   ▼
Amazon API Gateway (HTTP API)
   │
   ├── POST /orders   ──► Order Submit Lambda
   │                     • Validates request
   │                     • Stores order in DynamoDB
   │                     • Sends event to SQS
   │
   ├── POST /score    ──► Fraud Scorer Lambda
   │                     • Retrieves order
   │                     • Calculates risk score
   │                     • Stores fraud score
   │                     • Triggers SNS alert (HIGH risk)
   │
   └── GET /history   ──► History Query Lambda
                         • Fetches fraud history
                         • Returns JSON response
                             │
                             ▼
                      Amazon DynamoDB
                  ┌────────────────────┐
                  │ Orders Table        │
                  │ Fraud Scores Table  │
                  └────────────────────┘
                             │
                             ▼
                       Amazon SNS
                    (Fraud Alert Emails)
                             │
                             ▼
                       Amazon SQS
                  (Asynchronous Processing)
```

### Key Architectural Principles

- Fully serverless (no EC2, no containers)
- Event-driven design
- Loose coupling between services
- Least-privilege IAM roles
- Encrypted data at rest and in transit
- Environment-based isolation (dev / prod)

---

## 🧰 Technology Stack

### Cloud & Infrastructure
- **AWS Lambda** – Serverless compute
- **Amazon API Gateway (HTTP API)** – Public REST endpoints
- **Amazon DynamoDB** – NoSQL storage
- **Amazon S3** – Data storage
- **Amazon SNS** – Fraud alert notifications
- **Amazon SQS** – Asynchronous processing
- **AWS KMS** – Encryption
- **Amazon CloudWatch** – Logs & metrics

### Infrastructure as Code
- **Terraform** (100% IaC)
- Modular, environment-based deployment

### Programming & Tooling
- **Python 3.11**
- **Boto3 (AWS SDK)**
- **GitHub Actions** (CI/CD)

---

## 🎯 Core Features

### Real-Time Fraud Detection
- Order amount threshold checks
- Rule-based fraud scoring
- High-risk order alerts
- Historical fraud analysis

### Risk Classification
- **LOW** – Auto-approved
- **HIGH** – Alert triggered

### Security & Compliance
- IAM least privilege
- KMS-encrypted DynamoDB & S3
- HTTPS-only APIs
- CloudWatch audit logs

---

## 📁 Project Structure

```
restaurant-fraud-detection/
├── README.md
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   ├── lambda_api.tf
│   └── environments/
│       ├── dev/
│       └── prod/
├── lambda-functions/
│   ├── order-submit/
│   ├── fraud-scorer/
│   ├── history-query/
│   └── build_zips.sh
├── scripts/
│   └── generate-test-data.py
└── docs/
```

---

## 📊 Example Fraud Flow

1. Order submitted via `/orders`
2. Stored in DynamoDB
3. Fraud score calculated via `/score`
4. Result stored in Fraud Scores table
5. High-risk orders trigger SNS alert
6. Fraud history retrieved via `/history`

---

## 👤 Author

**Raju Edla**  
📧 Email: edlaraju200@gmail.com  
🔗 LinkedIn: https://www.linkedin.com/in/raju-edla/  
💻 GitHub: https://github.com/rajuedla

---
