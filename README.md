# 🛠️ AWS Serverless CRUD API

This is a **serverless CRUD API** built on AWS using key cloud-native services. It performs **Create, Read, Update, and Delete** operations on a DynamoDB table and is triggered via API Gateway using AWS Lambda functions.

This was created as a personal project based on a tutorial by *Code with Vinnie* to gain experience with real-world serverless architecture and to showcase practical AWS skills.

---

## ✨ Features

* Fully serverless
* Uses AWS managed services
* Fast NoSQL performance with DynamoDB
* Easily testable via Postman
* Basic IAM role security and CloudWatch logging

---

## 🏛️ Architecture Diagram

```
Client (Postman or Web App)
        |
        v
API Gateway (REST API)
        |
        v
AWS Lambda (CRUD handlers)
        |
        v
Amazon DynamoDB ("Tasks" table)
        |
        v
CloudWatch Logs (monitoring & debugging)
```

---

## 🔹 AWS Services Used

| Service     | Purpose                                               |
| ----------- | ----------------------------------------------------- |
| API Gateway | Exposes RESTful endpoints to trigger Lambda functions |
| AWS Lambda  | Contains business logic for CRUD operations           |
| DynamoDB    | NoSQL table to store and retrieve task items          |
| IAM         | Secure role-based access for Lambda                   |
| CloudWatch  | Logs function output and errors for debugging         |

---

## 📂 Lambda Code Overview

The Lambda function (`lambda/index.js`) contains logic to:

* `GET`    - Read all tasks from the table
* `POST`   - Create a new task (with `id`, `name`, `completed`)
* `PATCH`  - Update a task by ID
* `DELETE` - Remove a task by ID

---

## 🔧 Setup Instructions

### 1. Create DynamoDB Table

* Service: **DynamoDB**
* Table name: `Tasks`
* Partition key: `id` (String)
* Leave all defaults and create the table

### 2. Create Lambda Function

* Service: **Lambda**
* Function name: `task-crud-handler`
* Runtime: Node.js
* Permissions: Create a new role with basic Lambda permissions

Paste Lambda code from `lambda/index.js` (you can create this based on your own or modify it from the tutorial).

### 3. Attach Permissions to Lambda

* Go to IAM > Roles > `task-crud-handler-role`
* Attach the following policies:

  * `AWSLambdaBasicExecutionRole`
  * `AmazonDynamoDBFullAccess`
  * `CloudWatchLogsFullAccess`

### 4. Create REST API in API Gateway

* Service: **API Gateway** > REST API > Build
* API name: `task-api`
* Create a resource: `/task`
* Enable CORS
* Create 4 methods under `/task`:

  * `GET`, `POST`, `PATCH`, `DELETE`
  * Use **Lambda Proxy Integration** and link to `task-crud-handler`

### 5. Deploy API Gateway

* Create a stage named `production`
* Note the invoke URL: `https://<api-id>.execute-api.<region>.amazonaws.com/production/task`

---

## 👁️ Testing the API (Postman)

Import the included file: [lambda-code](https://github.com/DanielCloudSec/aws-serverless-crud/blob/200bfc61809a5514b93cdf9d80fea20ac29cbf8a/lambda-code)

Update the `{{base_url}}` variable to your deployed API Gateway base URL.

Each request includes:

* `GET` – fetch all tasks
* `POST` – create a task
* `PATCH` – update a task
* `DELETE` – remove a task

---

## 🔍 Logging & Debugging

* Visit **CloudWatch > Log groups**
* Look for the group matching `task-crud-handler`
* Open the latest log stream to view Lambda errors and outputs

---

## 🚀 Future Improvements

* Add schema validation (e.g., with Joi or middleware)
* Implement API key or Cognito authentication
* Add unit tests for Lambda logic
* Migrate to IaC using AWS SAM or CDK

---

## 👤 Author

**Daniel DeVore**
Built as a hands-on AWS learning project and for public showcase.

GitHub: [@DanielCloudSec](https://github.com/DanielCloudSec)
