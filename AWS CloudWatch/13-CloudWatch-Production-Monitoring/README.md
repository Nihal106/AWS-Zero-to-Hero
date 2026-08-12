# AWS CloudWatch — Production Monitoring

Production monitoring combines metrics, logs, alarms, dashboards, application monitoring, and incident response.

## Production Architecture

AWS
→ ALB / EC2 / RDS
→ CloudWatch
→ Metrics / Logs / Alarms
→ SNS
→ DevOps Team

## What Should We Monitor?

### Infrastructure
- CPU
- Memory
- Disk
- Network

### Application
- Request rate
- Latency
- Errors
- Exceptions

### Load Balancer
- Requests
- 4XX
- 5XX
- Target health
- Latency

### Database
- CPU
- Connections
- Storage
- IOPS
- Latency

## Incident Workflow

Alert
→ Identify affected service
→ Check metrics
→ Check logs
→ Run Logs Insights
→ Identify root cause
→ Fix
→ Verify metrics
→ Close incident

## Learning Goal

Students should be able to troubleshoot production incidents using CloudWatch rather than simply viewing dashboards.
