# 🚀 EKS Cluster Provisioning using Terraform via Jenkins

This project demonstrates how to automate the creation of an Amazon EKS (Elastic Kubernetes Service) cluster using Terraform, triggered through a Jenkins pipeline.

---

## 📌 Project Overview

- Infrastructure as Code using **Terraform**
- CI/CD automation using **Jenkins**
- Kubernetes cluster provisioning on **AWS EKS**
- Execution handled via **Jenkins Agent (Worker Node)**

---

## 🏗️ Architecture Flow

1. Developer pushes code to GitHub
2. Jenkins pipeline is triggered
3. Jenkins Agent executes Terraform commands
4. Terraform provisions AWS resources
5. EKS cluster is created

---

## 🏗️ Workflow

### 🔹 Job 1: Create Infrastructure
- Terraform Init → Validate → Plan → Apply

### 🔹 Job 2: Destroy Infrastructure
- Terraform Destroy

---


## ⚙️ Prerequisites

### 🔹 1. Jenkins Setup

- Jenkins installed and running
- Jenkins Agent (Worker Node) configured
- Agent connected successfully to Jenkins Master

---

### 🔹 2. Tools Installed on Jenkins Agent

Ensure the following tools are installed on the **Jenkins Worker Node**:

- Java (required for Jenkins agent)
- Terraform
- AWS CLI
- kubectl
- eksctl (optional but recommended)

Verify installations:

```bash
java -version
terraform -version
aws --version
kubectl version --client
