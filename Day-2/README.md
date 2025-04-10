# DevOps Training - Day 2
**Date:** 28/03/2025

## Topics Covered

- 3-Tier Architecture
- Load Balancing with ELB
- Auto Scaling
- Amazon SNS
- EC2 Instance Creation and Configuration
- Security Group & Inbound Rules
- User Data Script for Web Server Setup
- MobaXterm & SSH
- Application Load Balancer (ALB) Configuration
- Target Groups
- HTTP Server Deployment

---

## 1. 3-Tier Architecture

- **Web Server**: Nginx / Apache  
- **App Server**: Tomcat  
- **Database Server**: mysql

**Additional Concepts:**
- **Load Balancer**: Distributes incoming traffic to registered targets (servers).
- **Auto Scaling**: Automatically adds/removes instances based on load.
  - Min size = 1
  - Desired size = 2
  - Max size = 4
- **CloudWatch Alarms**: Monitor metrics and trigger alarms.
- **SNS (Simple Notification Service)**: Sends notifications via email/SMS.

---

## 2. Vertical vs Horizontal Scaling

- **Vertical Scaling**: Increasing server resources (e.g., RAM, storage).
- **Horizontal Scaling**: Increasing the number of servers to handle more traffic.

---

## 3. Load Balancers

### Elastic Load Balancing (ELB)

- Distributes traffic across multiple instances.
- Ensures high availability.
- Performs health checks.

### Types of Load Balancers:

- **Application Load Balancer (ALB)**: Handles HTTP & HTTPS.
- **Network Load Balancer (NLB)**: Handles TCP & UDP.
- **Gateway Load Balancer**: Works at IP level.

---

## 4. Auto Scaling

- Dynamically adjusts the number of EC2 instances.
- Automatically registers instances with load balancer.

---

## 5. Amazon SNS

- Used for sending alerts/notifications.
- Supports various protocols including Email and SMS.
- Subscription process:
  1. Create a topic.
  2. Add a subscription (Email/SMS).
  3. Confirm the subscription via token/link.

---

## 6. EC2 Instance Setup (Mumbai Region)

### Steps:

1. Go to **EC2 > Launch Instance**.
2. Choose OS and instance type.
3. Configure **Network Settings**:
   - Create/edit **Security Group**.
   - Allow inbound rules (SSH, HTTP, HTTPS).
4. **Key Pair**: Upload existing or launch without a key.
5. Finalize and **Launch Instance**.

---

## 7. Web Server Setup on EC2

### Connect to Instance:

```bash
sudo su
yum update -y
yum install nginx -y
systemctl start nginx
```

### Open Inbound Ports:

- Navigate to Security Group settings.
- Edit Inbound Rules:
  - Allow SSH (port 22)
  - Allow HTTP (port 80)
  - Allow HTTPS (port 443)

### Access Web Server:

- Get public IP from EC2 console.
- Open in browser to view Nginx welcome page.

---

## 8. SSH via MobaXterm

- Create a new SSH session.
- Remote Host: EC2 Public IP
- Advanced SSH settings:
  - Upload private key (.pem file)
- Login as `ec2-user`

---

## 9. User Data Script for Apache Web Server

### Instance 1 Script:

```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>WELCOME TO LINUX CLASS $(hostname -f)</h1>" > /var/www/html/index.html
```

### Instance 2 Script:

```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>WELCOME TO AWS CLASS $(hostname -f)</h1>" > /var/www/html/index.html
```

---

## 10. Load Balancer and Target Group

### Create Target Group:
- Example: `alb-au-tg`

### Create Load Balancer (ALB):
- Use same security group as EC2 instances.
- Link to the created target group.
- Listener configuration:
  - Forward traffic to target group.

### Test Load Balancer:

- After ALB is active, copy its **DNS name**.
- Open in browser to verify it routes to your instances.

Example:  
`http://alb-testing-au-1768857211.ap-south-1.elb.amazonaws.com`

---

## Notes

- **Instance Termination**:
  - Select instance > Actions > Terminate
- **Placement Group**:
  - Logical grouping of instances in AWS for better networking/performance.

---

## Useful Links

- [AWS Console](https://aws.amazon.com/console/)
- [SNS Subscriptions](https://ap-south-1.console.aws.amazon.com/sns/v3/home?region=ap-south-1#/subscriptions)

