# DevOps Training - Day 1  
**Date:** 27/03/2025

## Topics Covered

- Introduction to DevOps
- Cloud Computing & Providers
- Operating Systems & Server Components
- EC2 Instance Setup (AWS)
- Basic Linux Commands
- CI/CD Concepts
- DevOps Tools Overview:
  - Terraform
  - Jenkins
  - Docker
  - Kubernetes
  - Ansible
  - Nagios

---

## What is an Operating System?
An OS acts as an interface between hardware and software.

---

## Cloud Computing

- **Definition**: On-demand delivery of IT resources over the internet.
- **Features**: Storage, databases, compute, networking, security.
- **Billing**: Pay-as-you-go model.
- **Popular Cloud Providers**:
  - AWS
  - Microsoft Azure
  - Google Cloud Platform

---

## SDLC (Software Development Life Cycle)

- Structured process to develop high-quality, cost-effective, and secure software.
- Models:
  - Waterfall Model
  - Agile Methodology

---

## Waterfall Model ::
- It has distinct goals for each phase of development.
- Requirements, Analysis, Design, Coding, Testing, Operations
- has limitations(5) ::
	1. Once an application is in testing stage, it is very difficult to go back and change something that was not well-thought out in the concept stage.

## Agile Methodology ::
- DevOps is a culture, fostering collaboration amongst all participants involved in the development and maintenance of software. Agile can be described as a development methodology designed to maintain productivity and drive releases with the common reality of changing needs.
- Plan, (Design, Develop, Test, Deploy, Review), Launch
- (cycle)

- example of limitation: 
- dev team : code works in laptop
- op team : there is some problem w code, does not work in production

---

## DevOps

- A set of practices and philosophies to bridge the gap between software development and IT operations.
- Goals:
  - Shorten SDLC
  - Improve collaboration
  - Faster and efficient delivery
- Key Elements:
  - Automation
  - Continuous Integration (CI)
  - Continuous Deployment (CD)
  - Collaboration
  - Monitoring

---

## Why DevOps Matters

- Enables faster development of new products
- Easier maintenance of existing deployments

---

## Benefits of DevOps

- Speed
- Rapid Delivery
- Reliability
- Improved Collaboration
- Scalability
- Security

---

## CI/CD Pipeline

1. Continuous Development (Plan, Code)
2. Continuous Testing (Build, Test)
3. Continuous Deployment (Release, Deploy)
4. Continuous Monitoring (Operate, Monitor)

---

## Server Basics

- **Definition**: A server is a powerful system that provides services/data to clients over a network.
- **Server Components**:
  - OS (Linux/Windows/Mac)
  - RAM
  - CPU
  - Storage (HDD/SSD)
  - NIC (Network Interface Card)
- **Types of Servers**:
  1. Web Server (e.g., Nginx)
  2. File Server
  3. Database Server (e.g., MySQL, MongoDB)
  4. Mail Server (e.g., Microsoft Exchange)

---

## Basic Linux Commands

- `sudo su` - Become superuser
- `ls` - List files/directories
- `touch filename` - Create empty file
- `cat > filename` - Write content to file
- `cat filename` - Read file content
- `rm filename` - Remove file
- `mkdir dirname` - Create directory
- `cd dirname` - Change directory
- `cd ..` - Go up one directory
- `rmdir dirname` - Remove empty directory
- `rm -r dirname` - Remove directory and contents
- `ls -l` - List with permissions
- `pwd` - Print working directory
- `history` - Show command history
- `cd ../..` - Go up two levels

---

## EC2 (Elastic Compute Cloud) - AWS

### EC2 Configuration Steps

1. Open AWS Console
2. Launch instance, name the server
3. Choose Application & OS Image (Amazon Machine Image)
4. Choose instance type (RAM, CPU)
5. Key Pair (.pem) - used for secure connection
6. Configure networking:
   - VPC, subnets, route tables
   - NAT, Internet Gateway, Elastic IPs
7. Configure storage:
    - EBS (Elastic Block Store)
    - EBS is similar to hard disk
	- Persistant Block level storage
	- Can create new volumes and attach to running instance
8. Launch the instance : web server / windows server
9. Connect using RDP (Windows) or SSH
    - download remote desk file, get password.
    - git bash => ssh ec2@
    
---

## Git & GitHub Integration from EC2

### Install Git and Push Files

```bash
sudo yum update
sudo yum install git -y
touch file
sudo su
git init
git add .
git commit -m "Initial commit"
git remote add origin <repo-url>
git push origin master
```

---

## Generating GitHub Token

1. Go to GitHub Settings > Developer Settings > Personal Access Tokens
2. Generate new token (Classic)
3. Assign scopes (e.g., repo)
4. Use token in place of password for HTTPS push

---

