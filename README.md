# AWS Web Application Deployment with ALB + ASG + S3

## Project Overview
This project shows how to deploy a highly available static website using AWS services.  
The static content (HTML and images) is hosted in **S3**, served through **NGINX web servers** launched via an **Auto Scaling Group**, and traffic is distributed using an **Application Load Balancer (ALB)**.

---

## Deployment Steps

### 1- S3 Static Website Hosting
- **Bucket Name:** `maha-clarusway-assets` (Region: `eu-north-1`)
- **Static Hosting:** Enabled
- **Files Uploaded:**
  - `index.html`
  - `logo.png`
  - `sda.png`
- **Bucket Policy:** Configured to allow public read access

---

### 2- Launch Template & Auto Scaling Group
- **Launch Template:**
  - Name: `maha-launch-template`
  - AMI: Amazon Linux 2
  - Instance Type: `t3.micro`
  - IAM Role: `EC2-S3ReadOnlyRole`
  - User Data: Install & configure NGINX and copy website files from S3

- **Auto Scaling Group:**
  - Name: `clarusway-asg`
  - VPC & Subnets: Default, all availability zones
  - Capacity: Min = 1, Desired = 2, Max = 3
  - Health Checks: EC2 + ELB

---

### 3- Application Load Balancer (ALB)
- **ALB Name:** `clarusway-alb`
- **Scheme:** Internet-facing
- **Listener:** HTTP on port 80
- **Network:** Same VPC/Subnets as ASG

- **Target Group:**
  - Type: Instance
  - Port: 80
  - Health Check Path: `/`

- **Target Group Association:** Linked to the Auto Scaling Group

---

##  Screenshots (in `/screenshots` folder)
- `s3-bucket-files.png`
- `static-hosting-settings.png`
- `bucket-policy.png`
- `ec2-auto-scaling.png`
- `alb-dns-browser.png`
- `curl-200-ok.png`

---

## Result
After completing the deployment:
- The static website is accessible via the ALB DNS.
- Auto Scaling ensures high availability.
- The content is consistently pulled from the S3 bucket using user data.