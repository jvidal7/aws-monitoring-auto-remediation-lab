# aws-monitoring-auto-remediation-lab
AWS monitoring and auto-remediation system using CloudWatch, Lambda, and GuardDuty to detect, respond to, and mitigate performance and security issues across cloud environments.

## 3.1 Project Overview

### Overview:

#### Scenario
CloudGuard, a financial services company, recently experienced a security breach due to delayed detection of unusual system behavior. The operations team identified the issue too late, resulting in significant downtime and potential data exposure.

In response, leadership has prioritized building a proactive monitoring and automated remediation system to prevent similar incidents in the future.

---

### Solution:
This project implements a comprehensive monitoring and auto-remediation system using:
- Amazon CloudWatch for real-time monitoring and alerting  
- AWS Lambda for automated remediation  
- Amazon GuardDuty for intelligent threat detection  

The system is designed to automatically detect and respond to both performance issues and security threats across development and production environments.


---

### Project Description
In this project, I will take on the role of a **Cloud Support Engineer**.
I will:
- Configure monitoring for EC2 instances  
- Trigger automated responses to performance issues  
- Detect and investigate security threats  
- Simulate real-world incidents and respond accordingly  

---

### Project Steps:
The project is divided into the following key phases:
1. Configure EC2 environments (Development and Production)  
2. Implement custom CloudWatch monitoring  
3. Create automated remediation using AWS Lambda  
4. Enable GuardDuty and perform security incident response  

---

### Services Used
- **Amazon EC2** – Virtual servers for dev and prod environments  
- **Amazon CloudWatch** – Monitoring, metrics, alarms, and alerting  
- **AWS Lambda** – Serverless automation for remediation  
- **Amazon GuardDuty** – Threat detection and security monitoring  
- **AWS IAM** – Identity and access management  

---
## Final Result

A fully functional AWS monitoring and auto-remediation system for CloudGuard that demonstrates:

- Real-time performance monitoring using Amazon CloudWatch  
- Automated remediation workflows using AWS Lambda  
- Threat detection and security monitoring with Amazon GuardDuty  
- Incident response procedures for cloud support operations  
- Infrastructure monitoring across development and production environments  

This project provides hands-on experience with AWS monitoring, cloud security, and automated response systems used in real-world cloud environments.

### Final Architecture Diagram 

![Project Diagram](images/aws-monitoring-auto-remediation-diagram.gif)

# 3.2 Launching and Configuring EC2 Instances

## Introduction

In this phase of the project, I created separate EC2 environments to simulate real-world development and production infrastructure.

Creating multiple environments allows organizations to apply different monitoring configurations, security controls, and troubleshooting procedures while safely testing cloud operations.

---

## Launching EC2 Instances

### Development Server Configuration

Created the first EC2 instance to represent a development environment.

#### Configuration
- Name: `Dev-Server`
- Environment Tag: `Dev`
- AMI: Amazon Linux 2023
- Instance Type: `t2.micro`
- Storage: `8 GB gp3`
- Security Group: Allow SSH from my IP

### Screenshot
![Dev Server Configuration](images/dev-server-config.png)

---

### Production Server Configuration

Created the second EC2 instance to represent a production environment.

#### Configuration
- Name: `Prod-Server`
- Environment Tag: `Production`
- AMI: Amazon Linux 2023
- Instance Type: `t2.micro`
- Storage: `8 GB gp3`
- Security Group: Allow SSH from my IP

### Screenshot
![Prod Server Configuration](images/prod-server-config.png)

---

## Verifying Instance Deployment

After launching both instances, I verified they successfully entered the `Running` state in the EC2 dashboard.

### Screenshot
![Running EC2 Instances](images/both-instances-running.png)

---

# Instance Configuration for Monitoring & Troubleshooting

To simulate real-world operational issues, I installed testing utilities on both EC2 instances.

These tools will be used later to:
- Trigger CloudWatch alarms
- Simulate performance issues
- Test automated remediation workflows
- Validate monitoring configurations

---

## Development Environment Setup

### Installing the `stress` Tool

Installed the `stress` package on the development server to simulate high CPU utilization events.

### Command Used

```bash
sudo yum install stress -y
```

### Screenshot
![Installing Stress Tool](images/install-stress-tool.png)

---

### Simulating High CPU Usage

The `stress` tool generates artificial CPU load to trigger CloudWatch alarms and test monitoring workflows.

### Command Used

```bash
sudo stress --cpu 8 --timeout 300
```

### Screenshot
![CPU Stress Test](images/stress-command-running.png)

### CloudWatch Metrics

The CPU stress test successfully generated increased CPU utilization and network activity within Amazon CloudWatch metrics.

### Screenshot
![High CPU CloudWatch Metrics](images/high-cpu-cloudwatch-metrics.png)

---

## Production Environment Setup

### Installing `util-linux`

Installed the `util-linux` package on the production server to simulate disk usage issues.

### Command Used

```bash
sudo yum install util-linux -y
```

### Screenshot
![Installing util-linux](images/install-util-linux.png)

---

### Simulating Disk Space Issues

Used the `fallocate` command to quickly allocate storage space and trigger disk monitoring alerts.

### Command Used

```bash
fallocate -l 6G /home/ec2-user/fakefile
```

### Screenshot
![Disk Space Simulation](images/fallocate-command.png)

### Verifying Disk Usage

Verified the disk utilization increased successfully after creating the test file.

### Command Used

```bash
df -h
```

### Screenshot
![Disk Usage Check](images/disk-usage-check.png)

---

## Cleanup

Removed the test file to restore available disk space after testing.

### Command Used

```bash
rm /home/ec2-user/fakefile
```

### Screenshot
![Cleanup Command](images/remove-fakefile.png)

### Verifying Available Disk Space

Verified that disk utilization returned to normal after removing the test file.

### Command Used

```bash
df -h
```

### Screenshot
![Disk Space Restored](images/disk-space-restored.png)

The available disk space increased successfully after cleanup, confirming the simulated storage issue was resolved.

---

## Safety Considerations & Summary

The testing tools used throughout this project provide a safe and controlled way to simulate real-world cloud infrastructure issues without causing permanent system damage.

### Key Safety Features
- CPU stress testing automatically stops after the configured timeout period  
- Disk space can be restored immediately by removing the test file  
- No permanent changes were made to the EC2 instances  
- Simulated incidents were designed to avoid service disruption  

These controlled simulations allowed me to safely verify:
- CloudWatch monitoring functionality  
- System performance visibility  
- Infrastructure troubleshooting procedures  
- Readiness for automated remediation workflows  

---

## Outcome

Successfully configured:
- Development and production EC2 environments  
- CPU stress testing for monitoring validation  
- Disk usage simulation for alert testing  
- Infrastructure preparation for CloudWatch monitoring and automated remediation  

This phase established the foundation for implementing real-time monitoring, alerting, and automated incident response within AWS cloud environments.

---

# 3.3 Install CloudWatch Agent for Custom Metrics

## Introduction

In this phase of the project, I installed and configured the Amazon CloudWatch Agent on both EC2 instances to collect additional system metrics beyond the default EC2 monitoring data.

By default, CloudWatch provides limited metrics such as CPU utilization. Installing the CloudWatch Agent allows monitoring of:
- Memory utilization
- Disk usage
- Network performance
- System-level operational metrics

This provides improved visibility into infrastructure health and performance across both development and production environments.

---

## Installing the CloudWatch Agent

Connected to the `Dev-Server` instance and installed the Amazon CloudWatch Agent package.

### Command Used

```bash
sudo yum install amazon-cloudwatch-agent -y
```

### Screenshot
![CloudWatch Agent Installation](images/install-cloudwatch-agent.png)

---

## Running the CloudWatch Agent Configuration Wizard

Configured the CloudWatch Agent using the interactive configuration wizard.

### Command Used

```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
```

### Configuration Summary

| Configuration | Selection |
|---|---|
| Operating System | Linux |
| Environment | EC2 |
| Agent User | root |
| StatsD | No |
| CollectD | No |
| Host Metrics | Yes |
| Per-Core CPU Metrics | No |
| EC2 Dimensions | Yes |
| Aggregate EC2 Metrics | No |
| Collection Interval | 60 Seconds |
| Metrics Configuration | Standard |
| Log Monitoring | No |
| X-Ray Tracing | No |
| Store in Parameter Store | No |

![config wizard](images/cloudwatch-agent-config-wizard.png)

---

## Starting the CloudWatch Agent

After completing the configuration wizard, I started the CloudWatch Agent using the generated configuration file.

### Command Used

```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
-a fetch-config \
-m ec2 \
-c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json \
-s
```

### Screenshot
![Starting CloudWatch Agent](images/start-cloudwatch-agent.png)

---

## Verifying Agent Status

Verified the CloudWatch Agent was running successfully on the EC2 instance.

### Command Used

```bash
sudo systemctl status amazon-cloudwatch-agent
```

### Screenshot
![CloudWatch Agent Running](images/cloudwatch-agent-running.png)

Expected output:

```bash
active (running)
```

This confirms the CloudWatch Agent is actively collecting and sending metrics to Amazon CloudWatch.

---

## Repeating Configuration for Production Environment

Repeated the CloudWatch Agent installation and configuration process on the `Prod-Server` instance to ensure monitoring coverage across both environments.

Why both environments?
- Improved infrastructure visibility
- Consistent monitoring across systems
- Ability to compare operational behavior between environments
- Better incident detection and troubleshooting capabilities

### Screenshot
![Production CloudWatch Agent Setup](images/prod-cloudwatch-agent-setup.png)

---

## Outcome

Successfully configured:
- CloudWatch Agent installation on EC2 instances  
- Custom metric collection for system monitoring  
- Enhanced infrastructure visibility  
- Monitoring coverage across development and production environments  

This phase established the foundation for advanced CloudWatch alarms, automated remediation workflows, and proactive cloud infrastructure monitoring.

---

# 3.4 Creating CloudWatch Alarms and SNS Notifications

## Introduction

In this phase of the project, I configured Amazon CloudWatch alarms and Amazon SNS notifications to detect performance issues and automatically send alerts when monitoring thresholds were exceeded.

This allows cloud support teams to respond quickly to infrastructure problems before they impact production systems.

The alarms created in this project monitor:
- High CPU utilization
- High disk usage
- Infrastructure performance anomalies

---

## Creating an SNS Topic

Created an Amazon SNS topic to receive CloudWatch alarm notifications.

### Steps Performed
1. Navigated to the Amazon SNS Console  
2. Selected **Topics**  
3. Clicked **Create topic**  
4. Selected **Standard Topic**  
5. Configured the topic name:
   - `cloudguard-alerts`

### Screenshot
![SNS Topic Creation](images/sns-topic-creation.png)

---

## Creating an Email Subscription

Configured an email subscription to receive alarm notifications from Amazon SNS.

### Steps Performed
1. Opened the SNS topic  
2. Clicked **Create subscription**  
3. Selected:
   - Protocol: `Email`
4. Entered an email endpoint for notifications  
5. Confirmed the subscription through the AWS confirmation email  

### Screenshot
![SNS Email Subscription](images/sns-email-subscription.png)

---

## Creating a High CPU Alarm

Created a CloudWatch alarm to monitor CPU utilization on the `Dev-Server` instance.

### Alarm Configuration

| Setting | Value |
|---|---|
| Metric | CPUUtilization |
| Threshold | Greater than 85% |
| Evaluation Period | 5 Minutes |
| Statistic | Average |
| Notification Target | cloudguard-alerts SNS Topic |

### Steps Performed
1. Opened Amazon CloudWatch  
2. Navigated to **Alarms**  
3. Selected **Create Alarm**  
4. Chose the `CPUUtilization` metric for the EC2 instance  
5. Configured threshold conditions  
6. Attached SNS notification actions  

![cpu alarm](images/cloudwatch-cpu-alarm-created.png)

---
## Creating a Low Disk Space Alarm

Configured a CloudWatch alarm to monitor disk utilization on the `Prod-Server` instance using custom CloudWatch Agent metrics.

### Alarm Configuration

| Setting | Value |
|---|---|
| Namespace | CWAgent |
| Metric | disk_used_percent |
| Filesystem Path | `/` |
| Threshold | Greater than or Equal to 80% |
| Statistic | Average |
| Period | 1 Minute |

### Purpose

This alarm detects when disk utilization exceeds safe operational thresholds and triggers SNS notifications for rapid response.

---

## Creating the Lambda Function

Created a serverless AWS Lambda function to automatically respond to CloudWatch alarms triggered by infrastructure issues.

### Lambda Configuration

| Setting | Value |
|---|---|
| Function Name | EC2-AutoRemediation |
| Runtime | Python 3.14 |
| Architecture | x86_64 |
| Trigger Source | Amazon SNS |
| Purpose | Automated Incident Response |

### Steps Performed
1. Opened the AWS Lambda Console  
2. Selected **Create Function**  
3. Chose **Author from Scratch**  
4. Configured runtime and permissions  
5. Created the Lambda execution role  

### Screenshot
![Lambda Function Creation](images/lambda-function-created.png)

---

## Configuring Lambda Permissions

Attached IAM permissions required for the Lambda function to interact with EC2 resources and apply remediation tags.

### Permissions Added

| Permission | Purpose |
|---|---|
| AmazonEC2ReadOnlyAccess | Read EC2 instance details |
| ec2:CreateTags | Automatically tag affected instances |

### Inline Policy Created

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:CreateTags",
      "Resource": "arn:aws:ec2:*:*:instance/*"
    }
  ]
}
```

### Screenshot
![Lambda IAM Permissions](images/lambda-iam-permissions.png)

---

## Deploying the Auto-Remediation Code

Implemented Python-based remediation logic inside the Lambda function to process CloudWatch alarm events.

### What the Function Does

- Receives CloudWatch alarm notifications from SNS  
- Identifies the affected EC2 instance  
- Detects the issue type (`HighCPU` or `LowDisk`)  
- Automatically tags impacted instances  
- Logs remediation activity for monitoring and auditing  

### Technologies Used

- AWS Lambda  
- Amazon SNS  
- Amazon CloudWatch  
- Python (`boto3`)  

### Sample Lambda Logic

```python
if "HighCPU" in alarm_name:
    issue_tag = "HighCPU"
elif "LowDisk" in alarm_name:
    issue_tag = "LowDisk"

ec2.create_tags(
    Resources=[instance_id],
    Tags=[
        {
            'Key': 'Issue',
            'Value': issue_tag
        }
    ]
)
```

### Screenshot
![Lambda Function Code](images/lambda-function-code.png)

---

## Connecting Lambda to Amazon SNS

Integrated Amazon SNS with Lambda so CloudWatch alarms automatically trigger remediation workflows.

### SNS Subscription Configuration

| Setting | Value |
|---|---|
| Protocol | AWS Lambda |
| Endpoint | EC2-AutoRemediation |
| Notification Source | CloudWatch Alarms |

### Steps Performed
1. Opened the SNS Console  
2. Selected the `EC2-Alarms` topic  
3. Created a new subscription  
4. Selected **AWS Lambda** as the protocol  
5. Connected the `EC2-AutoRemediation` function  

### Screenshot
![SNS Lambda Subscription](images/sns-lambda-subscription.png)

---

## Outcome

Successfully implemented an automated remediation workflow capable of:

- Detecting infrastructure issues in real time  
- Triggering Lambda functions from CloudWatch alarms  
- Automatically tagging impacted EC2 instances  
- Integrating monitoring, alerting, and remediation services  

This phase demonstrates practical experience with event-driven cloud automation and incident response workflows using AWS serverless technologies.

---
