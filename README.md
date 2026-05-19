# aws-deployment-pipeline

## Project Overview

This project demonstrates a fully automated CI/CD pipeline for deploying a static portfolio website using AWS cloud services.

The solution automatically:
- retrieves source code from GitHub
- runs automated validation tests using AWS CodeBuild
- deploys website files to Amazon S3
- distributes content globally using CloudFront
- sends real-time failure notifications using EventBridge and Amazon SNS

The project follows a fully serverless cloud architecture and does not use any virtual machines.

---

## Live Website

https://YOUR-CLOUDFRONT-URL.cloudfront.net

---

## How to Deploy

Push any change to the `main` branch on GitHub.

AWS CodePipeline triggers automatically and performs:
1. Source retrieval from GitHub
2. Automated testing using CodeBuild
3. Deployment to Amazon S3
4. CloudFront cache invalidation

If the build fails, deployment stops automatically and the live website is not overwritten.

---

# Architecture

## Deployment Flow

GitHub  
↓  
AWS CodePipeline  
↓  
AWS CodeBuild  
↓  
Amazon S3 Static Website Hosting  
↓  
Amazon CloudFront CDN  

## Monitoring Flow

AWS CodeBuild Failure  
↓  
Amazon EventBridge  
↓  
Amazon SNS  
↓  
Email Notification  

---

# AWS Services Used

| Service | Purpose |
|---|---|
| AWS CodePipeline | Orchestrates the CI/CD workflow |
| AWS CodeBuild | Runs automated tests before deployment |
| Amazon S3 | Hosts the static website files |
| Amazon CloudFront | CDN, HTTPS, and global low-latency delivery |
| Amazon EventBridge | Detects CodeBuild failure events |
| Amazon SNS | Sends real-time email alerts |
| Amazon CloudWatch | Stores build and debugging logs |

No virtual machines were used. The entire solution is serverless.

---

# Automated Testing

The build stage validates the website before every deployment using commands defined in `buildspec.yml`.

The automated tests:
- check that required files exist
- verify files are not empty
- validate HTML structure
- confirm CSS integration
- validate JavaScript syntax
- create CloudFront cache invalidation after successful deployment

Example validation commands:

```yaml
- node --check app.js
- grep -q "<title>" index.html
- grep -q "stylesheet" index.html
- test -f style.css
```
If any validation step fails, deployment stops automatically.

---

# Failure Monitoring and Notifications

The project includes automated monitoring using Amazon EventBridge and Amazon SNS.

When AWS CodeBuild detects a failed build:
1. EventBridge captures the failure event
2. the event is forwarded to an SNS topic
3. SNS sends a real-time email notification

This allows immediate awareness of deployment failures and improves reliability.

---

# Failure Protection

The CI/CD pipeline prevents broken code from reaching production.

If a build fails:
- deployment stops automatically
- the existing live website is not overwritten
- CloudFront continues serving the previous successful deployment
- logs remain available for debugging in CloudWatch

This demonstrates safe deployment practices and production protection.

---

# Logging and Debugging

Amazon CloudWatch logs were used to:
- inspect CodeBuild execution logs
- debug pipeline failures
- validate automated test execution
- monitor deployment behavior

---

# Security

IAM roles follow least-privilege access principles.

Each AWS service only has the permissions required for its own tasks.

Examples:
- AWS CodeBuild only has deployment access to the required S3 bucket
- CloudFront invalidation permissions are restricted to the project distribution

---

# CloudFront Benefits

CloudFront improves:
- website loading speed
- HTTPS delivery
- CDN caching performance
- reduced latency for global users
- scalability and reliability

Using CloudFront is more scalable than exposing the S3 website endpoint directly.

---

# Serverless Architecture

This solution uses a fully serverless cloud architecture:
- no EC2 instances
- no virtual machines
- no manual infrastructure management
- automatic scalability through managed AWS services

This reduces operational complexity and infrastructure maintenance.

---

# Repository Structure

```text
aws-deployment-pipeline/
├── index.html        # Portfolio website
├── style.css         # Stylesheet
├── app.js            # JavaScript functionality
├── buildspec.yml     # CodeBuild instructions and tests
└── README.md
```

---

# Challenges Encountered

Several technical challenges were solved during development:
- configuring IAM permissions
- fixing CloudFront invalidation authorization
- debugging YAML syntax issues
- implementing automated failure monitoring
- integrating EventBridge with SNS notifications
- testing failure-safe deployment behavior

---

# Results

The final solution successfully demonstrates:
- automated CI/CD deployment
- automated testing
- deployment safety
- event-driven monitoring
- real-time notifications
- scalable serverless cloud architecture
- cloud-native website hosting

---

# Author

Alexandra Bud  
Computer Science Student
