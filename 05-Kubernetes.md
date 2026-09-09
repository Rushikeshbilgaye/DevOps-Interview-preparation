# Kubernetes Interview Questions & Answers

## 📌 Overview

This document contains **50 Kubernetes interview questions and answers** for DevOps engineers.

### Structure

* **20 Normal Interview Questions**
* **30 Scenario-Based Interview Questions**
* Beginner → Intermediate → Advanced
* Practical `kubectl` commands
* Production troubleshooting
* Kubernetes networking
* Deployments and Services
* ConfigMaps and Secrets
* Storage
* AWS EKS
* High availability and self-healing

---

# 🟢 PART 1 — NORMAL INTERVIEW QUESTIONS

## Q1. What is Kubernetes?

**Answer:**

Kubernetes is an open-source container orchestration platform used to deploy, manage, scale, and automate containerized applications.

Kubernetes provides features such as:

* Container orchestration
* Automatic scaling
* Self-healing
* Service discovery
* Load balancing
* Rolling updates
* Rollbacks
* Configuration management
* Secret management
* Storage orchestration

Example architecture:

```text
Developer
    |
    v
Docker Image
    |
    v
Kubernetes Cluster
    |
    +---- Pod
    |
    +---- Service
    |
    +---- Deployment
    |
    +---- Ingress
```

---

## Q2. What is a Kubernetes cluster?

**Answer:**

A Kubernetes cluster is a collection of machines used to run containerized applications.

A cluster contains:

```text
Kubernetes Cluster
       |
       +---- Control Plane
       |
       +---- Worker Node
       |
       +---- Worker Node
       |
       +---- Worker Node
```

The **control plane** manages the cluster, while **worker nodes** run application workloads.

---

## Q3. What is a Pod?

**Answer:**

A Pod is the smallest deployable unit in Kubernetes.

A Pod usually contains one application container, although it can contain multiple tightly coupled containers.

Example:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80
```

Create it:

```bash
kubectl apply -f pod.yaml
```

Check:

```bash
kubectl get pods
```

---

## Q4. What is a Node?

**Answer:**

A Node is a machine that runs Kubernetes workloads.

It can be:

* Physical server
* Virtual machine
* AWS EC2 instance

A worker node normally contains:

* kubelet
* container runtime
* kube-proxy

Check nodes:

```bash
kubectl get nodes
```

Detailed information:

```bash
kubectl describe node <node-name>
```

---

## Q5. What is the Kubernetes Control Plane?

**Answer:**

The control plane manages the Kubernetes cluster.

Major components include:

### API Server

Handles Kubernetes API requests.

### etcd

Stores Kubernetes cluster state.

### Scheduler

Decides which node should run a Pod.

### Controller Manager

Runs controllers that continuously reconcile desired and actual state.

Typical architecture:

```text
                Control Plane
                     |
        +------------+------------+
        |            |            |
    API Server    Scheduler    Controllers
        |
       etcd
        |
        v
   Worker Nodes
```

---

## Q6. What is kubelet?

**Answer:**

`kubelet` is an agent running on every Kubernetes worker node.

Its responsibilities include:

* Communicating with the API server
* Starting containers
* Monitoring Pods
* Reporting node and Pod status

You can check kubelet on Linux:

```bash
systemctl status kubelet
```

---

## Q7. What is a Deployment?

**Answer:**

A Deployment manages a set of identical Pods.

It provides:

* Replica management
* Rolling updates
* Rollbacks
* Self-healing
* Declarative deployment

Example:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
```

Check:

```bash
kubectl get deployments
```

---

## Q8. What is a ReplicaSet?

**Answer:**

A ReplicaSet ensures that a specified number of Pod replicas are running.

For example:

```yaml
replicas: 3
```

Kubernetes attempts to maintain:

```text
3 Pods running
```

If one Pod fails:

```text
3 Pods
  |
  +--- Pod 1
  +--- Pod 2
  +--- Pod 3
          X
        Failed
          |
          v
     New Pod Created
```

Deployments normally manage ReplicaSets rather than creating ReplicaSets directly.

---

## Q9. What is a Kubernetes Service?

**Answer:**

A Service provides stable networking access to a set of Pods.

Pods are temporary and their IP addresses can change.

A Service provides a stable endpoint.

Common Service types:

* ClusterIP
* NodePort
* LoadBalancer
* ExternalName

Example:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
```

---

## Q10. What is the difference between ClusterIP, NodePort, and LoadBalancer?

**Answer:**

### ClusterIP

Default Service type.

Accessible inside the cluster.

```yaml
type: ClusterIP
```

### NodePort

Exposes the Service on a port on each node.

```yaml
type: NodePort
```

### LoadBalancer

Requests an external load balancer from the underlying cloud platform.

```yaml
type: LoadBalancer
```

Typical AWS architecture:

```text
Internet
   |
   v
AWS Load Balancer
   |
   v
Kubernetes Service
   |
   v
Pods
```

---

## Q11. What is a Namespace?

**Answer:**

A Namespace logically separates resources inside a Kubernetes cluster.

For example:

```text
Cluster
 |
 +-- development
 |
 +-- testing
 |
 +-- production
```

Create:

```bash
kubectl create namespace dev
```

Deploy into it:

```bash
kubectl apply -f deployment.yaml -n dev
```

List:

```bash
kubectl get pods -n dev
```

---

## Q12. What is ConfigMap?

**Answer:**

A ConfigMap stores non-sensitive configuration data.

Example:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_ENV: production
  APP_PORT: "8080"
```

Use it inside a Pod:

```yaml
envFrom:
  - configMapRef:
      name: app-config
```

Do not store passwords or sensitive credentials in ConfigMaps.

---

## Q13. What is a Kubernetes Secret?

**Answer:**

A Secret is designed to hold sensitive configuration such as:

* Passwords
* API tokens
* Certificates
* Credentials

Example:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
stringData:
  username: admin
  password: mypassword
```

Important: Kubernetes Secrets are not automatically equivalent to a fully secure external secrets-management system. In production, protect access carefully and consider external secret-management solutions.

---

## Q14. What is a Namespace vs Node?

**Answer:**

| Namespace                  | Node                       |
| -------------------------- | -------------------------- |
| Logical isolation          | Compute machine            |
| Used to organize resources | Runs workloads             |
| Exists inside cluster      | Can be VM/physical machine |
| Does not provide compute   | Provides compute resources |

Example:

```text
Cluster
 |
 +-- Namespace: Production
 |       |
 |       +-- Pods
 |
 +-- Nodes
         |
         +-- Pod
         +-- Pod
```

---

## Q15. What is Ingress?

**Answer:**

Ingress provides HTTP/HTTPS routing into Kubernetes services.

Example:

```text
Internet
   |
   v
Ingress
   |
   +---- /api ---> Backend Service
   |
   +---- / ---> Frontend Service
```

Example:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-ingress
spec:
  rules:
    - host: example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: frontend
                port:
                  number: 80
```

Ingress requires an appropriate **Ingress controller**.

---

## Q16. What is a PersistentVolume?

**Answer:**

A PersistentVolume, or PV, represents storage available to Kubernetes workloads.

It provides persistent storage independent of a Pod's lifecycle.

```text
Pod
 |
 v
PVC
 |
 v
PV
 |
 v
Storage
```

Examples of underlying storage include:

* AWS EBS
* NFS
* Cloud provider storage

---

## Q17. What is a PersistentVolumeClaim?

**Answer:**

A PersistentVolumeClaim, or PVC, is a request for storage by an application.

Example:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: app-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
```

Check:

```bash
kubectl get pvc
```

---

## Q18. What is a StatefulSet?

**Answer:**

A StatefulSet is used for stateful applications that need stable identity and persistent storage.

Common examples:

* Databases
* Kafka
* ZooKeeper
* Stateful messaging systems

StatefulSets provide:

* Stable Pod names
* Stable network identity
* Ordered deployment
* Persistent storage association

Example:

```text
mysql-0
mysql-1
mysql-2
```

Unlike ordinary Deployment Pods, these identities are stable.

---

## Q19. What is a DaemonSet?

**Answer:**

A DaemonSet ensures that a Pod runs on each eligible node.

Common use cases:

* Log collection
* Monitoring agents
* Node-level security agents
* Network agents

Example:

```text
Node 1 → Log Agent
Node 2 → Log Agent
Node 3 → Log Agent
```

Check:

```bash
kubectl get daemonsets
```

---

## Q20. What is Horizontal Pod Autoscaler?

**Answer:**

Horizontal Pod Autoscaler, or HPA, automatically adjusts the number of Pod replicas based on metrics such as CPU or memory utilization.

Example:

```bash
kubectl autoscale deployment nginx \
  --cpu-percent=70 \
  --min=2 \
  --max=10
```

Check:

```bash
kubectl get hpa
```

Architecture:

```text
Traffic
   |
   v
Pods
   |
CPU/Memory increases
   |
   v
HPA
   |
   v
More Pods
```

---

# 🔴 PART 2 — SCENARIO-BASED INTERVIEW QUESTIONS

## Q21. A Kubernetes Pod is in `Pending` state. How would you troubleshoot it?

**Answer:**

First:

```bash
kubectl get pods
```

Then:

```bash
kubectl describe pod <pod-name>
```

Look at the Events section.

Common causes:

* Insufficient CPU
* Insufficient memory
* Node selector mismatch
* Taints and tolerations
* PVC not available
* Scheduling constraints

Check nodes:

```bash
kubectl get nodes
```

Check resources:

```bash
kubectl describe nodes
```

I would identify the scheduling reason, correct it, and verify that the Pod becomes `Running`.

---

## Q22. A Pod is in `CrashLoopBackOff`. What would you do?

**Answer:**

First check:

```bash
kubectl get pods
```

Then:

```bash
kubectl logs <pod-name>
```

If the container restarted:

```bash
kubectl logs <pod-name> --previous
```

Then inspect:

```bash
kubectl describe pod <pod-name>
```

Common causes:

* Application crash
* Incorrect environment variable
* Missing configuration
* Wrong command
* Dependency failure
* Health probe failure
* Insufficient resources

I would identify the root cause from logs and events rather than simply restarting the Pod repeatedly.

---

## Q23. A Pod is stuck in `ImagePullBackOff`. What could be wrong?

**Answer:**

Check:

```bash
kubectl describe pod <pod-name>
```

Look at the Events section.

Possible causes:

* Wrong image name
* Wrong image tag
* Image does not exist
* Private registry authentication failure
* Network problem
* Registry rate limit

Test the image:

```bash
docker pull <image>
```

For a private registry, configure an appropriate image pull secret and reference it from the workload.

---

## Q24. Your Pod is running, but the application is not accessible through the Service. How do you troubleshoot?

**Answer:**

First:

```bash
kubectl get svc
```

Then:

```bash
kubectl get endpoints
```

or:

```bash
kubectl get endpointslices
```

I would verify that the Service selector matches the Pod labels.

For example:

```yaml
selector:
  app: backend
```

The Pod should have:

```yaml
labels:
  app: backend
```

Then check the target port:

```bash
kubectl describe service <service-name>
```

Finally test connectivity from inside the cluster.

---

## Q25. A Kubernetes Service has no endpoints. What does that mean?

**Answer:**

It usually means the Service cannot find matching Pods.

Check:

```bash
kubectl get pods --show-labels
```

Then:

```bash
kubectl describe service <service-name>
```

Suppose Service has:

```yaml
selector:
  app: frontend
```

But Pod has:

```yaml
labels:
  app: backend
```

The Service will have no matching endpoints.

I would correct the labels/selectors and verify:

```bash
kubectl get endpointslices
```

---

## Q26. A Deployment is supposed to have 3 replicas, but only 1 Pod is running. What will you check?

**Answer:**

Run:

```bash
kubectl get deployment
```

Then:

```bash
kubectl describe deployment <deployment-name>
```

Check:

```bash
kubectl get replicasets
```

And:

```bash
kubectl get pods
```

Possible reasons:

* Insufficient node resources
* Image pull failures
* Pod crashes
* Scheduling constraints
* Readiness issues
* Resource quotas

I would follow the chain:

```text
Deployment
   ↓
ReplicaSet
   ↓
Pods
   ↓
Pod Events / Logs
```

---

## Q27. Your application is running but receives frequent traffic failures. What would you investigate?

**Answer:**

I would check:

```bash
kubectl get pods
kubectl get svc
kubectl get endpointslices
```

Then:

```bash
kubectl describe pod <pod-name>
```

I would investigate:

* Readiness probe
* Service endpoints
* Application logs
* CPU/memory pressure
* Network policies
* Ingress/load balancer
* Application response time

If Pods are overloaded, I would consider HPA and appropriate resource requests/limits.

---

## Q28. A Kubernetes node is showing `NotReady`. What would you do?

**Answer:**

First:

```bash
kubectl get nodes
```

Then:

```bash
kubectl describe node <node-name>
```

I would check node conditions and events.

On the node:

```bash
systemctl status kubelet
```

Check container runtime:

```bash
systemctl status containerd
```

I would also check:

```bash
df -h
free -h
```

Possible causes:

* kubelet failure
* Container runtime failure
* Disk pressure
* Memory pressure
* Network problem
* Node connectivity issue

---

## Q29. A node is running out of disk space. What will you check?

**Answer:**

On the node:

```bash
df -h
```

Then:

```bash
du -sh /var/lib/containerd/*
```

Depending on the runtime, inspect container/image storage.

I would also check Kubernetes events:

```bash
kubectl describe node <node-name>
```

Possible causes:

* Too many images
* Container logs
* Application-generated files
* Temporary files

I would clean unused resources carefully and investigate why disk usage grew.

For production, I would also configure monitoring and appropriate node disk capacity.

---

## Q30. A Kubernetes Pod is using too much memory and gets killed. What is happening?

**Answer:**

Check:

```bash
kubectl describe pod <pod-name>
```

Look for:

```text
OOMKilled
```

I would check resource configuration:

```yaml
resources:
  requests:
    memory: "256Mi"
  limits:
    memory: "512Mi"
```

Then check actual usage:

```bash
kubectl top pod <pod-name>
```

If the application legitimately requires more memory, increase the limit appropriately.

If usage is abnormal, investigate the application for a memory leak.

---

## Q31. A Pod is running but Kubernetes is not sending traffic to it. What could be wrong?

**Answer:**

The most common reason is a failing **readiness probe**.

Check:

```bash
kubectl describe pod <pod-name>
```

Look for readiness status.

Example:

```yaml
readinessProbe:
  httpGet:
    path: /health
    port: 8080
```

If the application does not respond successfully to `/health`, Kubernetes may keep the Pod out of Service endpoints.

I would test the endpoint inside the Pod and fix the probe or application.

---

## Q32. Your Pod keeps restarting even though the application itself looks healthy. What would you investigate?

**Answer:**

I would inspect the probes.

```bash
kubectl describe pod <pod-name>
```

Check:

* Liveness probe
* Readiness probe
* Startup probe

A badly configured liveness probe can cause Kubernetes to repeatedly restart a healthy but slow-starting application.

For slow-starting applications, a startup probe can prevent liveness checks from killing the container too early.

---

## Q33. Your Deployment update caused problems in production. How would you roll back?

**Answer:**

Check rollout history:

```bash
kubectl rollout history deployment/<deployment-name>
```

Rollback:

```bash
kubectl rollout undo deployment/<deployment-name>
```

Check:

```bash
kubectl rollout status deployment/<deployment-name>
```

Verify:

```bash
kubectl get pods
```

For a specific revision:

```bash
kubectl rollout undo deployment/<deployment-name> --to-revision=2
```

After recovery, I would investigate why the new version failed before redeploying it.

---

## Q34. A Kubernetes Deployment is stuck during a rolling update. How do you troubleshoot it?

**Answer:**

Check:

```bash
kubectl rollout status deployment/<deployment-name>
```

Then:

```bash
kubectl get pods
```

And:

```bash
kubectl describe deployment <deployment-name>
```

I would inspect:

* New Pod status
* Image pull errors
* Readiness probes
* Resource availability
* Deployment strategy
* ReplicaSet events

Then:

```bash
kubectl get replicasets
```

If necessary, rollback:

```bash
kubectl rollout undo deployment/<deployment-name>
```

---

## Q35. You changed a ConfigMap, but the application is still using the old configuration. Why?

**Answer:**

It depends on how the ConfigMap is consumed.

If configuration is injected as environment variables, existing containers generally keep their original environment values.

Restart the Pods:

```bash
kubectl rollout restart deployment/<deployment-name>
```

Then verify:

```bash
kubectl get pods
```

If the ConfigMap is mounted as a volume, updates can be reflected in the mounted files, subject to Kubernetes update behavior and application behavior.

---

## Q36. A Pod cannot connect to a database in another namespace. What would you check?

**Answer:**

I would verify the Service and DNS name.

For example:

```text
database.production.svc.cluster.local
```

Check:

```bash
kubectl get svc -n production
```

Then test DNS from the application Pod:

```bash
kubectl exec -it <pod-name> -- nslookup database.production.svc.cluster.local
```

I would also check:

* NetworkPolicy
* Service endpoints
* Database port
* Credentials
* Firewall/security controls if applicable

---

## Q37. Two Pods cannot communicate with each other. How would you troubleshoot?

**Answer:**

First verify both Pods:

```bash
kubectl get pods -o wide
```

Check Services:

```bash
kubectl get svc
```

Then test connectivity from one Pod:

```bash
kubectl exec -it <pod-name> -- sh
```

I would check:

* Service DNS
* Service selector
* Endpoints/EndpointSlices
* NetworkPolicy
* Application listening port
* Cluster networking

If a NetworkPolicy exists, I would verify that it permits the required traffic.

---

## Q38. An Ingress is returning 404. What would you check?

**Answer:**

First:

```bash
kubectl get ingress
```

Then:

```bash
kubectl describe ingress <ingress-name>
```

Check:

* Hostname
* Path
* Path type
* Backend Service
* Backend port
* Ingress controller
* DNS

Then verify the Service:

```bash
kubectl get svc
kubectl get endpointslices
```

A common problem is an incorrect path or Service name.

---

## Q39. Your LoadBalancer Service remains in `Pending` state on AWS EKS. What would you investigate?

**Answer:**

Check:

```bash
kubectl get svc
kubectl describe svc <service-name>
```

Look at Events.

I would investigate:

* AWS cloud integration
* IAM permissions
* Subnet configuration
* Security groups
* Load balancer controller requirements
* Service annotations
* Cluster networking

In EKS, the exact load balancer behavior depends on the AWS integration and controller configuration being used.

---

## Q40. Your Kubernetes application needs a database password. Where should you store it?

**Answer:**

I would not hard-code it in the Deployment YAML or Docker image.

At minimum, Kubernetes Secret can be used:

```yaml
env:
  - name: DB_PASSWORD
    valueFrom:
      secretKeyRef:
        name: db-secret
        key: password
```

For production environments, I would consider integrating Kubernetes with an external secret-management system such as AWS Secrets Manager or another enterprise secret store.

Access should be controlled using RBAC.

---

## Q41. A Pod needs persistent storage. How would you configure it?

**Answer:**

I would normally use:

```text
Pod
 |
 v
PVC
 |
 v
PV / StorageClass
 |
 v
Storage Backend
```

Check StorageClasses:

```bash
kubectl get storageclass
```

Create a PVC:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: app-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
```

Then mount it into the Pod.

---

## Q42. A PVC is stuck in `Pending`. What would you check?

**Answer:**

First:

```bash
kubectl get pvc
```

Then:

```bash
kubectl describe pvc <pvc-name>
```

I would check:

* StorageClass
* Requested capacity
* Access mode
* Available PVs
* CSI driver
* Cloud provider configuration
* Zone constraints

Check:

```bash
kubectl get storageclass
kubectl get pv
```

The Events section usually provides the most useful clue.

---

## Q43. A Kubernetes application works in development but fails in production. What would you compare?

**Answer:**

I would compare:

* Kubernetes version
* Container image
* ConfigMaps
* Secrets
* Environment variables
* Resource limits
* Service configuration
* Ingress
* NetworkPolicy
* Storage
* IAM permissions
* DNS

Useful commands:

```bash
kubectl get all -n production
kubectl describe pod <pod-name> -n production
kubectl logs <pod-name> -n production
```

I would avoid guessing and compare the actual running configuration between environments.

---

## Q44. How would you scale an application manually?

**Answer:**

Use:

```bash
kubectl scale deployment nginx \
  --replicas=5
```

Verify:

```bash
kubectl get deployment
kubectl get pods
```

For automatic scaling, configure HPA:

```bash
kubectl autoscale deployment nginx \
  --min=2 \
  --max=10 \
  --cpu-percent=70
```

---

## Q45. Your Pods are not distributed across nodes as expected. What Kubernetes features would you investigate?

**Answer:**

I would investigate:

* Pod affinity
* Pod anti-affinity
* Topology spread constraints
* Node selectors
* Node affinity
* Taints and tolerations

For example, Pod anti-affinity can help prevent replicas from being placed on the same node.

This is useful for high availability.

---

## Q46. One worker node fails. What happens to the application?

**Answer:**

Kubernetes detects that the node is unhealthy.

If workloads are managed by a Deployment and sufficient healthy nodes/resources exist, Kubernetes can recreate/reschedule Pods on other nodes.

For example:

```text
Before:

Node 1 → Pod A
Node 2 → Pod B
Node 3 → Pod C

Node 2 fails

After:

Node 1 → Pod A
Node 3 → Pod C
Node 1/3 → Replacement Pod
```

The exact behavior depends on workload configuration, storage, scheduling constraints, and cluster capacity.

This is one reason replicas should be distributed across failure domains.

---

## Q47. How would you secure a Kubernetes cluster?

**Answer:**

I would use multiple security layers:

### 1. RBAC

Give users and workloads only required permissions.

### 2. Network Policies

Restrict Pod-to-Pod traffic.

### 3. Secrets Management

Protect sensitive information.

### 4. Image Security

Scan images for vulnerabilities.

### 5. Pod Security

Avoid unnecessary root privileges and capabilities.

### 6. API Security

Protect the Kubernetes API server.

### 7. Node Security

Patch and harden worker nodes.

### 8. Monitoring

Monitor suspicious activity and cluster health.

Security should follow the principle of least privilege.

---

## Q48. Your Kubernetes cluster is running out of CPU during a traffic spike. What would you do?

**Answer:**

First check:

```bash
kubectl top nodes
kubectl top pods
```

Then determine whether the problem is:

* Pod CPU usage
* Insufficient node capacity
* Too many workloads
* Poor resource requests/limits

For application scaling, use HPA:

```text
Traffic
   |
   v
CPU increases
   |
   v
HPA
   |
   v
More Pods
```

If the cluster itself needs more capacity, use node autoscaling or add worker nodes.

In AWS EKS, I would evaluate the node group capacity and an appropriate node autoscaling solution.

---

## Q49. How would you perform a zero-downtime application deployment in Kubernetes?

**Answer:**

I would use a Deployment with multiple replicas and a rolling update strategy.

Example:

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 0
    maxSurge: 1
```

I would also configure:

* Readiness probe
* Liveness probe
* Startup probe where required
* Multiple replicas
* Proper resource requests
* Pod disruption strategy
* Load balancing

Deployment flow:

```text
Old Pods
   |
   v
New Pod starts
   |
   v
Readiness check passes
   |
   v
Traffic goes to new Pod
   |
   v
Old Pod removed
```

Then monitor:

```bash
kubectl rollout status deployment/<deployment-name>
```

---

## Q50. Explain a production-ready Kubernetes deployment architecture on AWS EKS.

**Answer:**

A typical production architecture could look like:

```text
                    Internet
                       |
                       v
                Route 53 / DNS
                       |
                       v
              AWS Load Balancer
                       |
                       v
              Kubernetes Ingress
                       |
             +---------+---------+
             |                   |
             v                   v
       Frontend Service    Backend Service
             |                   |
             v                   v
          Pods                 Pods
             |                   |
             +---------+---------+
                       |
                       v
                 Database
```

The EKS cluster could contain:

```text
AWS VPC
 |
 +---- Private Subnets
 |       |
 |       +---- EKS Worker Nodes
 |              |
 |              +---- Frontend Pods
 |              +---- Backend Pods
 |
 +---- Public Subnets
        |
        +---- Load Balancer
```

For a production environment, I would consider:

### Application

* Kubernetes Deployments
* Services
* Ingress
* ConfigMaps
* Secrets
* HPA
* Readiness/liveness/startup probes

### Infrastructure

* Multi-AZ node groups
* Private worker nodes
* Appropriate IAM permissions
* Security groups
* Network policies
* Autoscaling

### CI/CD

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
Image Scan
   |
   v
Amazon ECR
   |
   v
EKS Deployment
```

### Monitoring

I would use metrics, logs, and alerts to monitor:

* CPU
* Memory
* Pod restarts
* Node health
* Application latency
* Error rates
* Deployment status

The goal is not just to deploy the application, but to make the platform **scalable, observable, secure, and recoverable**.

---

# 🛠️ Kubernetes Troubleshooting Cheat Sheet

## Check cluster information

```bash
kubectl cluster-info
```

## Check Kubernetes version

```bash
kubectl version
```

## Check nodes

```bash
kubectl get nodes
```

## Detailed node information

```bash
kubectl describe node <node-name>
```

## Check Pods

```bash
kubectl get pods
```

## Check Pods in all namespaces

```bash
kubectl get pods -A
```

## Detailed Pod information

```bash
kubectl describe pod <pod-name>
```

## View Pod logs

```bash
kubectl logs <pod-name>
```

## View previous container logs

```bash
kubectl logs <pod-name> --previous
```

## Follow logs

```bash
kubectl logs -f <pod-name>
```

## Enter a container

```bash
kubectl exec -it <pod-name> -- sh
```

## Check Deployments

```bash
kubectl get deployments
```

## Check ReplicaSets

```bash
kubectl get replicasets
```

## Check Services

```bash
kubectl get svc
```

## Check Endpoints

```bash
kubectl get endpoints
```

## Check EndpointSlices

```bash
kubectl get endpointslices
```

## Check Ingress

```bash
kubectl get ingress
```

## Check ConfigMaps

```bash
kubectl get configmaps
```

## Check Secrets

```bash
kubectl get secrets
```

## Check Persistent Volumes

```bash
kubectl get pv
```

## Check Persistent Volume Claims

```bash
kubectl get pvc
```

## Check StorageClasses

```bash
kubectl get storageclass
```

## Check resource usage

```bash
kubectl top nodes
```

```bash
kubectl top pods
```

## Scale Deployment

```bash
kubectl scale deployment <deployment-name> --replicas=5
```

## Check rollout

```bash
kubectl rollout status deployment/<deployment-name>
```

## Rollback Deployment

```bash
kubectl rollout undo deployment/<deployment-name>
```

## Restart Deployment

```bash
kubectl rollout restart deployment/<deployment-name>
```

## View rollout history

```bash
kubectl rollout history deployment/<deployment-name>
```

## Apply YAML

```bash
kubectl apply -f deployment.yaml
```

## Delete resource

```bash
kubectl delete -f deployment.yaml
```

---

# 🧠 Kubernetes Troubleshooting Framework

When an interviewer gives you a Kubernetes production problem, follow this process:

```text
1. Understand the problem
        ↓
2. Check cluster health
        ↓
3. Check nodes
        ↓
4. Check Pods
        ↓
5. Check Pod events
        ↓
6. Check application logs
        ↓
7. Check Services
        ↓
8. Check endpoints
        ↓
9. Check networking
        ↓
10. Check resources
        ↓
11. Identify root cause
        ↓
12. Fix the issue
        ↓
13. Verify
        ↓
14. Add preventive measures
```

### Most important commands

```bash
kubectl get nodes
kubectl get pods -A
kubectl describe pod <pod>
kubectl logs <pod>
kubectl logs <pod> --previous
kubectl get svc
kubectl get endpointslices
kubectl get events --sort-by=.lastTimestamp
kubectl top nodes
kubectl top pods
```

---

# 🎯 Kubernetes Interview Topics Checklist

Before attending a DevOps/Kubernetes interview, make sure you understand:

* [ ] Kubernetes architecture
* [ ] Control Plane
* [ ] Worker Nodes
* [ ] API Server
* [ ] etcd
* [ ] Scheduler
* [ ] Controller Manager
* [ ] kubelet
* [ ] kube-proxy
* [ ] Container runtime
* [ ] Pods
* [ ] Deployments
* [ ] ReplicaSets
* [ ] Services
* [ ] ClusterIP
* [ ] NodePort
* [ ] LoadBalancer
* [ ] Namespaces
* [ ] ConfigMaps
* [ ] Secrets
* [ ] Ingress
* [ ] Ingress Controller
* [ ] PersistentVolumes
* [ ] PersistentVolumeClaims
* [ ] StorageClasses
* [ ] StatefulSets
* [ ] DaemonSets
* [ ] Jobs
* [ ] CronJobs
* [ ] HPA
* [ ] Resource Requests
* [ ] Resource Limits
* [ ] Liveness Probe
* [ ] Readiness Probe
* [ ] Startup Probe
* [ ] RBAC
* [ ] NetworkPolicy
* [ ] Node Affinity
* [ ] Pod Affinity
* [ ] Taints and Tolerations
* [ ] Rolling Updates
* [ ] Rollbacks
* [ ] Kubernetes Networking
* [ ] Kubernetes Storage
* [ ] Kubernetes Security
* [ ] Kubernetes Troubleshooting
* [ ] Kubernetes on AWS EKS
* [ ] Kubernetes with Docker
* [ ] Kubernetes with Jenkins
* [ ] Kubernetes with Terraform

---

# ⭐ Interview Tip

For Kubernetes scenario-based questions, don't immediately start typing commands.

Use this approach:

```text
Understand
    ↓
Check
    ↓
Analyze
    ↓
Identify Root Cause
    ↓
Fix
    ↓
Verify
    ↓
Prevent
```

### Example interview answer

**Interviewer:** "Your Pod is in CrashLoopBackOff. What will you do?"

**Strong answer:**

> "First, I would check the Pod status using `kubectl get pods`. Then I would check the application logs using `kubectl logs` and, because the container is restarting, I would also check `kubectl logs --previous`. After that I would use `kubectl describe pod` to inspect events, probes, image configuration, and resource-related failures. Once I identify the root cause, I would fix it, verify that the Pod becomes healthy, and add appropriate monitoring or configuration safeguards to prevent the issue from recurring."

That style shows the interviewer that you understand **Kubernetes troubleshooting**, rather than simply memorizing `kubectl` commands.
