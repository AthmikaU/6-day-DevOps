# DevOps Training - Day 3  
**Date:** 29/03/2025

## Topics Covered
- Auto Scaling - Hands-On

### Steps:

1. Go to EC2 → Instances → Launch Template → Create Launch Template  
   - Give a name  
   - Provide auto scaling  
   - Choose Amazon Linux  
   - Instance type: `t2.micro`  
   - No key pair  
   - Create launch template

2. EC2 Dashboard  
   - Auto Scaling Group → Create → Name: `athmika-asg`  
   - Choose launch template → Next  
   - Select any 3 Availability Zones → Next (x2)  
   - Health check: `60s`  
   - Scaling settings:  
     - Desired: `4`  
     - Min: `1`  
     - Max: `6`  
   - Next (x3)  
   - Review and Create Auto Scaling instance  
   → Instance is automatically created  

3. To delete instance:  
   - Select instance → Actions → Terminate  

---

## MULTI-FACTOR AUTHENTICATION (MFA)
---

1. Download **Microsoft Authenticator** on mobile  
2. Go to EC2 Website → Top Right Corner → Security Credentials  
3. Assign MFA  
   - Device: Mobile  
   - Authenticator App → Next → Generate QR code  
   - Scan QR code  
   - Enter 2 consecutive codes  
   - Logout & Login again with MFA  
   - Go back to credentials and remove MFA (if needed)

---

## SIMPLE STORAGE SERVICE (S3)
---

### Storage Types in AWS:

1. **Object Storage**: S3 (Simple Storage Service)  
   - Durable, scalable  
   - Pay-as-you-go  
   - Stores files/images/videos/CSVs/PDFs/folders  
   - 5 TB max per bucket  
   - Max 100 buckets/account  
   - Buckets can be public or private

### S3 Bucket Naming Rules:
- Globally unique  
- 3-63 characters  
- No uppercase/special characters/spaces  

### Steps:

1. Search `S3` → Create Bucket  
   - Enable Versioning (for restoration)  
2. Open bucket → Create folder → Upload images  
3. Select image → Copy URL → Test in browser (should be private)  
4. To make public:  
   - Bucket → Permissions → Deselect "Block Public Access"  
   - Save  
   - Copy ARN → Go to [AWS Policy Generator](https://awspolicygen.s3.amazonaws.com/policygen.html)  
     - Policy Type: S3  
     - Actions: `*`  
     - Paste ARN: `arn:aws:s3:::your-bucket-name/*`  
     - Generate Policy and paste in Bucket Policy  

5. To delete bucket:  
   - Empty bucket first → Then delete

---

## AWS CLI (Command Line Interface)
---

### Installation:

```sh
# Run as Administrator in CMD
msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
```

### Verify Version

```sh
aws --version
# Output:
# aws-cli/2.x.x Python/3.x.x Windows/11 exe/AMD64
```

### Configuration

```sh
aws configure
```

Use Access Key and Secret from AWS > Security Credentials

**Example:**

```txt
AWS Access Key ID [None]: AKIAXXXXXXXX
AWS Secret Access Key [None]: abcdefgXXXXXXX
Default region name [None]: ap-south-1
Default output format [None]: text
```

### Create/Delete S3 Bucket

```sh
aws s3api create-bucket --bucket athmika2 --region ap-south-1 --create-bucket-configuration LocationConstraint=ap-south-1
aws s3 ls
aws s3api delete-bucket --bucket athmika2 --region ap-south-1
aws s3 ls
```

---

## TERRAFORM

### Installation

Download from: [https://developer.hashicorp.com/terraform/install](https://developer.hashicorp.com/terraform/install)

### Setup (Git Bash)

```sh
terraform init
touch athmika.tf
```

### Validate

```sh
terraform validate
```

### Example Configuration

```hcl
provider "aws" {
  region = "ap-south-1"
}

resource "aws_s3_bucket" "my_bucket" {
  bucket = "athmika22"
  acl    = "private"
}

resource "aws_instance" "my_instance" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  key_name      = "" # Optional: Replace if you have a key

  tags = {
    Name = "MyEC2Instance"
  }
}
```

---

## DOCKER

### Use Case

> Break monolithic apps into microservices

---

### EC2 Setup

1. Create EC2 (Amazon Linux)  
2. SSH Connect  
3. Run the following:

```sh
sudo su
yum update
yum install docker -y
docker --version
service docker start
docker info
docker pull ubuntu
docker pull redis
docker pull alpine
```

---

### Docker Commands

```sh
docker images        # List images
docker ps            # Running containers
docker ps -a         # All containers

docker run ubuntu
docker run redis
docker run alpine

docker run -it ubuntu   # Interactive Ubuntu shell
exit                    # Exit the container

docker start <container_name>
docker attach <container_name>
```

---

### Example Output (abbreviated)

```txt
Docker version 25.0.8, build 0bab007

Images:
- ubuntu:latest
- redis:latest
- alpine:latest

Containers:
- hopeful_sammet (ubuntu)
- naughty_kapitsa (redis)
- funny_meninsky (alpine)
```

---

## Docker Desktop (Windows)

### Download

[Docker Desktop for Windows (x86_64)](https://docs.docker.com/desktop/setup/install/windows-install/)
