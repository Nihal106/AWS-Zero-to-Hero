AWS Elastic Beanstalk

What is AWS Elastic Beanstalk?

Amazon Web Services Elastic Beanstalk is a Platform as a Service (PaaS) offering from AWS that helps developers deploy, manage, and scale applications without manually handling the underlying infrastructure.

You simply upload your application code, and Elastic Beanstalk automatically handles:
	•	EC2 instance provisioning
	•	Load balancer setup
	•	Auto Scaling
	•	Application deployment
	•	Health monitoring
	•	Logging
	•	Security group configuration

It is mainly used for:
	•	Web applications
	•	APIs
	•	Backend services
	•	Microservices

⸻

Real-World Analogy

Imagine you want to open a restaurant.

Without Elastic Beanstalk:
	•	You buy land
	•	Build the kitchen
	•	Hire staff
	•	Install electricity
	•	Maintain everything

With Elastic Beanstalk:
	•	AWS gives you a fully managed restaurant setup
	•	You only bring the recipe (your application code)

⸻

Key Features

1. Easy Deployment

Deploy applications using:
	•	ZIP file upload
	•	Git
	•	CI/CD pipelines
	•	Docker containers

Supported languages:
	•	Java
	•	Python
	•	Node.js
	•	PHP
	•	Go
	•	Ruby
	•	.NET
	•	Docker

⸻

2. Auto Scaling

Elastic Beanstalk automatically:
	•	Adds EC2 instances during high traffic
	•	Removes instances during low traffic

Uses:
	•	Auto Scaling Groups

⸻

3. Load Balancing

Automatically creates:
	•	Application Load Balancer (ALB)

Traffic is distributed among multiple EC2 instances.

⸻

4. Health Monitoring

Elastic Beanstalk continuously checks:
	•	Application health
	•	Instance health
	•	Response time
	•	Errors

Health statuses:
	•	Green → Healthy
	•	Yellow → Warning
	•	Red → Critical

⸻

5. Monitoring

Integrated with:
	•	Amazon CloudWatch

Tracks:
	•	CPU utilization
	•	Network traffic
	•	Memory usage
	•	Request count

⸻

6. Managed Infrastructure

AWS automatically manages:
	•	EC2
	•	Auto Scaling
	•	Load Balancer
	•	Security Groups
	•	Deployment process

⸻

Important Components

1. Application

Top-level container in Elastic Beanstalk.

Example:

Application Name: ecommerce-app

Contains:
	•	Multiple environments
	•	Different versions

⸻

2. Environment

An environment is the actual deployed infrastructure.

Example:

Dev Environment
Test Environment
Production Environment

Each environment contains:
	•	EC2 instances
	•	Load balancer
	•	Auto Scaling group

⸻

3. Environment Tier

Two types:

Web Server Environment

Used for:
	•	Websites
	•	APIs
	•	Frontend apps

Processes HTTP/HTTPS requests.

⸻

Worker Environment

Used for:
	•	Background jobs
	•	Queue processing

Works with:
	•	Amazon Simple Queue Service (SQS)

⸻

4. Application Version

Each uploaded code package becomes a version.

Example:

v1
v2
v3

You can:
	•	Roll back
	•	Compare versions
	•	Redeploy old versions

⸻

5. Platform

Defines runtime environment.

Examples:
	•	Python 3.12
	•	Node.js 20
	•	Docker
	•	Tomcat

⸻

Elastic Beanstalk Architecture

User
   ↓
Load Balancer
   ↓
EC2 Instances
   ↓
Application

Behind the scenes AWS creates:
	•	EC2
	•	ALB
	•	Auto Scaling Group
	•	Security Groups
	•	CloudWatch
	•	IAM roles

⸻

Deployment Workflow

Step 1 — Create Application

Create:

Application → ecommerce-app


⸻

Step 2 — Create Environment

Choose:
	•	Web server
	•	Worker

Select platform:

Python / Node.js / Docker


⸻

Step 3 — Upload Code

Upload:

ZIP file

Or connect:
	•	GitHub
	•	CodePipeline

⸻

Step 4 — Elastic Beanstalk Creates Resources

Automatically provisions:
	•	EC2
	•	ALB
	•	Auto Scaling
	•	Security Groups

⸻

Step 5 — Access Application

AWS provides:

http://app-name.region.elasticbeanstalk.com


⸻

Elastic Beanstalk Deployment Policies

1. All at Once

Updates all instances simultaneously.

Pros
	•	Fast

Cons
	•	Downtime possible

⸻

2. Rolling

Updates instances in batches.

Pros
	•	Reduced downtime

Cons
	•	Slower deployment

⸻

3. Rolling with Additional Batch

Adds extra temporary instances before deployment.

Pros
	•	Better availability

Cons
	•	Higher cost

⸻

4. Immutable Deployment

Creates new Auto Scaling group with new instances.

Pros
	•	Safest deployment

Cons
	•	Most expensive

⸻

5. Blue/Green Deployment

Two separate environments:
	•	Blue → Old version
	•	Green → New version

Traffic switches after testing.

Pros
	•	Near-zero downtime
	•	Easy rollback

Cons
	•	Double infrastructure temporarily

⸻

Environment Types

Single Instance Environment

Architecture:

User → EC2

Used for:
	•	Development
	•	Testing

No load balancer.

⸻

Load Balanced Environment

Architecture:

User → ALB → Multiple EC2

Used for:
	•	Production

Supports:
	•	Auto scaling
	•	High availability

⸻

Elastic Beanstalk and Docker

Elastic Beanstalk supports:
	•	Single container Docker
	•	Multi-container Docker

Can deploy:

Dockerfile
docker-compose.yml

Uses:
	•	EC2 Docker platform

⸻

Elastic Beanstalk vs EC2

Feature	EC2	Elastic Beanstalk
Infrastructure management	Manual	Automatic
Scaling	Manual	Automatic
Load balancer setup	Manual	Automatic
Monitoring	Manual	Built-in
Flexibility	Very high	Medium
Learning curve	Higher	Easier


⸻

Elastic Beanstalk vs ECS

Feature	Elastic Beanstalk	ECS
Complexity	Easy	Medium
Container orchestration	Limited	Advanced
Best for	Simple apps	Microservices
Infrastructure control	Medium	High


⸻

Elastic Beanstalk vs Lambda

Feature	Elastic Beanstalk	Lambda
Server management	Managed	Fully serverless
Runtime duration	Long-running	Short-lived
Scaling	Auto Scaling	Instant scaling
Best for	Web apps	Event-driven tasks


⸻

Important AWS Services Used

1. Amazon Elastic Compute Cloud (EC2)

Runs application servers.

⸻

2. Elastic Load Balancing

Distributes traffic.

⸻

3. Amazon EC2 Auto Scaling

Automatically scales instances.

⸻

4. Amazon Simple Storage Service (S3)

Stores:
	•	Application versions
	•	Logs

⸻

5. AWS Identity and Access Management

Controls permissions.

⸻

6. Amazon Relational Database Service (RDS)

Optional database integration.

⸻

Configuration Files

.ebextensions

Used for:
	•	Custom configuration
	•	Package installation
	•	Environment variables

Example:

option_settings:
  aws:autoscaling:launchconfiguration:
    InstanceType: t3.micro


⸻

Environment Variables

Used for:
	•	Database credentials
	•	API keys
	•	Application configs

Example:

DB_HOST=mysql.example.com


⸻

Logging

View logs from:
	•	Elastic Beanstalk console
	•	EC2 instance

Commands:

eb logs

Logs include:
	•	Nginx logs
	•	Application logs
	•	Deployment logs

⸻

EB CLI (Elastic Beanstalk CLI)

Install:

pip install awsebcli

Common commands:

Initialize

eb init

Create environment

eb create

Deploy

eb deploy

Open app

eb open

View logs

eb logs

Terminate environment

eb terminate


⸻

Scaling Configuration

You can configure:
	•	Minimum instances
	•	Maximum instances
	•	CPU thresholds

Example:

Min: 2
Max: 6
Scale out at 70% CPU


⸻

Security in Elastic Beanstalk

IAM Roles

Used by:
	•	EC2 instances
	•	Elastic Beanstalk service

⸻

Security Groups

Controls:
	•	Inbound traffic
	•	Outbound traffic

Example:

Allow:
80 → HTTP
443 → HTTPS
22 → SSH


⸻

Database Options

Option 1 — Attach RDS inside Elastic Beanstalk

Pros
	•	Easy setup

Cons
	•	Database deleted if environment deleted

⸻

Option 2 — Separate RDS (Recommended)

Pros
	•	Safer
	•	Persistent

Cons
	•	Slightly more setup

⸻

Advantages

Easy to Use

No deep infrastructure management required.

Fast Deployment

Applications can be deployed quickly.

Managed Scaling

Auto Scaling handled automatically.

Monitoring Included

Health dashboard and CloudWatch integration.

Supports CI/CD

Works with:
	•	GitHub Actions
	•	Jenkins
	•	CodePipeline

⸻

Disadvantages

Less Infrastructure Control

Compared to raw EC2.

Not Ideal for Complex Microservices

Better to use:
	•	ECS
	•	EKS

Hidden AWS Resources

AWS creates resources automatically, which can confuse beginners.

⸻

Common Interview Questions

What is Elastic Beanstalk?

A PaaS service that deploys and manages applications automatically.

⸻

Does Elastic Beanstalk use EC2 internally?

Yes. It provisions EC2 instances automatically.

⸻

Can we SSH into Elastic Beanstalk EC2 instances?

Yes.

Command:

eb ssh


⸻

What is the difference between Elastic Beanstalk and CloudFormation?

Elastic Beanstalk	CloudFormation
Application deployment platform	Infrastructure as Code
Easier	More flexible
Managed app hosting	Full infrastructure provisioning


⸻

Does Elastic Beanstalk support Docker?

Yes.

Supports:
	•	Single container
	•	Multi-container Docker

⸻

What happens when traffic increases?

Elastic Beanstalk uses Auto Scaling to launch additional EC2 instances.

⸻

Real-World Use Cases

E-Commerce Website
	•	Frontend hosted in Beanstalk
	•	Auto scaling during sales

⸻

REST API Hosting

Deploy:
	•	Node.js APIs
	•	Python Flask apps
	•	Java Spring Boot apps

⸻

Startup MVP

Quick deployment without managing infrastructure.

⸻

Internal Company Portal

Simple management and monitoring.

⸻

Best Practices

Use Separate RDS

Avoid data loss.

⸻

Enable Auto Scaling

Improves availability.

⸻

Use Blue/Green Deployment

Safer production deployments.

⸻

Store Secrets Securely

Use:
	•	AWS Secrets Manager
	•	Parameter Store

⸻

Enable HTTPS

Use:
	•	ACM certificates
	•	Load balancer SSL termination

⸻

Sample Architecture

Users
  ↓
Application Load Balancer
  ↓
Auto Scaling Group
  ↓
EC2 Instances
  ↓
RDS Database


⸻

Important Tips

Remember:
	•	Elastic Beanstalk = PaaS
	•	Uses EC2 internally
	•	Automatically handles scaling and load balancing
	•	Supports multiple programming languages
	•	Supports Docker
	•	Can perform Blue/Green deployments
	•	Uses CloudWatch for monitoring

⸻

