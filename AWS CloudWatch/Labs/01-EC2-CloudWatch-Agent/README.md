# Lab 01 — Install CloudWatch Agent on EC2 and Monitor It

## Objective

In this lab you will:

- Launch an EC2 instance
- Create an IAM role
- Install the CloudWatch Agent
- Configure the agent
- Collect memory metrics
- Collect disk metrics
- Collect filesystem metrics
- Collect application logs
- Send telemetry to CloudWatch
- Verify metrics
- Verify logs
- Create a dashboard
- Create alarms

## Architecture

```text
EC2
 |
CloudWatch Agent
 |-------------|
 |             |
Metrics       Logs
 |             |
CloudWatch    CloudWatch Logs
 |
Dashboard
 |
Alarm
 |
SNS
 |
DevOps Engineer
```

## Prerequisites

- AWS account
- IAM permissions
- EC2 access
- SSH access
- Basic Linux knowledge

Recommended:
- Amazon Linux 2023
- t3.micro

## Step 1 — Launch EC2

Launch an EC2 instance named `cloudwatch-lab`.

Allow SSH only from your IP address.

## Step 2 — Create IAM Role

Create an EC2 IAM role and attach:

`CloudWatchAgentServerPolicy`

Attach the role to the EC2 instance.

The role allows the agent to communicate with AWS without storing access keys on the server.

## Step 3 — Connect

```bash
ssh -i key.pem ec2-user@<PUBLIC-IP>
```

Check OS:

```bash
cat /etc/os-release
```

## Step 4 — Install CloudWatch Agent

For Amazon Linux:

```bash
wget https://amazoncloudwatch-agent.s3.amazonaws.com/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm
```

Install:

```bash
sudo rpm -U ./amazon-cloudwatch-agent.rpm
```

Verify:

```bash
rpm -qa | grep amazon-cloudwatch-agent
```

## Step 5 — Create Application Log

```bash
sudo mkdir -p /var/log/myapp
sudo touch /var/log/myapp/application.log
```

Add logs:

```bash
echo "INFO Application started" | sudo tee -a /var/log/myapp/application.log
echo "INFO User login successful" | sudo tee -a /var/log/myapp/application.log
echo "ERROR Database connection failed" | sudo tee -a /var/log/myapp/application.log
```

## Step 6 — Configure Agent

Create:

```bash
sudo vi /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json
```

Use:

```json
{
  "agent": {
    "metrics_collection_interval": 60
  },
  "metrics": {
    "namespace": "CWAgent",
    "metrics_collected": {
      "mem": {
        "measurement": [
          "mem_used_percent"
        ]
      },
      "disk": {
        "measurement": [
          "used_percent"
        ],
        "resources": [
          "*"
        ]
      }
    }
  },
  "logs": {
    "logs_collected": {
      "files": {
        "collect_list": [
          {
            "file_path": "/var/log/myapp/application.log",
            "log_group_name": "/myapp/production",
            "log_stream_name": "{instance_id}"
          }
        ]
      }
    }
  }
}
```

## Step 7 — Start Agent

```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c file:/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json -s
```

## Step 8 — Check Status

```bash
sudo systemctl status amazon-cloudwatch-agent
```

Expected:

```text
Active: active (running)
```

## Step 9 — Check Agent Logs

```bash
sudo tail -f /opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log
```

## Step 10 — Generate Application Logs

```bash
for i in {1..20}
do
    echo "$(date) INFO Request processed successfully" | sudo tee -a /var/log/myapp/application.log
    sleep 2
done
```

Generate an error:

```bash
echo "$(date) ERROR Database connection failed" | sudo tee -a /var/log/myapp/application.log
```

## Step 11 — Verify Metrics

AWS Console:

CloudWatch
→ Metrics
→ All metrics
→ CWAgent

Look for:

`mem_used_percent`

and:

`disk_used_percent`

## Step 12 — Verify Logs

CloudWatch
→ Logs
→ Log groups

Open:

`/myapp/production`

You should see a log stream for the EC2 instance.

## Step 13 — Test Real-Time Logging

Terminal 1:

```bash
sudo tail -f /var/log/myapp/application.log
```

Terminal 2:

```bash
echo "$(date) INFO New request received" | sudo tee -a /var/log/myapp/application.log
```

Verify the new event appears in CloudWatch Logs.

## Step 14 — Logs Insights

Open:

CloudWatch
→ Logs Insights

Select:

`/myapp/production`

Query:

```text
fields @timestamp, @message
| sort @timestamp desc
| limit 20
```

## Step 15 — Find Errors

```text
fields @timestamp, @message
| filter @message like /ERROR/
| sort @timestamp desc
| limit 20
```

## Step 16 — Create Dashboard

Create:

`EC2-Production-Monitoring`

Add:

- CPUUtilization
- mem_used_percent
- disk_used_percent

## Step 17 — Create CPU Alarm

Metric:

`CPUUtilization`

Condition:

`Greater than 80%`

Evaluation:

`5 minutes`

Alarm name:

`EC2-High-CPU`

## Step 18 — Create Memory Alarm

Metric:

`mem_used_percent`

Condition:

`Greater than 80%`

## Step 19 — Test Monitoring

Install stress-ng:

```bash
sudo dnf install -y stress-ng
```

Generate CPU load:

```bash
stress-ng --cpu 2 --timeout 300
```

Watch the CloudWatch metric and alarm.

## Step 20 — Troubleshooting

Check agent:

```bash
sudo systemctl status amazon-cloudwatch-agent
```

Restart:

```bash
sudo systemctl restart amazon-cloudwatch-agent
```

Check agent logs:

```bash
sudo tail -100 /opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log
```

Check configuration:

```bash
sudo cat /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json
```

Verify IAM role contains:

`CloudWatchAgentServerPolicy`

## Lab Tasks

- [ ] Launch EC2
- [ ] Attach IAM role
- [ ] Install CloudWatch Agent
- [ ] Configure agent
- [ ] Collect memory metrics
- [ ] Collect disk metrics
- [ ] Collect application logs
- [ ] Verify `CWAgent` metrics
- [ ] Verify CloudWatch Logs
- [ ] Query Logs Insights
- [ ] Create dashboard
- [ ] Create CPU alarm
- [ ] Create memory alarm
- [ ] Generate CPU load
- [ ] Verify alarm
- [ ] Troubleshoot agent

## Expected Outcome

You should be able to explain:

```text
EC2
→ CloudWatch Agent
→ Metrics
→ CloudWatch
```

and:

```text
EC2 Application
→ Log File
→ CloudWatch Agent
→ CloudWatch Logs
→ Logs Insights
```
