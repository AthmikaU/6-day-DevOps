# Day 5: 02/04/2025

---

## Linux File Permissions

In Linux, each file has three types of permissions:

- **User**: Owner of the file
- **Group**: A collection of users with similar permissions
- **Others**: Everyone else (not the user and not in the group)

### Permission Types

- **Read (r)** = 4
- **Write (w)** = 2
- **Execute (x)** = 1

The sum of these values gives the numeric representation of permissions.

### Viewing Permissions

```sh
ls -l
```

This command displays the detailed list of files and their permissions.

### File Types

- **Regular file**: Begins with `-`
- **Directory**: Begins with `d`

### Example:

```
-rw-r--r--
```

- `-` : Regular file  
- `rw-` : User (read + write = 4 + 2 = 6)  
- `r--` : Group (read only = 4)  
- `r--` : Others (read only = 4)  

**Total Permission Value: 644**

Common Values:
- `777` : Full permissions for user, group, and others
- `700` : Full permission only for the owner

### Permission Commands in Practice

```sh
sudo su
touch f1
mkdir n1
ls -l
chmod 744 f1
chmod 044 f1
chmod 001 f1
chmod 070 f1
ls -l
```

### Example Output

```sh
-rwxr--r--. 1 root root 0 Apr  2 08:49 f1
drwxr-xr-x. 2 root root 6 Apr  2 08:49 n1

----r--r--. 1 root root 0 Apr  2 08:49 f1

---------x. 1 root root 0 Apr  2 08:49 f1

----rwx---. 1 root root 0 Apr  2 08:49 f1
```

---

## Kubernetes (EKS)

### Cluster Creation

1. Go to AWS Console → Search for `EKS`
2. Create a Cluster → Custom Configuration
3. Provide a cluster name
4. Create a new IAM role for the cluster

### IAM Role Setup

- AWS Service: EKS
- Use case: EKS - Cluster
- Add necessary permissions

Once created, the cluster can be accessed via the EKS console under **Resources** to view pods and deployments.

### Launch Master Node

1. Launch a new EC2 instance (Amazon Linux)  
2. In Advanced Settings:
   - Attach the IAM role created for EKS cluster
   - Use an existing security group with proper rules

3. Connect to EC2 instance via SSH  
4. Follow `kubectl` setup steps from previous sessions  
5. Once configured, you will see 2 pods in the EKS resources section

---

## Jenkins (CI/CD)

Jenkins is an open-source automation server used to implement CI/CD pipelines. It automates tasks such as building, testing, and deploying applications. It is developed in Java.

---

## Jenkins Installation on EC2 (Ubuntu)

### Step 1: Create EC2 Instance

- AMI: Ubuntu (t2.micro)
- Security Group: `jenkins-athmika-sg`
  - Allow HTTP, HTTPS, SSH (Anywhere)
  - Add a new rule for **All Traffic** (for demo/testing)
- Launch the instance

### Step 2: Connect to Instance

Connect using SSH:

```sh
ssh -i "your-key.pem" ubuntu@<public-ip>
```

---

### Step 3: Install Java

```sh
sudo apt-get update
sudo apt-get install default-jre
sudo apt install fontconfig openjdk-17-jre
```

---

### Step 4: Install Jenkins

```sh
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
sudo apt-get install jenkins
```

---

### Step 5: Access Jenkins

- Copy the **Public IPv4 address** of your EC2 instance
- Open your browser and go to:

```
http://<your-ec2-public-ip>:8080
```

Example:

```
http://107.21.155.134:8080
```

You will see the Jenkins login/setup page.

> Jenkins is now installed and ready for further configuration and integration with pipeline.

---

