# DevOps Training - Day 4  
**Date:** 01/04/2025

## Docker - Microservices Setup

### Project Setup

### Step 1: Navigate to Desktop and Create Project Folder

```bash
$ cd /c/Users/UKH/Desktop
$ mkdir microservices-app
$ cd microservices-app
```

### Step 2: Create Microservice Directories

```bash
$ mkdir order_service
$ mkdir user_service
$ touch docker-compose.yml
$ ls
```

Output:
```
docker-compose.yml  order_service/  user_service/
```

---

### Step 3: Inside `order_service` Directory

```bash
$ cd order_service
$ touch Dockerfile
$ touch app.py
$ touch requirements.txt
$ ls
```

---

### Step 4: Inside `user_service` Directory

```bash
$ cd ../user_service
$ touch Dockerfile
$ touch app.py
$ touch requirements.txt
$ ls -l
```

---

### Step 5: Edit Required Files

Use a text editor to add content to:

```bash
$ nano Dockerfile
$ nano app.py
$ nano requirements.txt
```

Repeat above for both services.

---

### Step 6: Navigate Back to Project Root and Create Docker Compose File

```bash
$ cd ..
$ nano docker-compose.yml
```

---

### Step 7: Build the Docker Images

```bash
$ docker-compose build
```

Expected output: long list of layer pulling, caching, and build steps for `user_service` and `order_service`.

---

## Kubernetes (K8s) - Introduction & Setup on AWS EKS

### What is Kubernetes?

- An open-source container orchestration platform used to automate the deployment, scaling, and management of containerized applications.
- Supports multiple container runtimes, including Docker.
- Manages workloads across clusters of physical/virtual machines.

---

### Components of Kubernetes

#### Control Plane (Master Node)

1. **API Server (kube-apiserver)**  
   - Frontend for the cluster  
   - Handles requests from kubectl, CLI, and UIs  

2. **etcd (Database)**  
   - Key-value store  
   - Stores configuration and state of the cluster  

3. **Scheduler (kube-scheduler)**  
   - Assigns pods to available worker nodes  

#### Worker Node Components

1. **Kubelet**  
   - Agent on each worker node  
   - Ensures containers in pods are running  

2. **Kube-Proxy**  
   - Manages networking  
   - Routes traffic between services/pods  

3. **Container Runtime**  
   - Runs containers (Docker, containerd, CRI-O)

---

### What is a Cluster?

- A group of nodes (master and workers)
- Master node assigns tasks, worker nodes execute them
- Ensures high availability and fault tolerance

---

## AWS EKS - Kubernetes Cluster Setup

### Create EKS Cluster

1. Navigate to AWS → EKS → Create Cluster
2. Set cluster name, select standard version
3. Enable Amazon EBS CSI Controller under Add-ons

---

### Create Master Node

1. Go to EC2 → Launch Instance
2. Select Amazon Linux, no key pair
3. Network settings → Existing default VPC
4. IAM role: Attach EKS Cluster Admin role
5. Launch instance → Rename (e.g., `athmika-masternode`)
6. Connect via SSH to the master node instance

---

### Create Worker Nodes

1. Launch 2 additional EC2 instances as worker nodes
2. Attach EKS Node IAM Role (EKSNodeRole) with permissions:
   - `AmazonEC2ContainerRegistryReadOnly`
   - `AmazonEKS_CNI_Policy`
   - `AmazonEKSWorkerNodePolicy`
   - `AmazonEBSCSIDriverPolicy`

---

### Configure Master Node (Install Tools)

```bash
# Step 1: Install kubectl
curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.24.11/2023-03-17/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo cp ./kubectl /usr/local/bin
export PATH=/usr/local/bin:$PATH

# Step 2: Verify kubectl version
kubectl version

# Step 3: Install AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Step 4: Install Git
sudo yum install git -y

# Step 5: Clone K8s app repo
git clone https://github.com/N4si/K8s-voting-app.git
```

---

### Configure Kubernetes

1. Update kubeconfig with cluster details:

```bash
aws eks update-kubeconfig --name athmika-cluster --region us-east-1
```

2. If `kubectl get nodes` returns "unauthorized", configure AWS CLI:

```bash
aws configure
# Provide Access Key and Secret Key
```

3. Try again:

```bash
kubectl get nodes
```

---

### Access Keys Used for AWS CLI

```bash
Access Key ID:     AKIATQPD7SLLCVE5K66Z  
Secret Access Key: 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
```

---

### Command History (for reference)

```bash
1  kubectl version
2  curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.24.11/2023-03-17/bin/linux/amd64/kubectl
3  chmod +x ./kubectl
4  sudo cp ./kubectl /usr/local/bin
5  export PATH=/usr/local/bin:$PATH
6  curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
7  unzip awscliv2.zip
8  sudo ./aws/install
9  sudo yum install git -y
10 git --version
11 git clone https://github.com/N4si/K8s-voting-app.git
12 cd K8s-voting-app
13 cd manifests
14 cd ../..
15 pwd
16 kubectl get nodes
17 aws eks update-kubeconfig --name athmika-cluster --region us-east-1
18 aws configure
19 aws eks update-kubeconfig --name athmika-cluster --region us-east-1
```

---
