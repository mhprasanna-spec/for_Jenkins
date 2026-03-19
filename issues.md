# 🚨 Issues Faced During EKS Provisioning using Terraform via Jenkins

This document captures the real-world issues encountered while automating AWS EKS infrastructure using Terraform through Jenkins, along with their fixes and key learnings.

---

## 🔹 1. Git Repository Access Issue

**Problem:**
Jenkins failed to clone the repository.

**Error:**

Please make sure you have the correct access rights and the repository exists


**Cause:**
- Incorrect repository URL  
- Missing authentication  

**Fix:**
- Configured GitHub Personal Access Token (PAT)  
- Added credentials in Jenkins  

---

## 🔹 2. Jenkins Credentials Misconfiguration

**Problem:**
AWS credentials were not accessible inside the Jenkins job.

**Cause:**
- Credentials not properly bound  

**Fix:**
- Enabled **"Use secret text(s) or file(s)"**  
- Exposed:

    - AWS_ACCESS_KEY_ID
    - AWS_SECRET_ACCESS_KEY


---

## 🔹 3. Terraform Not Installed on Agent

**Problem:**

terraform: command not found


**Cause:**
Terraform not installed on Jenkins worker node  

**Fix:**
- Installed Terraform  
- Verified using:

terraform -version


---

## 🔹 4. AWS Permission Issues

**Problem:**
Terraform execution failed due to insufficient permissions.

**Cause:**
IAM user lacked required access  

**Fix:**
- Attached policies:
  - EKS
  - EC2
  - VPC
  - IAM  

---

## 🔹 5. Terraform Initialization Errors

**Problem:**
Terraform required reinitialization.

**Error Message:**

Run "terraform init"


**Fix:**

terraform init


---

## 🔹 6. Terraform State File Issues

**Problem:**
- Inconsistent infrastructure state  
- Destroy job failed  

**Cause:**
Using local state file  

**Fix:**
- Recommended using remote backend:
  - AWS S3 (state storage)  
  - DynamoDB (state locking)  

---

## 🔹 7. Jenkins Agent Disconnection (Critical)

**Problem:**

java.io.EOFException
Channel is closing down


**Cause:**
- Jenkins agent disconnected during execution  
- Network interruption or system crash  

**Fix:**
- Reconnected agent  
- Ensured stable network  

---

## 🔹 8. Low Resource Issues (Memory/CPU)

**Problem:**
Terraform destroy caused agent crash.

**Cause:**
- Insufficient CPU/RAM (t2.micro / t3.micro)  

**Fix:**
- Upgraded instance:

t3.micro → t3.medium

- Added swap memory  

---

## 🔹 9. Network / Connectivity Issues

**Problem:**
Agent could not reach AWS or GitHub.

**Fix:**

ping github.com
aws sts get-caller-identity


---

## 🔹 10. Long Execution Time

**Problem:**
EKS creation/destruction took 15–20 minutes.

**Risk:**
Job timeout or agent disconnection  

**Fix:**

timeout 30m terraform destroy


---

## 🔹 11. Kubernetes Access Issue

**Problem:**
`kubectl` not working after cluster creation.

**Fix:**

aws eks update-kubeconfig --region <region> --name <cluster-name>
kubectl get nodes


---

## 🔹 12. Terraform Destroy Dependency Issues

**Problem:**
Resources not fully deleted.

**Examples:**
- Security groups still attached  
- ENI not released  

**Fix:**
- Re-run destroy  
- Manual cleanup if required  

---

# 🧠 Key Learnings

- Importance of proper credential management  
- Handling Terraform state effectively  
- Resource planning for heavy workloads  
- Debugging Jenkins agent issues  
- Real-world DevOps troubleshooting  

---

# 🎯 Summary

> Faced multiple real-world DevOps challenges including Git authentication failures, credential misconfiguration, Terraform initialization issues, and Jenkins agent crashes due to resource constraints. Resolved them through proper configuration, infrastructure upgrades, and systematic debugging.

---
