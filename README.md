# Multi-VPC DevOps on AWS

A Fully Automated CI/CD & GitOps Infrastructure with EKS, Jenkins, Terraform, Ansible, Prometheus, Grafana, and ECR

## 📌 Project Overview

This project demonstrates a **production-style Multi-VPC DevOps infrastructure deployed on AWS**, featuring:

* Kubernetes orchestration with **Amazon EKS**
* **Multi-VPC architecture** with Transit Gateway
* **CI/CD pipeline using Jenkins**, Docker, Trivy, and ECR
* **GitOps deployment with ArgoCD**
* **Infrastructure as Code using Terraform**
* **Configuration management using Ansible**
* Centralized monitoring using **Prometheus & Grafana**
* **Automated RDS backups through Lambda + EventBridge**

The project is designed as a real-world lab suitable for:

* DevOps practice
* Interview discussion
* Professional portfolio
* Cloud & Kubernetes learning

---

## 🎯 Architecture Diagram

> *(Insert the diagram here)*

This architecture includes two main VPCs connected through a Transit Gateway:

### **VPC 1 – Application Platform**

* Amazon EKS Cluster
* Application Pods
* Prometheus & Grafana for monitoring
* Amazon RDS + Read Replica
* NAT Gateways & Internet Gateway

### **VPC 2 – CI/CD VPC**

* Jenkins EC2 instance
* Docker & Trivy for container scanning
* Connectivity to ECR, GitHub, and EKS

---

## 🧱 Key Components

### 🟣 Amazon EKS

* Runs application workloads
* Managed node groups across multiple AZs
* ArgoCD continuously deploys Kubernetes manifests

### 🟠 Terraform

* Automates the creation of:

  * Both VPCs
  * Subnets
  * NAT + IGWs
  * EKS Cluster & Node Groups
  * RDS
  * Security Groups
  * Transit Gateway

### ⚙️ Ansible

* Used to provision Jenkins and required tools on EC2

### 🧰 Jenkins Pipeline

Pipeline stages:

1. Pull source code from GitHub
2. Build and tag Docker image
3. Scan image with Trivy
4. Push to Amazon ECR
5. Update Kubernetes manifests in GitHub
6. ArgoCD deploys automatically

### 🗂 Amazon RDS

* Stores application data
* Read replica for scaling reads
* Automated daily backup using Lambda + EventBridge

### 📊 Monitoring Stack

* **Prometheus** collects metrics from the cluster
* **Grafana** visualizes dashboards
* Provides complete visibility into application and infrastructure health

---

## 🔁 GitOps Workflow

1. Dev pushes code to GitHub
2. Jenkins builds & scans container
3. Image is pushed to ECR
4. Jenkins updates Kubernetes YAML manifests
5. ArgoCD detects changes
6. Application updates are applied instantly in EKS

No manual kubectl commands required.

---

## 🚀 CI/CD Flow (Step-by-Step)

### 1️⃣ Developer pushes code

GitHub triggers webhook → Jenkins job starts

### 2️⃣ Jenkins CI pipeline

```
git clone repo
docker build -t <image>
trivy scan <image>
docker push <ECR>
```

### 3️⃣ Update Kubernetes manifests

Jenkins commits new image tag back to GitHub

### 4️⃣ ArgoCD GitOps sync

Detects manifest changes and applies them to EKS

---

## 🧰 Tools & Technologies

| Category                | Tools                |
| ----------------------- | -------------------- |
| Cloud                   | AWS                  |
| Container Orchestration | Kubernetes (EKS)     |
| CI/CD                   | Jenkins              |
| GitOps                  | ArgoCD               |
| IaC                     | Terraform            |
| Config Management       | Ansible              |
| Monitoring              | Prometheus & Grafana |
| Container Registry      | ECR                  |
| Security Scanning       | Trivy                |
| Programming             | Python, YAML         |

---

## 📁 Repository Structure (Suggested)

```
├── terraform/
│   ├── vpc1/
│   ├── vpc2/
│   ├── eks/
│   ├── rds/
│   └── transit-gw/
│
├── ansible/
│   ├── roles/
│   └── jenkins-setup.yml
│
├── k8s/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── ingress.yaml
│
├── jenkins/
│   ├── Jenkinsfile
│   └── scripts/
│
├── monitoring/
│   ├── prometheus/
│   └── grafana/
│
└── README.md
```

---

## 📥 How to Deploy

### 1️⃣ Clone the repository

```
git clone https://github.com/AhmedSabeh/Multi-VPC-DevOps-on-AWS.git
```

### 2️⃣ Deploy infrastructure with Terraform

```
cd terraform
terraform init
terraform plan
terraform apply
```

### 3️⃣ Configure Jenkins using Ansible

```
ansible-playbook jenkins-setup.yml -i inventory
```

### 4️⃣ Configure ArgoCD

```
kubectl apply -f argo-app.yaml
```

### 5️⃣ Push a new commit to trigger a deployment

* Jenkins builds the image
* Trivy scans it
* ECR stores it
* ArgoCD deploys it

---

## 🔒 Security Highlights

* Private subnets for workloads
* NAT gateways for controlled internet access
* Trivy container scanning
* Minimal IAM roles
* VPC isolation
* Encrypted RDS storage

---

## 📊 Monitoring & Observability

### Metrics collected:

* CPU / Memory usage
* Request latency
* Pod health
* Node system metrics
* Application logs

Grafana dashboards display live insights for:

* Cluster performance
* Application behavior
* Database metrics
