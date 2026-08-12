# AWS CloudWatch — Dashboards

CloudWatch Dashboards provide a centralized visual view of infrastructure and application health.

## Example Production Dashboard

```text
+------------------------------------------------+
|             PRODUCTION MONITORING              |
+------------------------------------------------+
| CPU           | Memory         | Requests      |
| 65%           | 72%            | 15,230/min    |
+------------------------------------------------+
| Latency       | 5XX Errors     | DB Connections|
| 210 ms        | 3              | 120           |
+------------------------------------------------+
```

## Useful Dashboard Metrics

EC2:
- CPU
- Memory
- Disk

ALB:
- Requests
- Latency
- 4XX
- 5XX

RDS:
- CPU
- Connections
- Storage
- IOPS

Application:
- Request rate
- Error rate
- Response time

## Learning Goal

Create a dashboard that answers:
1. Is the system healthy?
2. Is traffic increasing?
3. Are errors increasing?
4. Are resources saturated?
5. Is the database healthy?
