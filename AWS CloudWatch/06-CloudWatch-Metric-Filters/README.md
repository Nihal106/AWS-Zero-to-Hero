# AWS CloudWatch — Metric Filters

Metric Filters search CloudWatch Logs for patterns and publish matching results as CloudWatch metrics.

## Architecture

Application
→ CloudWatch Logs
→ Metric Filter
→ CloudWatch Metric
→ Alarm
→ SNS

## Example

Logs:

```text
ERROR Payment failed
INFO Payment successful
ERROR Database unavailable
```

Filter:

```text
ERROR
```

Result:

```text
ErrorCount = 2
```

## Use Cases

- Application errors
- Authentication failures
- Failed payments
- HTTP errors
- Security events
- Database failures

## Learning Goal

Understand log pattern matching, metric filters, and creating alarms from filtered metrics.
