# Capstone Project – AWS Django Blog Application

## Project Summary

This project demonstrates the deployment of a scalable blog application built with Django on AWS. The architecture includes:

- 🌐 EC2 instances in an Auto Scaling Group
- ⚙️ Application Load Balancer (ALB)
- 🗄️ Amazon RDS (MySQL)
- ☁️ Amazon S3 for media storage
- 🏗️ AWS VPC with public/private subnets
- 🔐 IAM, NAT Gateway, Route Tables, SSM Parameters
- 📜 Infrastructure as Code using Terraform
- 📊 Monitoring with Prometheus and Grafana

---

## Step-by-Step Deployment Guide

### 1. VPC and Networking Setup

- Created a custom VPC `sda-capstone-vpc` with CIDR block `10.90.0.0/16`
- Enabled DNS hostnames for instance communication
- Created 2 public and 2 private subnets

### 2. Security Groups

- **ALB Security Group:** Allows HTTP (80) & HTTPS (443) from anywhere
- **EC2 Security Group:** Allows traffic only from ALB on 80/443 and SSH from anywhere
- **RDS Security Group:** Accepts MySQL (3306) traffic only from EC2 Security Group

### 3. GitHub Repository Setup

- Created a private GitHub repository: `capstone`
- Generated a personal access token with `repo` scope
- Cloned the repository locally and pushed project files

### 4. SSM Parameter Store

- Stored sensitive credentials securely in AWS SSM:
  - `/myname/capstone/username` → `admin`
  - `/myname/capstone/password` → `Clarusway1234`
  - `/myname/capstone/token` → `<GitHub Token>`

### 5. RDS – MySQL Database

- Created a DB Subnet Group with private subnets
- Launched an RDS MySQL 8.0 instance named `sda-capstone-rds`
- Public access disabled for security

### 6. Create S3 Bucket

- Created an S3 bucket named: `sdacapstone-<yourname>-blog`
- Purpose: Store user-uploaded images and videos

### 7. Create NAT Gateway

- Allocated an Elastic IP and created a NAT Gateway in the public subnet
- Updated the private route table to enable internet access for private EC2 instances

### 8. Update settings.py Configuration

- Dynamically pull credentials from SSM using boto3 to secure sensitive data

### 9. Test UserData Script on EC2

- Created a temporary EC2 instance
- Verified the UserData script for cloning the repository and running the application

### 10. Launch Admin Node

- Created an EC2 instance for monitoring and management tools
- Installed Terraform, Ansible, Prometheus, and Grafana

### 11. Build Infrastructure with Terraform

- Developed Terraform configuration files
- Ran `terraform init` and `terraform apply` to deploy the architecture

### 12. Configure Ansible Dynamic Inventory

- Created a dynamic inventory to manage EC2 instances
- Tested inventory discovery using tags

### 13. Deploy Prometheus Node Exporter via Ansible

- Created an Ansible playbook to install Node Exporter on all EC2 instances
- Verified service functionality on port 9100

### 14. Prometheus Service Discovery

- Configured Prometheus to auto-discover EC2 instances using tags

### 15. Grafana Integration & Dashboard

- Connected Prometheus as a data source in Grafana
- Imported a dashboard to visualize real-time metrics

## Challenges Faced

- ❌ Incompatible AMI image for the region
- 🔐 IAM permission errors requiring adjustments
- ⚙️ Outdated or misconfigured Terraform modules
- 🔄 Coordination for SSH access among team members
- 📁 File permission issues on EC2 instances
- 🔧 Environment variables not being recognized initially

---

## Clean-up Instructions

To destroy the infrastructure, run:

```bash
cd sda-capstone-infra-tf
terraform destroy --auto-approve
```

Manually delete:
- 🗄️ RDS
- ☁️ S3 Buckets (ensure they are empty)
- 🏗️ Subnet Groups
- 🔐 IAM Roles
- 🌐 Internet Gateway & NAT Gateway
- 🔑 SSM Parameters
- 🏡 VPC


## Final Notes

This project was completed as a capstone for the Clarusway DevOps Bootcamp in 2025 by Group 2. All steps were documented, executed, and tested successfully. 🎉
