# AWS CloudWatch — Cost Optimization

CloudWatch is powerful, but excessive telemetry can become expensive.

## Common Cost Sources

- Log ingestion
- Log storage
- Logs Insights queries
- Custom metrics
- Dashboards
- Alarms
- Other observability features

## Dangerous Logging Pattern

```text
Application
→ Infinite loop
→ DEBUG logging
→ Huge payload
→ CloudWatch Logs
→ Large ingestion
→ Large bill
```

Avoid unnecessarily logging:
- Entire PDFs
- Base64 payloads
- Images
- Large JSON
- Passwords
- Tokens
- Credentials
- Sensitive data

## Better

```text
INFO: PDF processing started
INFO: file_id=12345
INFO: size=4.2MB
INFO: processing completed
```

## Retention

Use appropriate retention periods for each environment.

## Cost Investigation

1. Open billing
2. Identify CloudWatch usage
3. Check log ingestion
4. Check log storage
5. Check Logs Insights usage
6. Check custom metrics
7. Identify noisy applications
8. Reduce unnecessary telemetry

## Learning Goal

Understand that monitoring itself must be monitored and optimized.
