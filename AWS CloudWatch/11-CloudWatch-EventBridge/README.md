# AWS CloudWatch — EventBridge

Amazon EventBridge detects events and triggers actions.

## CloudWatch vs EventBridge

CloudWatch:
“How is the system performing?”

EventBridge:
“What happened?”

## Example

EC2 instance stopped
→ EventBridge
→ Lambda
→ Notification

## Common Events

- EC2 state changes
- ECS task changes
- S3 events
- AWS API events
- Scheduled events
- Custom application events

## Scheduled Events

EventBridge
→ Lambda
→ Execute task

## Learning Goal

Understand events, event buses, event patterns, rules, targets, and scheduled events.
