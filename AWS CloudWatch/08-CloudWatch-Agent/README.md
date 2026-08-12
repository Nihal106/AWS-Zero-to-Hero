# AWS CloudWatch — CloudWatch Agent

The unified CloudWatch Agent is used to collect additional system-level metrics and logs from servers such as EC2 instances.

## Why Do We Need the Agent?

Default EC2 metrics include CPU, network, disk operations, and status checks.

The agent can additionally collect:
- Memory
- Filesystem utilization
- Swap
- Process metrics
- Application logs

## Architecture

EC2
→ CloudWatch Agent
→ Metrics / Logs
→ CloudWatch

## Common Configuration Sections

- agent
- metrics
- logs
- traces

## Default Namespace

Agent metrics normally appear under:

`CWAgent`

## Installation Methods

- Command line
- AWS Systems Manager
- CloudWatch console
- EC2 console workflows

## Important

Use the unified CloudWatch Agent. The older CloudWatch Logs agent is deprecated.

## Learning Goal

Understand installation, IAM permissions, configuration, metrics collection, log collection, and troubleshooting.
