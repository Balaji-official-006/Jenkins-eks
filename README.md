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
