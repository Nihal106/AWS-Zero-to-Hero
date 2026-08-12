# AWS CloudWatch — Metrics

A CloudWatch metric is a numerical measurement representing the performance or behavior of a resource or application.

## Examples

EC2:
- CPUUtilization
- NetworkIn
- NetworkOut
- DiskReadOps
- DiskWriteOps
- StatusCheckFailed

ALB:
- RequestCount
- TargetResponseTime
- HTTPCode_Target_5XX_Count

RDS:
- CPUUtilization
- DatabaseConnections
- FreeStorageSpace
- ReadIOPS
- WriteIOPS

## Core Concepts

### Namespace
Groups related metrics.

Examples:
- AWS/EC2
- AWS/RDS
- AWS/Lambda
- AWS/ApplicationELB

### Dimensions
Identify the resource associated with a metric.

Example:
`InstanceId = i-123456789`

### Statistics
- Average
- Minimum
- Maximum
- Sum
- SampleCount

### Period
The time interval over which metric data is aggregated.

Examples:
- 1 minute
- 5 minutes
- 1 hour

## Custom Metrics

Applications can publish metrics such as:
- OrdersProcessed
- PaymentFailures
- ActiveUsers
- QueueLength

## Learning Goal

Understand namespaces, dimensions, statistics, periods, AWS-provided metrics, and custom metrics.
