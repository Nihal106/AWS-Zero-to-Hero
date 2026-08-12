# AWS CloudWatch — Alarms

CloudWatch Alarms continuously evaluate metrics or other supported CloudWatch data against defined conditions and can take actions when thresholds are crossed.

## Example

CPUUtilization > 80%

for 5 consecutive minutes

OK → ALARM → SNS → Notification

## Alarm States

- OK
- ALARM
- INSUFFICIENT_DATA

## Alarm Components

- Metric
- Statistic
- Period
- Threshold
- Evaluation periods
- Comparison operator
- Missing-data behavior
- Alarm actions

## Common Alarms

EC2:
- High CPU
- Status check failure

ALB:
- High 5XX
- High latency

RDS:
- High CPU
- Low storage
- High connections

Lambda:
- Errors
- Duration
- Throttles

## Learning Goal

Understand alarm states, thresholds, evaluation periods, alarm actions, and SNS integration.
