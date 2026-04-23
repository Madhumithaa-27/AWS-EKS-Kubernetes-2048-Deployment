````markdown
# AWS EKS Kubernetes 2048 Deployment

## Overview

This project demonstrates the deployment of a containerized 2048 web application on Amazon EKS using Kubernetes. The application was deployed on AWS Fargate without managing worker nodes and made publicly accessible using AWS Application Load Balancer through Kubernetes Ingress.

The project helped in understanding real-world Kubernetes deployment flow, networking, ingress routing, IAM integration, and cloud-native application hosting.

---

## Tech Stack

- AWS EKS
- Kubernetes
- AWS Fargate
- kubectl
- eksctl
- Helm
- AWS IAM
- OIDC Provider
- AWS Load Balancer Controller
- Application Load Balancer (ALB)

---

## Architecture

```text
User Browser
     ↓
AWS ALB
     ↓
Kubernetes Ingress
     ↓
Service
     ↓
Deployment
     ↓
Pods (2048 Application)
     ↓
AWS Fargate
````

---

## Key Features

* Managed Kubernetes cluster using Amazon EKS
* Serverless pod execution using AWS Fargate
* Multi-replica deployment for scalability
* Public application exposure using ALB Ingress
* IAM OIDC integration for secure controller permissions
* Helm-based controller installation
* Real-time Kubernetes resource monitoring using kubectl

---

## Project Workflow

### 1. AWS CLI Configuration

```bash
aws configure
```

Configured AWS credentials and default region.

### 2. Created EKS Cluster

```bash
eksctl create cluster --name demo-cluster --region us-east-1 --fargate
```

Created:

* EKS Control Plane
* VPC
* Subnets
* Fargate Profile
* kubeconfig access

### 3. Created Namespace-Specific Fargate Profile

```bash
eksctl create fargateprofile --cluster demo-cluster --region us-east-1 --name alb-sample-app --namespace game-2048
```

### 4. Associated IAM OIDC Provider

```bash
eksctl utils associate-iam-oidc-provider --cluster demo-cluster --region us-east-1 --approve
```

### 5. Installed AWS Load Balancer Controller

Used IAM policy + service account + Helm installation.

### 6. Deployed Application

```bash
kubectl apply -f 2048_full.yaml
```

Created:

* Namespace
* Deployment
* Service
* Ingress

---

## Kubernetes Resources Used

### Namespace

Used to isolate application resources.

### Deployment

Managed 5 replicas of the 2048 pods.

### Service

Exposed pods internally.

### Ingress

Created external routing through AWS ALB.

---

## Verification Commands

```bash
kubectl get pods -n game-2048
kubectl get svc -n game-2048
kubectl get ingress -n game-2048
kubectl get pods -n kube-system
```

---

## Screenshots

## EKS Cluster

<img width="1360" height="711" alt="Screenshot 2026-04-23 171348" src="https://github.com/user-attachments/assets/e88eb890-4d44-45f5-b8d7-d3befc8fa931" />

<img width="1364" height="712" alt="Screenshot 2026-04-23 171457" src="https://github.com/user-attachments/assets/81c3fa30-5ddc-4aba-a7dc-dd229928740b" />

## Running Pods

<img width="1361" height="719" alt="Screenshot 2026-04-23 171431" src="https://github.com/user-attachments/assets/92ad87f4-ced8-4983-a98d-47318357283f" />


## Ingress / ALB DNS

<img width="1365" height="721" alt="Screenshot 2026-04-23 171510" src="https://github.com/user-attachments/assets/1093a782-94b6-41a9-a4da-76640fe3d0c6" />


## Live 2048 Application

<img width="1365" height="716" alt="Screenshot 2026-04-23 171525" src="https://github.com/user-attachments/assets/fbd33950-6e1b-4363-8f5e-68ef82de4cb1" />
<img width="1359" height="716" alt="Screenshot 2026-04-23 171604" src="https://github.com/user-attachments/assets/a441bb16-c50d-4d8a-91cc-3e7606469866" />


---

## What I Learned

* Kubernetes architecture and resources
* Amazon EKS practical deployment
* AWS Fargate serverless containers
* Ingress and load balancing concepts
* IAM OIDC integration
* Helm package management
* Public application exposure using ALB
* Troubleshooting Windows PowerShell vs Linux commands

---

## Resume Highlights

* Deployed containerized application on AWS EKS using Kubernetes
* Implemented serverless pod execution using AWS Fargate
* Configured ALB Ingress for public access
* Used Helm and IAM for controller setup

---

## Cleanup

```bash
eksctl delete cluster --name demo-cluster --region us-east-1
```

---


