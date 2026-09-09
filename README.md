# 🚀 DevOps Interview Preparation — 350+ Questions & Answers

Welcome to my **DevOps Interview Preparation Repository**.

This repository contains **350+ DevOps interview questions and answers** covering the most important tools and technologies used in modern DevOps environments.

The goal of this repository is to prepare for **DevOps Engineer interviews**, with a strong focus on both **theoretical knowledge** and **real-world troubleshooting scenarios**.

---

## 📚 Topics Covered

| # | Topic           | Normal Q&A | Scenario-Based Q&A |   Total |
| - | --------------- | ---------: | -----------------: | ------: |
| 1 | 🐧 Linux        |         20 |                 30 |      50 |
| 2 | ☁️ AWS          |         20 |                 30 |      50 |
| 3 | 🔀 Git & GitHub |         20 |                 30 |      50 |
| 4 | 🐳 Docker       |         20 |                 30 |      50 |
| 5 | ☸️ Kubernetes   |         20 |                 30 |      50 |
| 6 | 🏗️ Terraform   |         20 |                 30 |      50 |
| 7 | 🔧 Jenkins      |         20 |                 30 |      50 |
|   | **TOTAL**       |    **140** |            **210** | **350** |

---

# 🎯 Interview Preparation Structure

Each topic contains **50 questions** divided into two sections.

### 1️⃣ Normal Questions — 20

These questions focus on:

* Basic concepts
* Definitions
* Commands
* Architecture
* Important terminology
* Configuration
* Best practices
* Frequently asked interview questions

### 2️⃣ Scenario-Based Questions — 30

These questions focus on real-world DevOps situations such as:

* Troubleshooting
* Production issues
* Deployment failures
* Performance problems
* Security issues
* Networking problems
* Application failures
* CI/CD failures
* Incident handling
* Root cause analysis

The scenario-based section is designed to help prepare for **real-time technical interviews**.

---

# 📂 Repository Structure

```text
devops-interview-preparation/
│
├── README.md
│
├── 01-Linux/
│   ├── 01-normal-questions.md
│   └── 02-scenario-based-questions.md
│
├── 02-AWS/
│   ├── 01-normal-questions.md
│   └── 02-scenario-based-questions.md
│
├── 03-Git-GitHub/
│   ├── 01-normal-questions.md
│   └── 02-scenario-based-questions.md
│
├── 04-Docker/
│   ├── 01-normal-questions.md
│   └── 02-scenario-based-questions.md
│
├── 05-Kubernetes/
│   ├── 01-normal-questions.md
│   └── 02-scenario-based-questions.md
│
├── 06-Terraform/
│   ├── 01-normal-questions.md
│   └── 02-scenario-based-questions.md
│
└── 07-Jenkins/
    ├── 01-normal-questions.md
    └── 02-scenario-based-questions.md
```

---

# 🐧 1. Linux

### 50 Questions

**20 Normal Questions**

Topics include:

* Linux fundamentals
* File system
* File permissions
* Users and groups
* Processes
* Services
* Networking
* Disk management
* Memory and CPU
* Shell commands
* Package management
* Logs
* SSH
* Cron jobs
* Environment variables

**30 Scenario-Based Questions**

Examples:

* Server CPU utilization is 100%
* Disk is full
* Application is not responding
* SSH connection is failing
* Service suddenly stopped
* Memory utilization is very high
* A process is consuming too much CPU
* Permission denied errors
* Server logs are increasing rapidly

---

# ☁️ 2. AWS

### 50 Questions

**20 Normal Questions**

Important AWS services covered:

* IAM
* EC2
* VPC
* Subnets
* Security Groups
* NACL
* EBS
* EFS
* S3
* Load Balancer
* Auto Scaling
* Route 53
* CloudWatch
* RDS
* Lambda
* CloudTrail
* SNS
* SQS
* Systems Manager

**30 Scenario-Based Questions**

Examples:

* EC2 instance is unreachable
* Website is down
* Application is receiving high traffic
* EC2 disk is full
* S3 bucket needs to be secured
* Load balancer health check is failing
* Auto Scaling is not launching instances
* Application cannot connect to RDS
* Private EC2 needs internet access
* AWS infrastructure needs monitoring

---

# 🔀 3. Git & GitHub

### 50 Questions

**20 Normal Questions**

Topics include:

* Git fundamentals
* Git workflow
* Repository
* Branches
* Merge
* Rebase
* Commit
* Push
* Pull
* Fetch
* Clone
* Stash
* Tags
* GitHub
* Pull Requests
* `.gitignore`
* Remote repositories

**30 Scenario-Based Questions**

Examples:

* Git push is rejected
* Merge conflict occurs
* Wrong commit was pushed
* Secret was accidentally committed
* Branch needs to be reverted
* Developer deleted a branch
* Local repository is out of sync
* GitHub authentication fails
* Need to recover deleted commits
* Production branch was accidentally modified

---

# 🐳 4. Docker

### 50 Questions

**20 Normal Questions**

Topics include:

* Docker architecture
* Images
* Containers
* Dockerfile
* Docker Hub
* Volumes
* Networks
* Ports
* Docker Compose
* Container lifecycle
* Docker commands
* Image layers
* Multi-stage builds
* Environment variables
* Container security

**30 Scenario-Based Questions**

Examples:

* Container keeps restarting
* Docker image is very large
* Container cannot connect to another container
* Port mapping is not working
* Docker build is failing
* Container cannot access the internet
* Application works locally but not inside Docker
* Docker disk space is full
* Container exits immediately
* Docker Compose service is failing

---

# ☸️ 5. Kubernetes

### 50 Questions

**20 Normal Questions**

Topics include:

* Kubernetes architecture
* Cluster
* Node
* Pod
* Deployment
* ReplicaSet
* Service
* ConfigMap
* Secret
* Namespace
* Ingress
* Volumes
* PersistentVolume
* PersistentVolumeClaim
* StatefulSet
* DaemonSet
* Jobs
* CronJobs
* Probes
* Resource limits

**30 Scenario-Based Questions**

Examples:

* Pod is stuck in `Pending`
* Pod is in `CrashLoopBackOff`
* Pod is in `ImagePullBackOff`
* Service cannot reach pods
* Deployment is not creating replicas
* Ingress is not working
* Pod cannot connect to database
* Node becomes `NotReady`
* Kubernetes deployment has zero downtime requirement
* Application needs automatic scaling

---

# 🏗️ 6. Terraform

### 50 Questions

**20 Normal Questions**

Topics include:

* Infrastructure as Code
* Terraform architecture
* Providers
* Resources
* Variables
* Outputs
* Data sources
* Modules
* State
* Backend
* `terraform init`
* `terraform plan`
* `terraform apply`
* `terraform destroy`
* State locking
* Workspaces
* Terraform best practices

**30 Scenario-Based Questions**

Examples:

* Terraform state is locked
* Terraform plan shows unexpected changes
* Resource was manually deleted from AWS
* Terraform state file is lost
* Two engineers run Terraform simultaneously
* Need to manage multiple environments
* Need reusable infrastructure modules
* Terraform deployment fails halfway
* Sensitive values must be protected
* Infrastructure needs to be created automatically

---

# 🔧 7. Jenkins

### 50 Questions

**20 Normal Questions**

Topics include:

* Jenkins architecture
* Controller
* Agents
* Jobs
* Pipelines
* Declarative Pipeline
* Scripted Pipeline
* Jenkinsfile
* Stages
* Steps
* Credentials
* Plugins
* Webhooks
* Git integration
* Docker integration
* Environment variables
* Parameters
* Build triggers

**30 Scenario-Based Questions**

Examples:

* Jenkins build is failing
* GitHub webhook is not triggering Jenkins
* Docker build fails inside Jenkins
* Jenkins agent is offline
* Credentials are not working
* Deployment fails during pipeline execution
* Pipeline needs rollback
* Build is taking too long
* Jenkins server disk is full
* Need to implement CI/CD for production

---

# 🧠 How to Use This Repository

I recommend following this learning process:

```text
Step 1 → Learn the concept
          ↓
Step 2 → Practice the command/configuration
          ↓
Step 3 → Answer the normal interview question
          ↓
Step 4 → Solve the scenario-based question
          ↓
Step 5 → Practice explaining the solution verbally
          ↓
Step 6 → Implement the solution in a lab
```

---

# 🎤 Interview Answer Format

For scenario-based questions, answers should follow this structure:

```text
1. Understand the problem
2. Identify possible causes
3. Check logs / metrics
4. Troubleshoot step-by-step
5. Apply the solution
6. Verify the solution
7. Prevent the issue from happening again
```

Example:

### Scenario

> A Docker container keeps restarting. How would you troubleshoot it?

### Approach

```text
1. Check container status
2. Check container logs
3. Inspect the container
4. Check application configuration
5. Check environment variables
6. Verify dependencies
7. Check resource limits
8. Fix the root cause
9. Restart the container
10. Verify application health
```

---

# 🛠️ Tools Covered

```text
Linux
AWS
Git
GitHub
Docker
Kubernetes
Terraform
Jenkins
```

---

# 🚀 Skills Demonstrated

This repository demonstrates knowledge of:

* Linux administration
* Cloud infrastructure
* Version control
* Containerization
* Container orchestration
* Infrastructure as Code
* CI/CD
* Troubleshooting
* Production incident handling
* DevOps best practices

---

# 📈 Preparation Levels

The questions are designed to gradually increase in difficulty:

```text
Beginner
   ↓
Intermediate
   ↓
Advanced
   ↓
Real-Time Scenario
   ↓
Production Troubleshooting
```

---

# 🎯 Interview Goals

By completing this repository, I aim to be able to:

* Explain DevOps concepts clearly
* Answer common technical interview questions
* Troubleshoot real-world infrastructure problems
* Understand CI/CD pipelines
* Work with AWS infrastructure
* Build and manage Docker containers
* Deploy applications on Kubernetes
* Provision infrastructure using Terraform
* Build CI/CD pipelines using Jenkins
* Explain production troubleshooting approaches

---

# ⭐ Recommended Practice

Don't just memorize the answers.

For every scenario:

```text
Understand → Think → Troubleshoot → Implement → Explain
```

Try to reproduce the scenarios in a **Linux/AWS lab environment** whenever possible.

---

# 📌 Progress Tracker

| Topic        | Normal | Scenario | Status         |
| ------------ | -----: | -------: | -------------- |
| Linux        |  20/20 |     0/30 | 🔄 In Progress |
| AWS          |   0/20 |     0/30 | ⏳ Pending      |
| Git & GitHub |   0/20 |     0/30 | ⏳ Pending      |
| Docker       |   0/20 |     0/30 | ⏳ Pending      |
| Kubernetes   |   0/20 |     0/30 | ⏳ Pending      |
| Terraform    |   0/20 |     0/30 | ⏳ Pending      |
| Jenkins      |   0/20 |     0/30 | ⏳ Pending      |

---

# 📊 Repository Statistics

```text
Total Technologies : 7
Normal Questions   : 140
Scenario Questions : 210
Total Questions    : 350
```

---

# 💡 Future Topics

Additional DevOps topics can be added later:

* Prometheus
* Grafana
* Ansible
* Helm
* ArgoCD
* AWS EKS
* SonarQube
* Nexus
* GitHub Actions
* DevSecOps
* Monitoring & Logging
* CI/CD Architecture

---

# 👨‍💻 Author

**Rushikesh Bilgaye**

DevOps Engineer — Interview Preparation

---

## ⭐ If This Repository Helps You

Give this repository a ⭐ on GitHub and use it for your own DevOps interview preparation.

**Keep learning. Keep practicing. Keep troubleshooting. 🚀**
