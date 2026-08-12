# AWS CloudWatch — Logs Insights

CloudWatch Logs Insights is used to query and analyze CloudWatch Logs.

## Basic Query

```text
fields @timestamp, @message
| sort @timestamp desc
| limit 20
```

## Find Errors

```text
fields @timestamp, @message
| filter @message like /ERROR/
| sort @timestamp desc
| limit 50
```

## Find Exceptions

```text
fields @timestamp, @message
| filter @message like /Exception/
| sort @timestamp desc
| limit 50
```

## Count Errors

```text
fields @message
| filter @message like /ERROR/
| stats count()
```

## Real-World Troubleshooting

ALB 5XX increases
→ CloudWatch Logs
→ Logs Insights
→ Search ERROR
→ Identify root cause
→ Fix
→ Verify

## Learning Goal

Students should be able to search, filter, sort, count, and analyze application logs.
