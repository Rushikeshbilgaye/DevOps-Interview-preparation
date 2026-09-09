# Docker Interview Questions & Answers

## 📌 Overview

This document contains **50 Docker interview questions and answers** for DevOps engineers.

### Structure

* **20 Normal Interview Questions**
* **30 Scenario-Based Interview Questions**
* Beginner → Intermediate → Advanced
* Practical commands
* Production troubleshooting scenarios
* CI/CD and Docker Hub scenarios

---

# 🟢 PART 1 — NORMAL INTERVIEW QUESTIONS

## Q1. What is Docker?

**Answer:**

Docker is a containerization platform used to package an application along with its dependencies into a portable unit called a **container**.

A Docker container can run consistently across different environments such as:

* Developer laptop
* Testing server
* Jenkins server
* AWS EC2
* Kubernetes

### Example

```bash
docker run nginx
```

This downloads the Nginx image and starts a container.

---

## Q2. What is a Docker container?

**Answer:**

A container is a lightweight, isolated runtime environment that contains:

* Application code
* Dependencies
* Libraries
* Configuration
* Runtime environment

Containers share the host operating system kernel, making them lighter than virtual machines.

---

## Q3. What is a Docker image?

**Answer:**

A Docker image is a read-only template used to create containers.

For example:

```bash
docker pull nginx
```

The `nginx` image can then be used to create multiple containers.

```bash
docker run -d nginx
```

### Image → Container

```text
Docker Image
     |
     v
Docker Container
```

---

## Q4. What is the difference between Docker image and container?

**Answer:**

| Image                     | Container                    |
| ------------------------- | ---------------------------- |
| Read-only template        | Running instance of an image |
| Used to create containers | Runs the application         |
| Immutable                 | Has a writable layer         |
| Can be stored in registry | Runs on Docker Engine        |

Example:

```bash
docker pull nginx
docker run -d nginx
```

Here:

* `nginx` image = template
* Running Nginx = container

---

## Q5. What is Docker Engine?

**Answer:**

Docker Engine is the core technology that builds and runs Docker containers.

It contains components such as:

* Docker CLI
* Docker daemon
* Docker API
* Container runtime

Example:

```bash
docker version
```

```bash
docker info
```

---

## Q6. What is a Dockerfile?

**Answer:**

A Dockerfile is a text file containing instructions used to build a Docker image.

Example:

```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/

EXPOSE 80
```

Build the image:

```bash
docker build -t my-nginx .
```

Run it:

```bash
docker run -d -p 80:80 my-nginx
```

---

## Q7. What is the purpose of `FROM` in Dockerfile?

**Answer:**

`FROM` specifies the base image.

Example:

```dockerfile
FROM ubuntu:24.04
```

Or:

```dockerfile
FROM node:20-alpine
```

Every Dockerfile normally starts with a `FROM` instruction unless using a special scratch-based build.

---

## Q8. What is the difference between CMD and ENTRYPOINT?

**Answer:**

Both define what should execute when a container starts.

### CMD

Provides the default command or arguments.

```dockerfile
CMD ["nginx", "-g", "daemon off;"]
```

### ENTRYPOINT

Defines the main executable.

```dockerfile
ENTRYPOINT ["python3"]
```

A common pattern is:

```dockerfile
ENTRYPOINT ["python3"]
CMD ["app.py"]
```

Running:

```bash
docker run myapp test.py
```

results in:

```text
python3 test.py
```

---

## Q9. What is Docker Hub?

**Answer:**

Docker Hub is a public container image registry.

Developers can:

* Pull images
* Push images
* Store private images
* Share images

Example:

```bash
docker pull nginx
```

Login:

```bash
docker login
```

Push:

```bash
docker push username/myapp:latest
```

---

## Q10. What is a Docker registry?

**Answer:**

A Docker registry is a system that stores and distributes container images.

Examples:

* Docker Hub
* Amazon ECR
* GitHub Container Registry
* Google Artifact Registry
* Azure Container Registry

Typical workflow:

```text
Developer
   |
   v
docker build
   |
   v
Docker Image
   |
   v
Docker Registry
   |
   v
Production Server
```

---

## Q11. What are Docker volumes?

**Answer:**

Docker volumes are used to persist data outside the container's writable layer.

Example:

```bash
docker volume create mydata
```

Run:

```bash
docker run -d \
  --name mysql \
  -v mydata:/var/lib/mysql \
  mysql
```

Even if the container is deleted, the volume can remain.

---

## Q12. What is a bind mount?

**Answer:**

A bind mount maps a directory or file from the host system into a container.

Example:

```bash
docker run -d \
  -v /home/ubuntu/app:/app \
  nginx
```

Here:

```text
Host:
 /home/ubuntu/app

       |
       v

Container:
 /app
```

### Volume vs Bind Mount

| Volume                           | Bind Mount               |
| -------------------------------- | ------------------------ |
| Managed by Docker                | Managed by user          |
| Better for application data      | Useful for development   |
| Docker controls storage location | User specifies host path |

---

## Q13. What is Docker networking?

**Answer:**

Docker networking allows containers to communicate with:

* Other containers
* Host machine
* External systems
* Internet

List networks:

```bash
docker network ls
```

Common network drivers include:

* bridge
* host
* none
* overlay

---

## Q14. What is Docker port mapping?

**Answer:**

Port mapping connects a host port to a container port.

Example:

```bash
docker run -d -p 8080:80 nginx
```

Meaning:

```text
Host Port 8080
       |
       v
Container Port 80
```

Access:

```text
http://server-ip:8080
```

---

## Q15. What is Docker Compose?

**Answer:**

Docker Compose is used to define and run multi-container applications using a YAML file.

Example:

```yaml
services:

  frontend:
    image: nginx
    ports:
      - "80:80"

  backend:
    image: my-backend

  database:
    image: mysql:8
```

Start:

```bash
docker compose up -d
```

Stop:

```bash
docker compose down
```

---

## Q16. What is a Docker image layer?

**Answer:**

Docker images are built from multiple layers.

Example:

```dockerfile
FROM ubuntu
RUN apt-get update
RUN apt-get install -y nginx
COPY index.html /var/www/html/
```

Each instruction can create an image layer.

Docker uses layer caching to make subsequent builds faster.

---

## Q17. What is a multi-stage Docker build?

**Answer:**

Multi-stage builds allow us to use multiple stages in a Dockerfile and copy only the required output into the final image.

Example:

```dockerfile
FROM node:20 AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build


FROM nginx:alpine

COPY --from=builder /app/dist /usr/share/nginx/html
```

### Benefit

The final image does not need the Node.js build environment.

This helps reduce:

* Image size
* Attack surface
* Deployment time

---

## Q18. What is `docker exec`?

**Answer:**

`docker exec` runs a command inside a running container.

Example:

```bash
docker exec -it mycontainer /bin/sh
```

For Ubuntu:

```bash
docker exec -it mycontainer /bin/bash
```

Check files:

```bash
docker exec mycontainer ls
```

---

## Q19. What is the difference between `docker stop` and `docker kill`?

**Answer:**

### docker stop

Gracefully stops a container.

```bash
docker stop mycontainer
```

Docker sends a termination signal and gives the application time to shut down.

### docker kill

Immediately sends a kill signal.

```bash
docker kill mycontainer
```

Use `kill` when the container is stuck or does not stop gracefully.

---

## Q20. What is a Docker health check?

**Answer:**

A health check determines whether the application inside a container is healthy.

Example:

```dockerfile
HEALTHCHECK --interval=30s --timeout=5s \
  CMD curl -f http://localhost:8080/health || exit 1
```

Check status:

```bash
docker ps
```

A container can show:

```text
healthy
```

or:

```text
unhealthy
```

---

# 🔴 PART 2 — SCENARIO-BASED INTERVIEW QUESTIONS

## Q21. Your Docker container starts and immediately exits. What will you check?

**Answer:**

First check the container:

```bash
docker ps -a
```

Then inspect logs:

```bash
docker logs container_name
```

Check the configured command:

```bash
docker inspect container_name
```

Common causes:

* Application crashed
* Incorrect CMD
* Incorrect ENTRYPOINT
* Missing configuration
* Application completed and exited normally

I would identify the root cause from the logs, fix the configuration or command, then restart and verify the container.

---

## Q22. Your Docker container is running, but you cannot access the application from the browser. What will you do?

**Answer:**

First:

```bash
docker ps
```

Check port mapping.

Example:

```text
0.0.0.0:8080->80/tcp
```

Then test locally:

```bash
curl http://localhost:8080
```

If that works, check:

* AWS Security Group
* Firewall
* Network ACL
* Correct public IP
* Correct host port

For example:

```bash
docker run -d -p 8080:80 nginx
```

The AWS Security Group must allow TCP port `8080`.

---

## Q23. You get "port is already allocated" while starting a container. How do you fix it?

**Answer:**

Check which process/container is using the port.

```bash
docker ps
```

For example:

```bash
docker run -d -p 80:80 nginx
```

If port 80 is already used, use another host port:

```bash
docker run -d -p 8080:80 nginx
```

Or stop the existing container:

```bash
docker stop old-container
```

Then start the new container.

---

## Q24. You receive "Conflict. The container name is already in use." What do you do?

**Answer:**

Check existing containers:

```bash
docker ps -a
```

Suppose:

```bash
docker run --name demo-container nginx
```

returns a name conflict.

Remove the old container:

```bash
docker rm demo-container
```

Or choose another name:

```bash
docker run --name demo-container-2 nginx
```

If the old container is running:

```bash
docker stop demo-container
docker rm demo-container
```

---

## Q25. A container works on your laptop but fails on the server. How would you troubleshoot?

**Answer:**

I would compare the environments.

First check:

```bash
docker version
docker info
```

Then inspect:

```bash
docker logs container_name
docker inspect container_name
```

I would verify:

* Image version
* Environment variables
* Configuration files
* Network
* Ports
* Volumes
* CPU/memory
* Architecture

I would also make sure the same image tag or, preferably, the same immutable image digest is being deployed.

---

## Q26. Your Docker image is 2 GB. How would you reduce its size?

**Answer:**

I would:

1. Use a smaller base image.
2. Use multi-stage builds.
3. Remove unnecessary packages.
4. Add `.dockerignore`.
5. Combine appropriate build steps.
6. Avoid copying unnecessary files.

Example:

```dockerfile
FROM node:20-alpine
```

instead of:

```dockerfile
FROM node:20
```

For compiled applications, use:

```dockerfile
FROM builder
```

and copy only the required runtime files into the final stage.

---

## Q27. Docker build is taking too long. How would you optimize it?

**Answer:**

I would check Docker layer caching.

Bad:

```dockerfile
COPY . .
RUN npm install
```

Better:

```dockerfile
COPY package*.json ./
RUN npm install

COPY . .
```

This allows Docker to reuse the dependency layer when application source files change but `package.json` does not.

I would also:

* Use `.dockerignore`
* Avoid unnecessary files
* Use BuildKit/buildx
* Use smaller build contexts

---

## Q28. Your Docker container is consuming too much memory. What would you do?

**Answer:**

First inspect usage:

```bash
docker stats
```

Then inspect the application.

I would check:

* Memory leak
* Application configuration
* Number of worker processes
* Traffic
* Container limits

We can set a memory limit:

```bash
docker run -d --memory=512m myapp
```

Then monitor the application and determine whether the limit is appropriate.

---

## Q29. A Docker container is consuming 100% CPU. How do you troubleshoot it?

**Answer:**

Run:

```bash
docker stats
```

Then identify the application process:

```bash
docker top container_name
```

Check logs:

```bash
docker logs container_name
```

Possible causes:

* Infinite loop
* High traffic
* Bad application logic
* Excessive worker processes
* CPU-intensive task

I would identify the process causing the load, fix the application issue, and apply appropriate CPU limits if necessary.

---

## Q30. Your Docker host disk is full. What will you check?

**Answer:**

First:

```bash
df -h
```

Then check Docker usage:

```bash
docker system df
```

Check unused resources:

```bash
docker images
docker ps -a
docker volume ls
```

Unused resources can be cleaned carefully.

For example:

```bash
docker image prune
```

Or:

```bash
docker container prune
```

For a broader cleanup:

```bash
docker system prune
```

I would never blindly remove volumes in production because they may contain important data.

---

## Q31. A container cannot connect to another container. What will you check?

**Answer:**

Check networks:

```bash
docker network ls
```

Inspect the network:

```bash
docker network inspect mynetwork
```

Both containers should be connected to the appropriate Docker network.

Create a network:

```bash
docker network create app-network
```

Run containers on it:

```bash
docker run -d --network app-network --name backend backend-image
docker run -d --network app-network --name frontend frontend-image
```

The frontend can communicate with the backend using the container name:

```text
http://backend:8080
```

---

## Q32. Your application cannot connect to the database container. What could be wrong?

**Answer:**

I would check:

```bash
docker ps
docker logs database
docker inspect database
```

Then verify:

* Database container is running
* Correct database hostname
* Correct port
* Correct username/password
* Same Docker network
* Database is ready before application starts

In Docker Compose, I would use the service name as the hostname.

Example:

```yaml
services:
  backend:
    environment:
      DB_HOST: database

  database:
    image: mysql:8
```

The backend should connect to:

```text
database:3306
```

not `localhost:3306`.

---

## Q33. Your Docker container cannot access the internet. How do you troubleshoot it?

**Answer:**

First test inside the container:

```bash
docker exec -it container_name sh
```

Then:

```bash
ping 8.8.8.8
```

and:

```bash
nslookup google.com
```

I would check:

* Docker network
* DNS
* Host internet connectivity
* Firewall
* Proxy configuration
* Docker daemon configuration

I would also test the default bridge network and a custom network to isolate the problem.

---

## Q34. Your Docker container keeps restarting. What would you investigate?

**Answer:**

First:

```bash
docker ps
```

Then:

```bash
docker logs container_name
```

Check restart configuration:

```bash
docker inspect container_name
```

Possible causes:

* Application crash
* Invalid configuration
* Missing environment variable
* Failed dependency
* Health check failure
* Incorrect restart policy

I would fix the underlying application problem rather than simply disabling the restart policy.

---

## Q35. Docker Compose shows that one service is unhealthy. What do you do?

**Answer:**

Check:

```bash
docker compose ps
```

Then:

```bash
docker compose logs service_name
```

Inspect the health status:

```bash
docker inspect container_name
```

I would verify:

* Health check command
* Application startup time
* Port
* Dependency availability
* Environment variables

A common issue is that the health check starts before the application is ready.

---

## Q36. `docker compose up` fails because a directory does not exist. How would you troubleshoot?

**Answer:**

I would inspect the Compose file.

For example:

```yaml
services:
  backend:
    build: ./backend
```

If the directory does not exist:

```bash
ls
ls backend
```

I would either create the required directory or correct the path in `compose.yaml`.

Then run:

```bash
docker compose config
```

to validate the Compose configuration.

---

## Q37. You get "docker: unknown command: docker compose". What does it mean?

**Answer:**

It usually means the Docker Compose plugin is not installed or the Docker installation is incomplete/outdated.

Check:

```bash
docker version
docker compose version
```

On modern Docker installations, the preferred command is:

```bash
docker compose up -d
```

The older standalone syntax is:

```bash
docker-compose up -d
```

I would install/configure the Compose plugin appropriate for the operating system and Docker version rather than mixing installation methods.

---

## Q38. Docker says the legacy builder is deprecated. What should you do?

**Answer:**

The modern Docker build workflow uses **BuildKit/buildx**.

Check:

```bash
docker buildx version
```

Build using:

```bash
docker buildx build -t myapp:latest .
```

If pushing directly:

```bash
docker buildx build \
  -t username/myapp:latest \
  --push .
```

For normal local builds, current Docker versions also support:

```bash
docker build -t myapp:latest .
```

with BuildKit underneath.

---

## Q39. You accidentally committed a `.env` file containing a Docker Hub password. What would you do?

**Answer:**

First, I would treat the credential as compromised.

I would:

1. Revoke/rotate the credential immediately.
2. Remove the secret from the repository.
3. Add `.env` to `.gitignore`.
4. Check whether the secret exists in Git history.
5. Remove sensitive data from history if necessary.
6. Update the application to use a proper secret-management mechanism.

Example:

```gitignore
.env
```

I would never rely on deleting the file from the latest commit alone because secrets may remain in Git history.

---

## Q40. Docker Hub push fails with an authentication error. What do you check?

**Answer:**

First:

```bash
docker login
```

Then verify:

```bash
docker info
```

Tag the image correctly:

```bash
docker tag myapp:latest username/myapp:latest
```

Push:

```bash
docker push username/myapp:latest
```

I would verify:

* Docker Hub username
* Authentication method
* Repository permissions
* Repository name
* Image tag

I would avoid putting passwords directly into shell commands or Dockerfiles.

---

## Q41. Your Jenkins pipeline builds the Docker image but fails during `docker push`. What could be wrong?

**Answer:**

I would check:

```bash
docker images
docker login
```

Then verify the image tag:

```bash
docker images
```

For example:

```bash
docker tag project:latest username/project:latest
docker push username/project:latest
```

I would also check:

* Jenkins credentials
* Docker daemon access
* Docker Hub permissions
* Registry URL
* Network connectivity

In Jenkins, credentials should be stored in Jenkins Credentials rather than hard-coded in the Jenkinsfile.

---

## Q42. Jenkins gives "cannot connect to the Docker daemon." How do you troubleshoot?

**Answer:**

On the Jenkins server:

```bash
sudo systemctl status docker
```

If stopped:

```bash
sudo systemctl start docker
```

Test:

```bash
docker ps
```

If Jenkins runs as a separate user, verify that the Jenkins user has appropriate access to Docker.

For example:

```bash
sudo usermod -aG docker jenkins
```

Then restart the relevant service/session as appropriate.

I would also consider the security implications of granting access to the Docker socket because Docker daemon access is highly privileged.

---

## Q43. Your Dockerfile application starts but the expected port is not available. What will you check?

**Answer:**

I would check:

```bash
docker ps
```

Then inspect the port mapping:

```bash
docker port container_name
```

Also verify the application is actually listening inside the container.

```bash
docker exec container_name ss -lntp
```

I would distinguish between:

```dockerfile
EXPOSE 8080
```

and actual port publishing:

```bash
docker run -p 8080:8080 myapp
```

`EXPOSE` documents the container port; it does not itself publish the port to the host.

---

## Q44. A containerized Node.js application cannot find the `express` module. What would you check?

**Answer:**

I would check whether dependencies are installed.

```bash
docker exec -it container_name sh
```

Then:

```bash
npm list express
```

Check:

```bash
cat package.json
```

The Dockerfile should normally install dependencies before starting the application.

Example:

```dockerfile
COPY package*.json ./
RUN npm install

COPY . .
```

Then rebuild:

```bash
docker build --no-cache -t myapp .
```

---

## Q45. Your Docker container cannot access a file copied into it. How would you troubleshoot?

**Answer:**

Check the Dockerfile:

```dockerfile
COPY index.html /app/
```

Then inspect the container:

```bash
docker exec -it container_name sh
```

Check:

```bash
ls -la /app
```

I would verify:

* `COPY` source path
* Build context
* `.dockerignore`
* Destination path
* File permissions

A common problem is that `.dockerignore` accidentally excludes the file.

---

## Q46. You need to persist MySQL data even if the container is deleted. What would you do?

**Answer:**

Use a Docker volume.

```bash
docker volume create mysql-data
```

Run:

```bash
docker run -d \
  --name mysql \
  -v mysql-data:/var/lib/mysql \
  mysql:8
```

Now the database data is stored in the Docker volume instead of only inside the container's writable layer.

For production, I would also consider using a managed database service rather than relying on a single Docker host for database durability.

---

## Q47. Your container is running as root. Is that a security problem?

**Answer:**

Running applications as root inside containers increases the potential impact of a container compromise.

I would prefer a non-root user where possible.

Example:

```dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

USER node

CMD ["npm", "start"]
```

Additional security practices include:

* Minimal base images
* Read-only filesystems where possible
* Dropping unnecessary Linux capabilities
* Resource limits
* Vulnerability scanning
* Keeping images updated

---

## Q48. You need to deploy a frontend and backend using Docker Compose. How would you design it?

**Answer:**

I would separate the services.

Example:

```yaml
services:

  frontend:
    build: ./frontend
    ports:
      - "80:80"
    depends_on:
      - backend

  backend:
    build: ./backend
    environment:
      DB_HOST: database
    depends_on:
      - database

  database:
    image: mysql:8
    volumes:
      - db-data:/var/lib/mysql

volumes:
  db-data:
```

Architecture:

```text
Internet
   |
   v
Frontend
   |
   v
Backend
   |
   v
Database
```

I would also configure health checks and secrets appropriately for a production deployment.

---

## Q49. Your production Docker container has been running for several months and is becoming difficult to manage. What improvements would you make?

**Answer:**

I would improve the deployment process rather than manually managing containers.

I would introduce:

* CI/CD
* Image versioning
* Automated builds
* Vulnerability scanning
* Centralized logging
* Monitoring
* Health checks
* Resource limits
* Secrets management
* Automated rollback

For larger production environments, I would consider an orchestrator such as Kubernetes.

Example workflow:

```text
GitHub
   |
   v
Jenkins
   |
   v
Docker Build
   |
   v
Security Scan
   |
   v
Docker Registry
   |
   v
Kubernetes / Production
```

---

## Q50. Explain a production-ready Docker CI/CD workflow.

**Answer:**

A typical workflow would be:

```text
Developer
    |
    v
GitHub
    |
    v
Jenkins
    |
    +----> Unit Tests
    |
    +----> Docker Build
    |
    +----> Security Scan
    |
    v
Docker Registry
    |
    v
Deployment
    |
    v
Production
```

### Example Jenkins stages

```text
1. Checkout
2. Test
3. Docker Build
4. Docker Image Scan
5. Docker Login
6. Docker Push
7. Deploy
8. Health Check
9. Notify
```

Example Docker commands:

```bash
docker build -t myapp:${BUILD_NUMBER} .
```

Tag:

```bash
docker tag myapp:${BUILD_NUMBER} username/myapp:${BUILD_NUMBER}
```

Push:

```bash
docker push username/myapp:${BUILD_NUMBER}
```

Then deploy the specific version.

Using immutable version tags such as:

```text
myapp:42
```

is generally safer than deploying only:

```text
myapp:latest
```

because the exact image version can be identified and rolled back.

---

# 🛠️ Docker Troubleshooting Cheat Sheet

## Check Docker version

```bash
docker version
```

## Check Docker information

```bash
docker info
```

## Check running containers

```bash
docker ps
```

## Check all containers

```bash
docker ps -a
```

## List images

```bash
docker images
```

## Pull image

```bash
docker pull nginx
```

## Build image

```bash
docker build -t myapp .
```

## Run container

```bash
docker run -d --name myapp myapp:latest
```

## Run with port mapping

```bash
docker run -d -p 8080:80 nginx
```

## View logs

```bash
docker logs myapp
```

## Follow logs

```bash
docker logs -f myapp
```

## Execute command inside container

```bash
docker exec -it myapp sh
```

## Inspect container

```bash
docker inspect myapp
```

## Check resource usage

```bash
docker stats
```

## Check container processes

```bash
docker top myapp
```

## Stop container

```bash
docker stop myapp
```

## Start container

```bash
docker start myapp
```

## Restart container

```bash
docker restart myapp
```

## Remove container

```bash
docker rm myapp
```

## Remove image

```bash
docker rmi myapp:latest
```

## List networks

```bash
docker network ls
```

## List volumes

```bash
docker volume ls
```

## Check Docker disk usage

```bash
docker system df
```

## Clean unused containers

```bash
docker container prune
```

## Clean unused images

```bash
docker image prune
```

## Clean unused Docker resources

```bash
docker system prune
```

⚠️ Always review what will be deleted before running cleanup commands in production.

---

# 🧠 Docker Interview Troubleshooting Framework

When an interviewer gives you a Docker production problem, follow this sequence:

```text
1. Understand the problem
        ↓
2. Check container status
        ↓
3. Check logs
        ↓
4. Inspect container configuration
        ↓
5. Check ports
        ↓
6. Check networking
        ↓
7. Check CPU / Memory
        ↓
8. Check volumes
        ↓
9. Identify root cause
        ↓
10. Fix the problem
        ↓
11. Verify the application
        ↓
12. Add preventive measures
```

### Important commands

```bash
docker ps -a
docker logs <container>
docker inspect <container>
docker stats
docker top <container>
docker exec -it <container> sh
docker network inspect <network>
docker system df
```

---

# 🎯 Docker Interview Topics Checklist

Before attending a DevOps interview, make sure you understand:

* [ ] Docker architecture
* [ ] Docker Engine
* [ ] Images
* [ ] Containers
* [ ] Dockerfile
* [ ] Docker image layers
* [ ] Docker cache
* [ ] CMD
* [ ] ENTRYPOINT
* [ ] RUN
* [ ] COPY
* [ ] ADD
* [ ] WORKDIR
* [ ] EXPOSE
* [ ] ENV
* [ ] ARG
* [ ] USER
* [ ] HEALTHCHECK
* [ ] Volumes
* [ ] Bind mounts
* [ ] Docker networking
* [ ] Port mapping
* [ ] Docker Compose
* [ ] Docker Hub
* [ ] Docker Registry
* [ ] Multi-stage builds
* [ ] Resource limits
* [ ] Docker logging
* [ ] Docker security
* [ ] Docker troubleshooting
* [ ] Docker with Jenkins
* [ ] Docker with Kubernetes
* [ ] Docker in AWS
* [ ] Docker image optimization
* [ ] Docker CI/CD

---

# ⭐ Interview Tip

For scenario-based Docker questions, avoid answering with only a command.

Use this structure:

```text
Understand
   ↓
Check
   ↓
Find root cause
   ↓
Fix
   ↓
Verify
   ↓
Prevent
```

For example:

> "First I would check the container status using `docker ps -a`. Then I would inspect the application logs using `docker logs`. After identifying the root cause, I would fix the configuration or application issue, restart the container, verify the service, and finally add monitoring or health checks to prevent the issue from happening again."

This style demonstrates **real-world DevOps troubleshooting ability**, not just Docker command knowledge.
