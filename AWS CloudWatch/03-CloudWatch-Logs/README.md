# AWS CloudWatch — Logs

CloudWatch Logs is used to collect, store, search, analyze, and monitor log data.

## Why Logs?

Metrics tell us that something is wrong.

Logs help answer why it is wrong.

## CloudWatch Logs Architecture

Application
→ Log Source
→ CloudWatch Logs
→ Log Group
→ Log Stream
→ Log Events

## Log Group

A logical container for log streams.

Examples:
- `/myapp/production`
- `/aws/lambda/payment-service`
- `/ecs/my-application`

## Log Stream

A sequence of log events from a particular source.

## Log Event

An individual log entry.

Example:

`2026-08-12 10:31:05 ERROR Database connection failed`

## Common Sources

- EC2
- Lambda
- ECS
- VPC Flow Logs
- API Gateway
- CloudTrail
- Applications

## Good Logging

```text
INFO: Payment request received
INFO: Payment completed
ERROR: Database connection failed
```

Avoid unnecessarily logging:
- Passwords
- Access keys
- Tokens
- Credit card data
- Sensitive personal data
- Huge binary payloads

## Retention

Configure an appropriate retention period instead of keeping every log forever.

## Learning Goal

Understand Log Groups, Log Streams, Log Events, retention, application logging, and AWS service logging.
