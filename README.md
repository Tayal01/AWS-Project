# AWS-Project

AWS Production Deployment Project – Phase 1 (Ubuntu Version)
Highly Available & Auto-Scalable Web Application on AWS
Project Overview
This project demonstrates a production-ready deployment architecture for a static web application on AWS using Ubuntu Server.

Repository:
https://github.com/Mayank1242/aws-session2.git

This is Phase 1 (Initial Deployment).
Advanced capabilities such as CI/CD, HTTPS, Monitoring, Infrastructure as Code, and Security Enhancements will be added in upcoming phases.

Technology Stack
AWS Ubuntu Nginx EC2 ALB AutoScaling

Architecture Overview (Phase 1)
                 ┌────────────────────────────┐
                 │   Application Load Balancer │
                 │        (Multi-AZ)           │
                 └─────────────┬──────────────┘
                               │
                     ┌─────────┴─────────┐
                     │                   │
            ┌────────▼───────┐   ┌───────▼────────┐
            │  EC2 Instance  │   │  EC2 Instance   │
            │  Ubuntu + Nginx│   │ Ubuntu + Nginx  │
            └────────┬───────┘   └────────┬────────┘
                     │                    │
                     └──── Auto Scaling Group ────┘
                          (Target Tracking Policy)
Project Objective
Design and implement a highly available and auto-scalable web infrastructure using:

Ubuntu EC2 Instances
Nginx Web Server
Custom AMI
Launch Template
Application Load Balancer (ALB)
Auto Scaling Group (Target Tracking Policy)
Security Groups
AWS Services Used
Amazon EC2 – Compute infrastructure
Custom AMI – Reusable production-ready image
Launch Template – Standardized instance configuration
Application Load Balancer (ALB) – Traffic distribution
Target Group – Health monitoring
Auto Scaling Group (ASG) – Auto scaling & self-healing
Security Groups – Firewall configuration
Implementation Steps
Step 1: Launch EC2 Instance (Ubuntu)
AMI: Ubuntu Server 22.04 LTS
Instance Type: t2.micro
Configure Key Pair
Configure Security Group:
Allow SSH (Port 22)
Allow HTTP (Port 80)
Step 2: Install Nginx
sudo apt update -y
sudo apt upgrade -y
sudo apt install nginx -y
Start and enable Nginx:

sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
Step 3: Deploy Website
Install Git:

sudo apt install git -y
Clone repository:

git clone https://github.com/Mayank1242/aws-session2.git
Replace default Nginx content:

sudo rm -rf /var/www/html/*
sudo cp -r aws-session2/* /var/www/html/
sudo systemctl restart nginx
Access application:

http://<EC2-Public-IP>
Step 4: Create Custom AMI
Select configured EC2 instance
Click Actions → Image → Create Image
Name: Ubuntu-Nginx-WebServer-AMI
Wait until AMI status becomes Available
Step 5: Create Launch Template
AMI: MY AMI
Instance Type: t2.micro
Key Pair: Required
Security Group:
Port 22 (SSH)
Port 80 (HTTP)
Step 6: Create Target Group
Target Type: Instance
Protocol: HTTP
Port: 80
Health Check Path: /
Ensure instances show Healthy
Step 7: Create Application Load Balancer
Type: Application Load Balancer
Scheme: Internet-facing
Listener: HTTP (Port 80)
Select at least 2 Availability Zones
Attach Target Group
Copy the Load Balancer DNS Name and verify website accessibility.

Step 8: Create Auto Scaling Group
Using Launch Template:

Minimum Capacity: 1
Desired Capacity: 2
Maximum Capacity: 3
Attach to Target Group
Scaling Policy Configuration
Policy Type: Target Tracking Policy
Metric: Average CPU Utilization
Target Value: 50%
Auto Scaling Behavior
Scale Out → When CPU > 50%
Scale In → When CPU < 50%
Self-healing enabled → Failed instances automatically replaced
Testing Requirements
High Availability Test
Access website via Load Balancer DNS
Manually terminate one EC2 instance
Verify:
Auto Scaling launches replacement instance
Target Group health becomes Healthy
Website remains accessible
Security Configuration
Type	Port	Source
SSH	22	Your IP
HTTP	80	0.0.0.0/0
Only required ports open
No unnecessary inbound rules
Instances secured behind Load Balancer
Production Best Practices Implemented
Multi-AZ deployment
Load-balanced architecture
Self-healing infrastructure
Target Tracking auto scaling
Custom AMI for consistency
Launch Template standardization
