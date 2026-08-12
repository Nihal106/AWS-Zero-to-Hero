# AWS CloudWatch — Container Insights

Container Insights provides observability for container environments such as ECS and EKS.

## What Can Be Monitored?

Cluster:
- CPU
- Memory
- Network

Nodes:
- CPU
- Memory
- Disk

Pods/Tasks:
- CPU
- Memory
- Resource utilization

Containers:
- Resource utilization
- Application logs

## Example Incident

Pod CPU reaches 95%.

CloudWatch
→ Container metrics
→ Identify pod
→ Check logs
→ Find application issue
→ Fix deployment

## Learning Goal

Understand container monitoring, ECS monitoring, EKS monitoring, container metrics, and container logs.
