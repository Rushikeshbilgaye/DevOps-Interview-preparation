# Jenkins Interview Questions & Answers

## 📌 Jenkins Interview Preparation

This file contains **50 Jenkins interview questions and answers**:

* **20 Normal Interview Questions**
* **30 Scenario-Based Interview Questions**
* Beginner → Intermediate → Advanced
* Practical DevOps examples
* Jenkins + GitHub + Docker + AWS
* Troubleshooting questions

---

# Part 1 — Normal Jenkins Interview Questions

## 1. What is Jenkins?

**Answer:**

Jenkins is an open-source automation server used to automate:

* Build
* Test
* Deployment
* CI/CD pipelines
* Application delivery

Example:

```text
Developer
   ↓
GitHub
   ↓
Jenkins
   ↓
Build
   ↓
Test
   ↓
Docker Image
   ↓
Docker Hub
   ↓
AWS Server
```

---

## 2. What is CI/CD?

**Answer:**

CI/CD means:

* **CI — Continuous Integration**
* **CD — Continuous Delivery / Continuous Deployment**

### Continuous Integration

Developers frequently push code to Git. Jenkins automatically:

```text
Pull Code
   ↓
Build
   ↓
Test
   ↓
Code Quality Check
```

### Continuous Deployment

After successful testing:

```text
Build
 ↓
Test
 ↓
Deploy
 ↓
Production
```

---

## 3. Why is Jenkins used in DevOps?

**Answer:**

Jenkins automates repetitive software delivery tasks.

Main benefits:

* Automated builds
* Automated testing
* Automated deployment
* Integration with GitHub
* Docker integration
* AWS integration
* Notification support
* Pipeline automation

---

## 4. What is a Jenkins Pipeline?

**Answer:**

A Jenkins Pipeline defines the complete CI/CD process as code.

Example:

```groovy
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/example/project.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t myapp .'
            }
        }

        stage('Test') {
            steps {
                sh 'docker run myapp npm test'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker run -d -p 80:80 myapp'
            }
        }
    }
}
```

---

## 5. What is Jenkinsfile?

**Answer:**

A Jenkinsfile is a text file that contains the Jenkins Pipeline configuration.

Usually it is stored inside the Git repository.

Example:

```text
project/
├── Dockerfile
├── Jenkinsfile
├── index.html
└── README.md
```

This is called **Pipeline as Code**.

---

## 6. What are the types of Jenkins Pipeline?

**Answer:**

There are two major types:

### 1. Declarative Pipeline

Example:

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
    }
}
```

### 2. Scripted Pipeline

Example:

```groovy
node {
    stage('Build') {
        sh 'npm install'
    }

    stage('Test') {
        sh 'npm test'
    }
}
```

Declarative Pipeline is generally easier to read and maintain.

---

## 7. What is a Jenkins Job?

**Answer:**

A Jenkins Job is a task configured in Jenkins.

For example:

```text
Build application
Run tests
Build Docker image
Deploy application
```

Modern Jenkins environments commonly use Pipeline jobs.

---

## 8. What is a Jenkins Node?

**Answer:**

A Jenkins node is a machine that can execute Jenkins tasks.

There are two common concepts:

* Jenkins controller
* Jenkins agent

Example:

```text
Jenkins Controller
       |
       +---- Agent 1
       |
       +---- Agent 2
       |
       +---- Agent 3
```

---

## 9. What is Jenkins Controller?

**Answer:**

The Jenkins controller is responsible for managing Jenkins.

It handles tasks such as:

* Managing jobs
* Scheduling builds
* Managing agents
* Storing configuration
* Managing credentials
* Coordinating pipelines

Build workloads can be delegated to agents.

---

## 10. What is a Jenkins Agent?

**Answer:**

A Jenkins agent is a machine that executes pipeline tasks.

For example:

```text
Controller
     |
     +---- Linux Agent
     |       └── Docker Build
     |
     +---- Windows Agent
             └── .NET Build
```

Agents help distribute workloads.

---

## 11. What is a Jenkins Stage?

**Answer:**

A stage represents a logical section of a pipeline.

Example:

```groovy
stages {

    stage('Checkout') {
        steps {
            echo 'Getting source code'
        }
    }

    stage('Build') {
        steps {
            echo 'Building application'
        }
    }

    stage('Test') {
        steps {
            echo 'Running tests'
        }
    }

    stage('Deploy') {
        steps {
            echo 'Deploying application'
        }
    }
}
```

---

## 12. What is a Jenkins Step?

**Answer:**

A step is an individual action inside a stage.

Example:

```groovy
stage('Build') {
    steps {
        sh 'npm install'
        sh 'npm run build'
    }
}
```

Here:

```text
sh 'npm install'
sh 'npm run build'
```

are steps.

---

## 13. What is a Jenkins Plugin?

**Answer:**

Plugins extend Jenkins functionality.

Examples:

* Git plugin
* Docker-related plugins
* Pipeline plugins
* Credentials plugin
* GitHub integration plugins
* Kubernetes plugin

Plugins allow Jenkins to integrate with external tools.

---

## 14. How does Jenkins integrate with GitHub?

**Answer:**

A common workflow is:

```text
Developer
   ↓
git push
   ↓
GitHub
   ↓
Webhook
   ↓
Jenkins
   ↓
Pipeline
```

Jenkins detects the code change and starts the pipeline.

---

## 15. What is a Jenkins Webhook?

**Answer:**

A webhook allows GitHub to notify Jenkins when an event occurs.

For example:

```text
Developer pushes code
        ↓
GitHub receives push
        ↓
GitHub sends webhook
        ↓
Jenkins receives webhook
        ↓
Pipeline starts
```

This provides automatic CI/CD.

---

## 16. What are Jenkins Credentials?

**Answer:**

Jenkins Credentials securely store authentication information.

Examples:

* GitHub token
* SSH private key
* Docker Hub credentials
* AWS credentials
* Username/password
* API tokens

Instead of writing secrets directly inside Jenkinsfile, credentials should be stored in Jenkins Credentials Manager.

---

## 17. How do you use Jenkins credentials in a Pipeline?

**Answer:**

Example:

```groovy
environment {
    DOCKER_CREDS = credentials('dockerhub-credentials')
}
```

Or:

```groovy
withCredentials([
    usernamePassword(
        credentialsId: 'dockerhub-credentials',
        usernameVariable: 'USERNAME',
        passwordVariable: 'PASSWORD'
    )
]) {
    sh 'docker login -u "$USERNAME" -p "$PASSWORD"'
}
```

Secrets should never be hardcoded.

---

## 18. What is an Artifact in Jenkins?

**Answer:**

An artifact is a file generated during the build process.

Examples:

```text
app.jar
app.war
build.zip
Docker image
test-report.xml
```

Artifacts can be archived for later use.

Example:

```groovy
post {
    success {
        archiveArtifacts artifacts: 'build/*.jar'
    }
}
```

---

## 19. What is the difference between Freestyle Job and Pipeline?

**Answer:**

| Freestyle                 | Pipeline                   |
| ------------------------- | -------------------------- |
| UI-based configuration    | Code-based                 |
| Harder to version         | Stored in Git              |
| Limited complex workflows | Supports complex workflows |
| Less flexible             | Highly flexible            |
| Suitable for simple jobs  | Suitable for CI/CD         |

For modern DevOps projects, Pipeline is generally preferred.

---

## 20. What is a Jenkins Multibranch Pipeline?

**Answer:**

A Multibranch Pipeline automatically discovers branches containing a Jenkinsfile.

Example:

```text
GitHub
│
├── main
│   └── Jenkinsfile
│
├── develop
│   └── Jenkinsfile
│
└── feature/login
    └── Jenkinsfile
```

Jenkins can create separate pipeline executions for each branch.

---

# Part 2 — Scenario-Based Jenkins Interview Questions

## 21. Jenkins pipeline is not starting after a GitHub push. What will you check?

**Answer:**

Check:

1. GitHub webhook configuration
2. Jenkins webhook endpoint
3. Jenkins job configuration
4. Repository URL
5. Credentials
6. Branch configuration
7. Jenkins logs

Workflow:

```text
GitHub Push
    ↓
Webhook
    ↓
Jenkins
    ↓
Pipeline
```

If the webhook is not reaching Jenkins, the pipeline will not start.

---

## 22. Jenkins shows "Permission denied" when running Docker commands. How do you fix it?

**Answer:**

Usually the Jenkins user does not have permission to access Docker.

Check:

```bash
docker ps
```

Then:

```bash
sudo usermod -aG docker jenkins
```

Restart Jenkins:

```bash
sudo systemctl restart jenkins
```

Then verify:

```bash
sudo -u jenkins docker ps
```

The exact fix should also consider your organization's security policy.

---

## 23. Jenkins pipeline fails with "docker: command not found". What do you do?

**Answer:**

Check whether Docker is installed:

```bash
docker --version
```

Check the Jenkins environment:

```bash
sudo -u jenkins which docker
```

If Docker is installed but Jenkins cannot find it, check:

* PATH
* Jenkins agent configuration
* Docker installation
* Agent environment

---

## 24. Jenkins pipeline fails during Git checkout. What could be wrong?

**Answer:**

Possible causes:

* Incorrect repository URL
* Invalid credentials
* Missing Git installation
* Branch does not exist
* Network problem
* SSH key problem

Check:

```bash
git --version
```

Then test repository access from the Jenkins agent.

---

## 25. Jenkins says "Authentication failed" while accessing GitHub. How do you troubleshoot?

**Answer:**

Check:

1. Repository URL
2. Jenkins credentials
3. Credential type
4. Token permissions
5. SSH key configuration
6. Repository permissions

Do not use a normal GitHub account password for Git authentication where token/SSH authentication is required.

---

## 26. Jenkins pipeline builds successfully but deployment fails. What will you check?

**Answer:**

Check:

```text
Build
 ↓
Test
 ↓
Deployment
```

Since the build succeeded, focus on:

* Target server connectivity
* SSH credentials
* Docker status
* Application port
* Deployment commands
* Firewall/security groups
* Application logs

Example:

```bash
docker ps
docker logs <container>
```

---

## 27. Jenkins pipeline fails with "container name already in use". What is the problem?

**Answer:**

A container with the same name already exists.

Check:

```bash
docker ps -a
```

Remove the old container if appropriate:

```bash
docker rm -f demo-container
```

Then run:

```bash
docker run -d --name demo-container -p 80:80 myimage:latest
```

A better production pipeline should handle existing deployments gracefully rather than blindly failing.

---

## 28. Jenkins gives this error: "cannot attach stdin to a TTY-enabled container". Why?

**Answer:**

This usually happens when a command uses interactive terminal options such as:

```bash
docker run -it
```

A Jenkins non-interactive environment normally does not provide a terminal.

Instead use:

```bash
docker run -d --name demo-container -p 80:80 myimage:latest
```

or remove unnecessary `-t`/`-i` options.

---

## 29. Jenkins pipeline is stuck for a long time. What will you check?

**Answer:**

Check:

1. Console Output
2. Agent availability
3. Running shell commands
4. Docker commands
5. Network connectivity
6. External API calls
7. Waiting processes
8. Locks/resources

For example:

```text
Waiting for agent
```

means the required agent may not be available.

---

## 30. Jenkins agent is offline. How do you troubleshoot?

**Answer:**

Check:

```text
Agent status
   ↓
Network
   ↓
Jenkins connectivity
   ↓
Java/runtime
   ↓
Agent logs
```

Also verify that the agent machine is running and can communicate with the Jenkins controller.

---

## 31. Jenkins server disk usage reaches 100%. What do you do?

**Answer:**

Check:

```bash
df -h
```

Find large directories:

```bash
sudo du -sh /var/*
```

Check Jenkins workspace:

```bash
sudo du -sh /var/lib/jenkins/*
```

Possible solutions:

* Remove old workspaces
* Configure build retention
* Delete old artifacts
* Clean unnecessary Docker images
* Increase disk size
* Configure log rotation

---

## 32. Docker images are consuming too much disk space on the Jenkins agent. What will you do?

**Answer:**

Check:

```bash
docker system df
```

Remove unused resources carefully:

```bash
docker image prune
```

For broader cleanup:

```bash
docker system prune
```

Be careful with production systems because cleanup commands can remove resources that are still needed.

---

## 33. Jenkins pipeline fails because tests are failing. Should you deploy?

**Answer:**

Normally, **no**.

The pipeline should stop:

```text
Checkout
   ↓
Build
   ↓
Test ❌
   ↓
Deployment STOPPED
```

Example:

```groovy
stage('Test') {
    steps {
        sh 'npm test'
    }
}
```

If the test command returns a failure status, later stages should not execute.

---

## 34. How can you prevent Jenkins from deploying if the build fails?

**Answer:**

Use sequential pipeline stages.

Example:

```groovy
stage('Build') {
    steps {
        sh 'docker build -t myapp .'
    }
}

stage('Test') {
    steps {
        sh 'docker run myapp npm test'
    }
}

stage('Deploy') {
    steps {
        sh './deploy.sh'
    }
}
```

If Build or Test fails, Jenkins normally stops before Deploy.

---

## 35. Jenkins accidentally exposes a password in console output. What should you do?

**Answer:**

Immediately:

1. Rotate the exposed credential.
2. Remove hardcoded secrets.
3. Store the secret in Jenkins Credentials.
4. Update the pipeline.
5. Check whether the credential was exposed elsewhere.

Never write:

```groovy
sh "docker login -u admin -p mypassword"
```

Use Jenkins Credentials instead.

---

## 36. How do you securely store Docker Hub credentials in Jenkins?

**Answer:**

Create credentials in:

```text
Manage Jenkins
 → Credentials
 → Add Credentials
```

Then reference them using the credentials ID.

Example:

```groovy
withCredentials([
    usernamePassword(
        credentialsId: 'dockerhub',
        usernameVariable: 'DOCKER_USER',
        passwordVariable: 'DOCKER_PASS'
    )
]) {
    sh '''
        echo "$DOCKER_PASS" | docker login \
          -u "$DOCKER_USER" \
          --password-stdin
    '''
}
```

This is safer than putting credentials directly into the Jenkinsfile.

---

## 37. Jenkins builds are running very slowly. How do you troubleshoot?

**Answer:**

Check:

* CPU
* Memory
* Disk I/O
* Network
* Build commands
* Docker build time
* Dependency downloads
* Number of concurrent builds

Commands:

```bash
top
```

```bash
free -h
```

```bash
df -h
```

Optimize:

* Dockerfile layers
* Dependency caching
* Build agents
* Parallel stages
* Workspace cleanup

---

## 38. Multiple developers push code at the same time. How should Jenkins handle it?

**Answer:**

Use appropriate concurrency controls.

Options include:

* Disable concurrent builds
* Queue builds
* Use separate agents
* Use branch-specific pipelines
* Use locking for shared resources

Example:

```groovy
options {
    disableConcurrentBuilds()
}
```

This prevents multiple builds of the same job from running simultaneously.

---

## 39. How can you implement rollback in Jenkins?

**Answer:**

Use versioned deployments.

Example:

```text
v1.0 → Production
v1.1 → Production
v1.2 → Production
```

If v1.2 fails:

```text
Production
    ↓
Rollback
    ↓
v1.1
```

Docker example:

```bash
docker pull myuser/myapp:v1.1
```

Then deploy the previous known-good version.

---

## 40. How do you implement zero-downtime deployment using Jenkins?

**Answer:**

Jenkins itself does not automatically provide zero downtime.

Use deployment strategies such as:

* Rolling deployment
* Blue-green deployment
* Canary deployment

Example:

```text
             Load Balancer
                  |
          ┌───────┴───────┐
          ↓               ↓
       Version 1       Version 2
       Old Version     New Version
```

Traffic can gradually move to the new version.

---

## 41. How can Jenkins deploy a Docker application to an AWS EC2 server?

**Answer:**

Typical architecture:

```text
Developer
   ↓
GitHub
   ↓
Jenkins
   ↓
Docker Build
   ↓
Docker Hub
   ↓
EC2
   ↓
Docker Pull
   ↓
Docker Run
```

Example deployment:

```bash
docker pull username/myapp:latest

docker stop myapp || true
docker rm myapp || true

docker run -d \
  --name myapp \
  -p 80:80 \
  username/myapp:latest
```

For production, use immutable version tags rather than relying only on `latest`.

---

## 42. Jenkins needs to deploy to an EC2 server. How can it authenticate?

**Answer:**

Common methods include:

* SSH key
* AWS Systems Manager
* AWS IAM roles
* Secure deployment agents

Avoid hardcoding private keys in Jenkinsfiles.

Store required credentials securely in Jenkins or use an AWS-native identity mechanism where appropriate.

---

## 43. How can you trigger Jenkins automatically when code is pushed to GitHub?

**Answer:**

Use a GitHub webhook.

Architecture:

```text
Developer
   ↓
git push
   ↓
GitHub
   ↓
Webhook
   ↓
Jenkins
   ↓
Pipeline
```

Configure the repository webhook and Jenkins job to react to the appropriate push/PR events.

---

## 44. What happens if the Jenkins controller goes down while a pipeline is running?

**Answer:**

The impact depends on Jenkins configuration and where the build is running.

A production Jenkins environment should have:

* Persistent Jenkins home
* Backups
* Reliable agents
* Externalized important configuration
* Monitoring
* Recovery procedures

For critical environments, design Jenkins so the controller is not a single point of operational failure.

---

## 45. How do you secure Jenkins?

**Answer:**

Important practices:

### Authentication

Use:

* SSO
* LDAP
* Identity provider integration
* Strong authentication

### Authorization

Use:

* Role-based access control
* Least privilege

### Credentials

Store secrets in:

```text
Jenkins Credentials Manager
```

### Other practices

* Keep Jenkins updated
* Restrict network access
* Use HTTPS
* Secure agents
* Review plugins
* Back up Jenkins
* Audit access
* Avoid exposing Jenkins directly to the public internet

---

## 46. Jenkins pipeline needs to run build and test at the same time. How can you do it?

**Answer:**

Use parallel stages.

Example:

```groovy
stage('Parallel Tests') {
    parallel {

        stage('Unit Test') {
            steps {
                sh 'npm run test:unit'
            }
        }

        stage('Integration Test') {
            steps {
                sh 'npm run test:integration'
            }
        }
    }
}
```

This can reduce pipeline execution time.

---

## 47. How can you pass parameters to a Jenkins build?

**Answer:**

Use build parameters.

Example:

```groovy
parameters {
    choice(
        name: 'ENVIRONMENT',
        choices: ['dev', 'staging', 'prod'],
        description: 'Select environment'
    )
}
```

Then:

```groovy
echo "Deploying to ${params.ENVIRONMENT}"
```

This allows controlled deployments.

---

## 48. Your production deployment failed halfway through. What will you do?

**Answer:**

First, stop further changes.

Then:

```text
Check Pipeline Logs
       ↓
Identify Failed Step
       ↓
Check Application
       ↓
Check Infrastructure
       ↓
Rollback if Required
       ↓
Verify Application
```

For example:

```bash
docker ps
docker logs myapp
```

If the new version is unhealthy, deploy the previous known-good version.

After recovery, investigate the root cause.

---

## 49. Design a Jenkins CI/CD pipeline for a Docker application.

**Answer:**

A practical pipeline:

```text
Developer
    ↓
GitHub
    ↓
Webhook
    ↓
Jenkins
    ↓
Checkout
    ↓
Build
    ↓
Unit Tests
    ↓
Docker Build
    ↓
Security/Quality Checks
    ↓
Docker Registry
    ↓
Deploy to Staging
    ↓
Smoke Test
    ↓
Approval
    ↓
Production
```

Example Jenkinsfile:

```groovy
pipeline {

    agent any

    environment {
        IMAGE = 'myuser/myapp'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t $IMAGE:$BUILD_NUMBER .'
            }
        }

        stage('Test') {
            steps {
                sh 'echo Running tests'
            }
        }

        stage('Push') {
            steps {
                echo 'Push image to registry'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploy application'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully'
        }

        failure {
            echo 'Pipeline failed'
        }
    }
}
```

---

## 50. Explain a complete real-world Jenkins CI/CD architecture.

**Answer:**

A production-style architecture can look like this:

```text
                  Developer
                      |
                      | git push
                      ↓
                   GitHub
                      |
                   Webhook
                      |
                      ↓
              Jenkins Controller
                      |
             -------------------
             |                 |
             ↓                 ↓
        Build Agent       Build Agent
             |
             ↓
       Build Application
             |
             ↓
         Run Tests
             |
             ↓
       Docker Build
             |
             ↓
       Security Scan
             |
             ↓
      Container Registry
             |
             ↓
        Deploy Staging
             |
             ↓
       Smoke / API Tests
             |
          Approval
             |
             ↓
        Production
             |
             ↓
       Load Balancer
             |
             ↓
        Application
```

A production Jenkins setup should also include:

```text
Monitoring
Logging
Backups
RBAC
Secrets Management
Artifact Management
Security Scanning
Notifications
```

---

# Jenkins Troubleshooting Cheat Sheet

## Check Jenkins Service

```bash
sudo systemctl status jenkins
```

Start:

```bash
sudo systemctl start jenkins
```

Restart:

```bash
sudo systemctl restart jenkins
```

Enable at boot:

```bash
sudo systemctl enable jenkins
```

---

## Check Jenkins Logs

```bash
sudo journalctl -u jenkins
```

Follow logs:

```bash
sudo journalctl -u jenkins -f
```

---

## Check Jenkins Port

```bash
sudo ss -tulpn | grep 8080
```

---

## Check Java

```bash
java -version
```

---

## Check Docker

```bash
docker --version
```

```bash
docker ps
```

---

## Check Git

```bash
git --version
```

---

## Check Disk

```bash
df -h
```

---

## Check Memory

```bash
free -h
```

---

## Check CPU

```bash
top
```

---

# Important Jenkins Pipeline Concepts

Remember these concepts for interviews:

```text
Jenkins
│
├── Controller
├── Agents
├── Jobs
├── Pipeline
├── Jenkinsfile
├── Stages
├── Steps
├── Plugins
├── Credentials
├── Webhooks
├── Artifacts
├── Parameters
├── Environment Variables
├── Workspace
└── Build History
```

---

# Jenkins + Docker Interview Flow

Be able to explain:

```text
GitHub
   ↓
Jenkins
   ↓
Checkout Code
   ↓
Docker Build
   ↓
Docker Image
   ↓
Docker Hub / Registry
   ↓
EC2 / Kubernetes
   ↓
Application
```

---

# Jenkins Interview Answer Framework

For scenario questions, use this structure:

### 1. Identify the problem

Example:

> First, I check the Jenkins console output to identify the exact failed stage.

### 2. Check logs

```bash
docker logs
journalctl
```

### 3. Check infrastructure

```bash
df -h
free -h
top
```

### 4. Fix the issue

Apply the appropriate fix.

### 5. Verify

Run the pipeline again and verify the application.

### 6. Prevent recurrence

Add:

* Monitoring
* Alerts
* Automated tests
* Health checks
* Proper credentials
* Build retention
* Documentation

---

# Most Important Jenkins Interview Topics

Before an interview, make sure you can explain:

* Jenkins architecture
* Controller vs Agent
* CI/CD
* Jenkins Pipeline
* Jenkinsfile
* Declarative Pipeline
* Scripted Pipeline
* Stages and Steps
* GitHub integration
* Webhooks
* Jenkins credentials
* Environment variables
* Parameters
* Artifacts
* Plugins
* Multibranch Pipeline
* Docker integration
* AWS deployment
* Pipeline troubleshooting
* Rollback
* Zero-downtime deployment
* Parallel stages
* Jenkins security
* Backup and recovery
* Production CI/CD architecture

---

# Quick Interview Revision

### Jenkins

> Jenkins is an open-source automation server used to implement CI/CD and automate software build, test, and deployment processes.

### Pipeline

> A Jenkins Pipeline defines the complete software delivery workflow as code.

### Jenkinsfile

> Jenkinsfile contains the pipeline definition and is normally stored in the source-code repository.

### Controller

> The Jenkins controller manages jobs, scheduling, configuration, and agents.

### Agent

> A Jenkins agent executes pipeline workloads.

### Webhook

> A webhook allows GitHub to notify Jenkins automatically when repository events occur.

### Credentials

> Jenkins Credentials Manager securely stores authentication information used by pipelines.

### Artifact

> An artifact is a file produced by a build that can be archived or passed to later stages.

### Multibranch Pipeline

> A Multibranch Pipeline automatically creates and manages pipelines for branches containing a Jenkinsfile.

### CI/CD

> CI/CD automates the process of integrating code, testing it, and delivering or deploying it.

---

# Final Jenkins Interview Checklist

Before attending a DevOps interview, you should be comfortable answering:

```text
☑ What is Jenkins?
☑ What is CI/CD?
☑ Jenkins architecture
☑ Controller vs Agent
☑ What is Pipeline?
☑ What is Jenkinsfile?
☑ Declarative vs Scripted Pipeline
☑ Stages vs Steps
☑ Jenkins Plugins
☑ GitHub integration
☑ GitHub Webhooks
☑ Jenkins Credentials
☑ Environment Variables
☑ Build Parameters
☑ Artifacts
☑ Multibranch Pipeline
☑ Jenkins + Docker
☑ Jenkins + AWS
☑ Jenkins + Kubernetes
☑ Pipeline failures
☑ Docker permission errors
☑ Git authentication errors
☑ Container name conflicts
☑ TTY errors
☑ Disk-full problems
☑ Agent offline problems
☑ Rollback
☑ Zero-downtime deployment
☑ Jenkins security
☑ Production CI/CD architecture
```

# ⭐ Interview Tip

For scenario-based questions, don't only give a command.

Explain:

```text
Problem
   ↓
Investigation
   ↓
Root Cause
   ↓
Fix
   ↓
Verification
   ↓
Prevention
```

This makes your answer sound much more like a **real DevOps engineer** than someone who only memorized Jenkins commands.
