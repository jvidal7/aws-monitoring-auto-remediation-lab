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

