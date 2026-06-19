# Deployment Guide

## Step 1: Create VPC

* Created a custom VPC with CIDR block `10.0.0.0/16`
* Enabled DNS hostnames and DNS resolution

---

## Step 2: Create Subnets

Created six subnets across three Availability Zones:

### Public Subnets

* Public Subnet AZ-1
* Public Subnet AZ-2
* Public Subnet AZ-3

### Private Subnets

* Private Subnet AZ-1
* Private Subnet AZ-2
* Private Subnet AZ-3

---

## Step 3: Configure Internet Gateway

* Created an Internet Gateway
* Attached it to the VPC

---

## Step 4: Configure Route Tables

### Public Route Table

Route:

```text
0.0.0.0/0 → Internet Gateway
```

Associated with all public subnets.

### Private Route Tables

Route:

```text
0.0.0.0/0 → NAT Gateway
```

Associated with private subnets.

---

## Step 5: Create NAT Gateway

* Created NAT Gateway in a public subnet
* Allocated an Elastic IP
* Enabled internet access for private subnets

---

## Step 6: Launch EC2 Instance

Launched an EC2 instance in Public Subnet AZ-1.

Installed Apache:

```bash
sudo apt update
sudo apt install apache2 -y
```

Deployed the StreamFlix application.

---

## Step 7: Create Amazon Machine Image (AMI)

Created an AMI from the configured EC2 instance.

Purpose:

* Faster deployments
* Consistent server configuration

---

## Step 8: Create Launch Template

Configured:

* AMI
* Instance Type
* Security Group
* Key Pair

---

## Step 9: Create Target Group

Configured:

* Protocol: HTTP
* Port: 80
* Health Check Path: /

---

## Step 10: Create Application Load Balancer

Configured:

* Internet-facing ALB
* Attached all public subnets
* Associated target group

---

## Step 11: Create Auto Scaling Group

Configuration:

| Parameter        | Value |
| ---------------- | ----- |
| Desired Capacity | 3     |
| Minimum Capacity | 3     |
| Maximum Capacity | 6     |

---

## Step 12: Configure Scaling Policy

Target Tracking Policy:

| Metric          | Value |
| --------------- | ----- |
| CPU Utilization | 50%   |

When average CPU utilization exceeds 50%, Auto Scaling automatically launches additional EC2 instances.

---

## Outcome

Successfully deployed a highly available and scalable StreamFlix web application using AWS networking, load balancing, and auto scaling services.
