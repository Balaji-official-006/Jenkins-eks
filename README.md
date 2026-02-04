# Jenkins → EKS CI/CD Pipeline Project

## Project Title  
End-to-End CI/CD Pipeline using Jenkins, Docker, Amazon ECR & Amazon EKS

---

## About Me

I am a graduate with hands-on experience in AWS and DevOps tools.  
This project demonstrates a real-world CI/CD workflow using Jenkins, Docker, and Kubernetes on AWS.

---

## Project Objective

To design and implement a complete CI/CD pipeline where:

- Source code is stored in GitHub  
- Jenkins automatically builds Docker images  
- Images are pushed to Amazon ECR with versioned tags  
- Jenkins deploys the application to Amazon EKS using Kubernetes manifests  
- The application is exposed publicly using a Kubernetes LoadBalancer  

This project simulates a production-style DevOps workflow with automated deployment and image versioning.

---

## Architecture Flow

GitHub → Jenkins → Docker Build → Amazon ECR → Amazon EKS → LoadBalancer → Browser


---

## Tools & Services Used

- Jenkins – CI/CD automation  
- GitHub – Source code management  
- Docker – Containerization  
- Amazon Elastic Container Registry (ECR) – Image repository  
- Amazon Elastic Kubernetes Service (EKS) – Kubernetes orchestration  
- AWS EC2 – Jenkins host  
- kubectl – Kubernetes CLI  
- eksctl – EKS cluster creation  

---

## Implementation Overview

### Jenkins Setup

- Installed Jenkins on EC2  
- Installed Docker, AWS CLI, and kubectl  
- Configured GitHub credentials in Jenkins  
- Attached IAM permissions for ECR and EKS  

---

### EKS Cluster Creation

- Created EKS cluster using eksctl  
- Added managed node groups  
- Configured kubeconfig:

```bash
aws eks update-kubeconfig --region us-east-1 --name jenkins-eks


# Issues Faced and Solutions – Jenkins → EKS CI/CD Pipeline

---

## 1. EKS Cluster Creation Failed (Elastic IP Limit)

**Problem:**  
Cluster creation failed because AWS Elastic IP quota was exceeded.

**Solution:**  
Released unused Elastic IPs and recreated the cluster.

---

## 2. CloudFormation Stack Already Exists

**Problem:**  
Failed EKS attempt left CloudFormation stack behind.

**Solution:**  
Disabled termination protection and manually deleted the stack.

---

## 3. Worker Nodegroup Failed

**Problem:**  
Managed nodegroup went into rollback due to EC2 capacity issues.

**Solution:**  
Created a new nodegroup using a smaller instance type.

---

## 4. Public Subnet Misconfiguration

**Problem:**  
Worker nodes failed because public subnets didn’t have `MapPublicIpOnLaunch` enabled.

**Solution:**

```bash
eksctl utils update-legacy-subnet-settings

---

## 5. kubectl Authentication Errors

**Problem:**  
kubectl returned forbidden/login errors.

**Solution:**  
Reconfigured kubeconfig and mapped IAM identity.

---

## 6. Jenkins Git Checkout Failed

**Problem:**  
Pipeline failed due to duplicate clone stage and Jenkins agent DNS issues.

**Solution:**  
Removed redundant Clone stage and forced pipeline to run on Jenkins controller.

---

## 7. Dockerfile Missing

**Problem:**  
Docker build failed because Dockerfile was not present in the repository.

**Solution:**  
Added Dockerfile to repository root.

---

## 8. ECR Push Failed

**Problem:**  
Docker image push failed because ECR repository did not exist.

**Solution:**  
Created ECR repository before pushing images.

---

## 9. kubectl Not Installed on Jenkins

**Problem:**  
Deploy stage failed because kubectl was not installed on Jenkins EC2.

**Solution:**  
Installed kubectl and configured EKS context.

---

## Summary

These issues provided hands-on experience with real-world DevOps troubleshooting involving:

- AWS networking and quotas  
- EKS cluster and nodegroup failures  
- Jenkins pipeline misconfigurations  
- Docker build and ECR integration  
- Kubernetes authentication and deployment  

Each problem was resolved through systematic debugging, improving practical understanding of CI/CD pipelines on AWS.
