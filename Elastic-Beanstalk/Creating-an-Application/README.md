# Creating and Deploying an Application using AWS Elastic Beanstalk

## 📘 Objective

In this lab, we will:

- Create an AWS Elastic Beanstalk application
- Create an Elastic Beanstalk environment
- Deploy a sample application
- Understand the resources created by Elastic Beanstalk
- Access the deployed application from the browser

---

# 📋 Prerequisites

Before starting this lab, ensure you have:

- AWS Account
- IAM user with appropriate permissions
- Basic understanding of:
  - EC2
  - Security Groups
  - Load Balancer
  - Auto Scaling

---

# 🏗️ Architecture

```text
User
  ↓
Elastic Beanstalk
  ↓
Load Balancer
  ↓
EC2 Instance
  ↓
Application
```

Elastic Beanstalk automatically creates:

- EC2 instance
- Security Group
- Load Balancer (optional)
- Auto Scaling Group
- CloudWatch monitoring

---

# 🚀 Step 1 — Login to AWS Console

1. Open AWS Console
2. Search for:

```text
Elastic Beanstalk
```

3. Open the service

---

# 🚀 Step 2 — Create Application

Click:

```text
Create Application
```

---

## Application Configuration

### Application Name

```text
my-demo-app
```

---

## Environment Information

### Environment Name

```text
my-demo-env
```

---

## Domain

AWS automatically generates:

```text
my-demo-env.region.elasticbeanstalk.com
```

---

# 🚀 Step 3 — Select Platform

Under:

```text
Platform
```

Choose:

```text
Node.js
```

You may also choose:

- Python
- Docker
- Java
- PHP
- Go

---

# 🚀 Step 4 — Upload Application Code

Under:

```text
Application Code
```

Choose:

```text
Sample Application
```

Elastic Beanstalk provides a demo application.

---

# 🚀 Step 5 — Configure Presets

Choose:

```text
Single Instance
```

Used for:

- Development
- Testing

For production environments, choose:

```text
High Availability
```

---

# 🚀 Step 6 — Create Application

Click:

```text
Create Application
```

Elastic Beanstalk now starts creating resources.

---

# ⏳ Resources Created Automatically

Elastic Beanstalk automatically provisions:

| Resource | Purpose |
|---|---|
| EC2 Instance | Runs application |
| Security Group | Controls traffic |
| Auto Scaling Group | Scaling management |
| Load Balancer | Traffic distribution |
| CloudWatch | Monitoring |
| S3 Bucket | Stores application versions |

---

# 📦 Deployment Process

Elastic Beanstalk performs:

1. EC2 provisioning
2. Platform installation
3. Application deployment
4. Health checks
5. Environment startup

---

# 🌐 Step 7 — Access Application

Once environment health becomes:

```text
Green
```

Open the provided URL:

```text
http://my-demo-env.region.elasticbeanstalk.com
```

You should see the sample application page.

---

# 🔍 Step 8 — Explore Elastic Beanstalk Dashboard

Inside the environment dashboard, explore:

- Health
- Logs
- Monitoring
- Configuration
- Events

---

# 🖥️ Step 9 — View EC2 Instance

Navigate to:

```text
EC2 Console
```

You will notice Elastic Beanstalk created:

- EC2 instance
- Security Group
- Key pair (optional)

---

# ⚖️ Step 10 — View Load Balancer

If High Availability was selected:

Navigate to:

```text
EC2 → Load Balancers
```

Elastic Beanstalk automatically creates:

```text
Application Load Balancer
```

---

# 📈 Step 11 — View Auto Scaling Group

Navigate to:

```text
EC2 → Auto Scaling Groups
```

Elastic Beanstalk automatically creates scaling policies.

---

# 📊 Step 12 — Monitoring

Navigate to:

```text
CloudWatch
```

Monitor:

- CPU Utilization
- Network Traffic
- Request Count
- Instance Health

---

# 🔐 Step 13 — Security Group

Navigate to:

```text
EC2 → Security Groups
```

Verify inbound rules:

| Port | Protocol | Purpose |
|---|---|---|
| 80 | HTTP | Web traffic |
| 22 | SSH | SSH access |

---

# 🧪 Testing the Application

Test:

- Browser accessibility
- Health status
- Response time

---

# 🗂️ Understanding Environment Health

| Color | Meaning |
|---|---|
| Green | Healthy |
| Yellow | Warning |
| Red | Critical |
| Grey | Pending |

---

# 🔄 Updating the Application

To deploy new application versions:

1. Go to:

```text
Elastic Beanstalk → Upload and Deploy
```

2. Upload new ZIP file
3. Deploy application

Elastic Beanstalk automatically updates the environment.

---

# 🧹 Step 14 — Terminate Resources

To avoid AWS charges:

1. Open Elastic Beanstalk
2. Select environment
3. Click:

```text
Actions → Terminate Environment
```

---

# 📌 Important Notes

- Elastic Beanstalk uses EC2 internally
- Infrastructure is automatically managed
- Application deployment is automated
- Environment URL is auto-generated
- Monitoring is integrated with CloudWatch

---

# 💡 Best Practices

- Use High Availability for production
- Enable Auto Scaling
- Store secrets securely
- Use separate RDS databases
- Enable HTTPS using ACM

---

# ❓ Troubleshooting

## Application Not Opening

Check:

- Security Group
- Health status
- EC2 instance state

---

## Environment Health is Red

Check:

- Application logs
- Deployment logs
- Platform compatibility

---

## Deployment Failed

Verify:

- ZIP structure
- Platform version
- Application dependencies

---

# 🧠 Interview Questions

## What happens when you create an Elastic Beanstalk environment?

Elastic Beanstalk automatically provisions infrastructure such as EC2, Auto Scaling, Load Balancer, and Security Groups.

---

## Does Elastic Beanstalk create EC2 instances?

Yes.

---

## What is the difference between Single Instance and High Availability?

| Single Instance | High Availability |
|---|---|
| One EC2 instance | Multiple EC2 instances |
| No Load Balancer | Uses Load Balancer |
| Used for testing | Used for production |

---
