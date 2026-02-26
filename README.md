# 🚀 3-Tier Web Application Deployment on AWS EKS

![AWS](https://img.shields.io/badge/AWS-EKS-orange?style=for-the-badge&logo=amazonaws)
![Docker](https://img.shields.io/badge/Container-Docker-blue?style=for-the-badge&logo=docker)
![Kubernetes](https://img.shields.io/badge/Orchestration-Kubernetes-blue?style=for-the-badge&logo=kubernetes)
![MongoDB](https://img.shields.io/badge/Database-MongoDB-green?style=for-the-badge&logo=mongodb)
![React](https://img.shields.io/badge/Frontend-React-blue?style=for-the-badge&logo=react)
![NodeJS](https://img.shields.io/badge/Backend-NodeJS-green?style=for-the-badge&logo=node.js)

---

## 📌 Project Overview

This project demonstrates the deployment of a **3-Tier Web Application** on **Amazon EKS (Elastic Kubernetes Service)** using Docker and Kubernetes.

The application follows a modern cloud-native architecture:

- **Frontend** – ReactJS  
- **Backend** – NodeJS + Express  
- **Database** – MongoDB  
- **Containerization** – Docker  
- **Orchestration** – Kubernetes (EKS)  
- **Cloud Services** – AWS (EKS, ECR, ALB, Route 53, EC2, IAM, VPC)

The application is deployed in a **scalable, highly available, and production-ready Kubernetes environment** on AWS.

---

## 🏗️ Architecture Overview

```
User → Route 53 → Application Load Balancer (ALB)
         ↓
      EKS Cluster (EC2 Worker Nodes)
         ↓
   ┌───────────────┐
   │   Frontend    │ (React)
   ├───────────────┤
   │   Backend     │ (NodeJS API)
   ├───────────────┤
   │   MongoDB     │ (Persistent Storage)
   └───────────────┘
```

---

## 🛠️ Tech Stack

### ☁️ AWS Services
- Amazon EKS  
- Amazon ECR  
- Application Load Balancer (ALB)  
- Route 53  
- EC2 (Worker Nodes)  
- IAM  
- VPC  

### 🔧 DevOps & Containerization
- Docker  
- Kubernetes (kubectl)  
- eksctl  

### 💻 Application Layer
- ReactJS (Frontend)  
- NodeJS + Express (Backend)  
- MongoDB (Database)  

---

## 📂 Project Structure

```
Application-Code/
 ├── frontend/        # ReactJS Frontend
 └── backend/         # NodeJS Backend

Kubernetes-Manifests-file/
 ├── Frontend/
 ├── Backend/
 ├── Database/
 └── ingress.yaml
```

---

# 🚀 Deployment Guide

---

## 🎯 Step 1: IAM Configuration

- Create an IAM user (e.g., `eks-admin`)
- Attach **AdministratorAccess** policy
- Generate Access Key & Secret Key
- Configure using:

```bash
aws configure
```

---

## 💻 Step 2: EC2 Setup

- Launch Ubuntu EC2 instance
- SSH into the instance
- Use this server to install required tools

---

## ☁️ Step 3: Install AWS CLI v2

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip
unzip awscliv2.zip
sudo ./aws/install -i /usr/local/aws-cli -b /usr/local/bin --update
aws configure
```

---

## 🐳 Step 4: Install Docker

```bash
sudo apt update
sudo apt install docker.io
docker ps
sudo chown $USER /var/run/docker.sock
```

---

## 🔧 Step 5: Install kubectl

```bash
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version --short --client
```

---

## 📦 Step 6: Install eksctl

```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
```

---

## 🏔️ Step 7: Setup EKS Cluster

```bash
eksctl create cluster \
  --name three-tier-cluster \
  --region us-west-2 \
  --node-type t2.medium \
  --nodes 2

aws eks update-kubeconfig --region us-west-2 --name three-tier-cluster
kubectl get nodes
```

---

## 📂 Step 8: Run Kubernetes Manifests

```bash
kubectl create namespace three-tier
kubectl apply -f ./Kubernetes-Manifests-file/
```

To delete:

```bash
kubectl delete -f ./Kubernetes-Manifests-file/
```

---

## 🛡️ Step 9: Install AWS Load Balancer IAM Policy

```bash
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/install/iam_policy.json

aws iam create-policy \
  --policy-name AWSLoadBalancerControllerIAMPolicy \
  --policy-document file://iam_policy.json
```

---

## 🚀 Step 10: Deploy AWS Load Balancer Controller

```bash
sudo snap install helm --classic
helm repo add eks https://aws.github.io/eks-charts
helm repo update eks

helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  -n kube-system \
  --set clusterName=three-tier-cluster
```

---

# 🔐 Security & Networking

- IAM roles for EKS and worker nodes  
- Private container registry (ECR)  
- VPC-based networking  
- Kubernetes Secrets for MongoDB credentials  
- Security Groups restricting inbound traffic  
- ALB managing external HTTP/HTTPS traffic  

---

# 📈 Scalability & High Availability

- Kubernetes Deployments ensure self-healing pods  
- Replica sets for backend & frontend  
- EC2 worker nodes managed by EKS  
- Load balanced traffic via ALB  
- Persistent storage using PV & PVC  

---

# 🧠 What I Engineered

- Designed and deployed a complete cloud-native 3-tier application  
- Integrated ALB and Route 53 for intelligent traffic routing  
- Deployed workloads on Amazon EKS with EC2 worker nodes  
- Used ECR, IAM, and VPC networking for secure infrastructure  
- Built a scalable and resilient production-ready architecture  

---

# 🎯 Learning Outcomes

- Kubernetes architecture  
- AWS EKS deployment  
- Container orchestration  
- Cloud networking & security  
- Load balancing & DNS configuration  
- Real-world production deployment patterns  

---

# 🧹 Cleanup

```bash
eksctl delete cluster --name three-tier-cluster --region us-west-2
```

Also remove:
- ECR repositories  
- Load balancer  
- Route 53 records  
- EC2 instance  
- Security groups  

---

# ⭐ If You Like This Project

Give it a ⭐ on GitHub and feel free to fork and improve it!

---
