# ☁️ AWS Interview Questions & Answers

This file contains **50 AWS interview questions and answers** for DevOps Engineer interview preparation.

* **20 Normal Questions & Answers**
* **30 Scenario-Based Questions & Answers**

---

# 📚 Part 1 — Normal AWS Interview Questions

## Q1. What is AWS?

**Answer:**

AWS stands for **Amazon Web Services**. It is a cloud computing platform that provides on-demand infrastructure and managed services.

AWS provides services for:

* Compute
* Storage
* Networking
* Databases
* Security
* Monitoring
* Serverless applications
* Containers
* DevOps

Common AWS services used by DevOps engineers include:

```text
EC2
S3
IAM
VPC
ELB
Auto Scaling
RDS
CloudWatch
CloudTrail
Route 53
Lambda
EKS
ECR
SNS
SQS
Systems Manager
```

---

## Q2. What is an AWS Region?

**Answer:**

An AWS Region is a geographical area containing multiple isolated Availability Zones.

For example:

```text
ap-south-1 → Mumbai
us-east-1  → N. Virginia
eu-west-1  → Ireland
```

A Region is selected based on factors such as:

* Latency
* Data residency
* Service availability
* Cost
* Disaster recovery requirements

---

## Q3. What is an Availability Zone?

**Answer:**

An Availability Zone (AZ) is an isolated location within an AWS Region.

A Region normally contains multiple Availability Zones.

For example:

```text
AWS Region
│
├── Availability Zone A
├── Availability Zone B
└── Availability Zone C
```

Using multiple AZs improves:

* High availability
* Fault tolerance
* Disaster resilience

---

## Q4. What is Amazon EC2?

**Answer:**

Amazon EC2 stands for **Elastic Compute Cloud**.

It provides virtual servers in the AWS cloud.

EC2 can be used to run:

* Web applications
* APIs
* Docker containers
* Jenkins
* Monitoring tools
* Backend services

Important EC2 components include:

```text
AMI
Instance Type
EBS
Security Group
Key Pair
Elastic IP
IAM Role
User Data
```

---

## Q5. What is an AMI?

**Answer:**

AMI stands for **Amazon Machine Image**.

It is a template used to launch EC2 instances.

An AMI can contain:

* Operating system
* Application software
* Configuration
* Required packages

For example:

```text
Ubuntu AMI
Amazon Linux AMI
Red Hat AMI
```

---

## Q6. What is an IAM?

**Answer:**

IAM stands for **Identity and Access Management**.

It controls authentication and authorization for AWS resources.

IAM includes:

```text
Users
Groups
Roles
Policies
```

A good security practice is to use **IAM roles for AWS workloads** instead of storing long-term access keys on EC2 instances.

---

## Q7. What is the difference between IAM User and IAM Role?

**Answer:**

An **IAM User** represents a specific identity that can have long-term credentials.

An **IAM Role** provides temporary permissions that can be assumed by AWS services, users, or applications.

For example, instead of putting AWS access keys inside an EC2 application, we can attach an IAM role to the EC2 instance.

```text
EC2
 ↓
IAM Role
 ↓
AWS Permissions
```

This is more secure than hard-coding credentials.

---

## Q8. What is Amazon S3?

**Answer:**

Amazon S3 stands for **Simple Storage Service**.

It is an object storage service used to store data such as:

* Images
* Videos
* Backups
* Logs
* Static websites
* Application artifacts

S3 stores objects inside **buckets**.

Example:

```text
Bucket
│
├── application.zip
├── backup.sql
├── images/
└── logs/
```

---

## Q9. What is an AWS VPC?

**Answer:**

VPC stands for **Virtual Private Cloud**.

It allows you to create a logically isolated network inside AWS.

A VPC can contain:

```text
VPC
│
├── Public Subnet
├── Private Subnet
├── Route Table
├── Internet Gateway
├── NAT Gateway
└── Security Groups
```

VPC is the foundation for networking AWS workloads.

---

## Q10. What is the difference between public and private subnet?

**Answer:**

A **public subnet** has a route to an Internet Gateway.

A **private subnet** does not have a direct route to the Internet Gateway.

Typical architecture:

```text
Internet
   ↓
Load Balancer
   ↓
Public Subnet
   ↓
Private Subnet
   ↓
Application / Database
```

Private subnets are commonly used for application servers and databases that should not be directly accessible from the internet.

---

## Q11. What is a Security Group?

**Answer:**

A Security Group is a virtual firewall associated with resources such as EC2 instances.

It controls inbound and outbound traffic.

Example:

```text
Inbound
22  → SSH
80  → HTTP
443 → HTTPS
```

Security Groups are **stateful**, meaning return traffic is automatically allowed for an allowed connection.

---

## Q12. What is a Network ACL?

**Answer:**

A Network ACL, or NACL, controls traffic at the subnet level.

Unlike Security Groups, NACLs are **stateless**.

They support:

* Allow rules
* Deny rules

Comparison:

| Feature        | Security Group        | NACL                       |
| -------------- | --------------------- | -------------------------- |
| Level          | Instance/resource     | Subnet                     |
| Stateful       | Yes                   | No                         |
| Rules          | Allow                 | Allow + Deny               |
| Return traffic | Automatically allowed | Must be explicitly allowed |

---

## Q13. What is an Elastic Load Balancer?

**Answer:**

Elastic Load Balancing distributes incoming traffic across multiple targets.

Common AWS load balancers include:

```text
Application Load Balancer
Network Load Balancer
Gateway Load Balancer
```

For HTTP/HTTPS applications, an **Application Load Balancer (ALB)** is commonly used.

Example:

```text
Users
  ↓
Load Balancer
  ↓
 ┌───────────────┐
 ↓       ↓       ↓
EC2     EC2     EC2
```

This improves availability and scalability.

---

## Q14. What is Auto Scaling?

**Answer:**

Amazon EC2 Auto Scaling automatically adjusts the number of EC2 instances based on demand or configured policies.

For example:

```text
Normal traffic
    ↓
2 EC2 instances

High traffic
    ↓
5 EC2 instances
```

When demand decreases, instances can be terminated according to the scaling policy.

Benefits include:

* Scalability
* High availability
* Cost optimization

---

## Q15. What is Amazon EBS?

**Answer:**

EBS stands for **Elastic Block Store**.

It provides block storage volumes for EC2 instances.

EBS can be used for:

* Operating system disks
* Application data
* Databases
* Persistent storage

Common EBS volume types include:

```text
gp3
io2
st1
sc1
```

---

## Q16. What is Amazon RDS?

**Answer:**

Amazon RDS is a managed relational database service.

It supports database engines such as:

```text
MySQL
PostgreSQL
MariaDB
Oracle
SQL Server
```

AWS manages many operational tasks such as:

* Automated backups
* Patching
* Monitoring
* Maintenance

RDS can also be configured for high availability using Multi-AZ deployments.

---

## Q17. What is Amazon CloudWatch?

**Answer:**

Amazon CloudWatch is a monitoring and observability service.

It can collect:

* Metrics
* Logs
* Events
* Alarms

For example, we can create an alarm when EC2 CPU utilization exceeds a threshold.

```text
EC2
 ↓
CloudWatch
 ↓
CPU Alarm
 ↓
SNS Notification
```

---

## Q18. What is AWS CloudTrail?

**Answer:**

AWS CloudTrail records API activity and account actions.

It can help answer questions such as:

* Who changed an AWS resource?
* When was it changed?
* Which API call was made?
* From where was the request made?

CloudTrail is useful for:

* Auditing
* Security investigations
* Compliance
* Troubleshooting

---

## Q19. What is AWS Route 53?

**Answer:**

Route 53 is AWS's managed DNS service.

It can be used for:

* Domain registration
* DNS resolution
* Health checks
* Routing traffic

Example:

```text
www.example.com
       ↓
Route 53
       ↓
Load Balancer
       ↓
Application
```

---

## Q20. What is AWS Lambda?

**Answer:**

AWS Lambda is a serverless compute service.

You can run code without managing servers.

A Lambda function can be triggered by events such as:

```text
API Gateway
S3
EventBridge
SQS
SNS
CloudWatch Events
```

Example:

```text
User uploads file
       ↓
S3
       ↓
Lambda
       ↓
Process file
```

---

# 🔥 Part 2 — 30 Scenario-Based AWS Interview Questions

## Q21. Scenario: Your EC2 instance is unreachable through SSH. How will you troubleshoot it?

**Answer:**

I would troubleshoot from the network layer to the instance.

### Step 1 — Check EC2 status

Verify:

* Instance is running
* Status checks are passing

### Step 2 — Check Security Group

Verify inbound SSH:

```text
TCP 22
Source → authorized IP/network
```

### Step 3 — Check Network ACL

Make sure traffic is allowed in both directions as required.

### Step 4 — Check routing

Verify:

* Route table
* Internet Gateway
* Subnet
* Public IP

### Step 5 — Check SSH service

If console/SSM access is available:

```bash
systemctl status ssh
```

or:

```bash
systemctl status sshd
```

### Step 6 — Check authentication

Verify:

* Correct username
* Correct private key
* Key permissions

The important point is to **not immediately assume the problem is the SSH key**. I would first identify where the connection is failing.

---

## Q22. Scenario: Your EC2 instance CPU utilization suddenly reaches 100%. What will you do?

**Answer:**

First check CloudWatch metrics to determine when the issue started.

Then connect to the instance if possible:

```bash
top
```

or:

```bash
ps aux --sort=-%cpu | head
```

I would identify the process consuming CPU.

Then check:

* Application logs
* Traffic levels
* Recent deployments
* Background jobs
* System processes

If traffic has increased significantly, I would consider scaling the application.

After restoring service, I would investigate the root cause and improve monitoring/alerting.

---

## Q23. Scenario: An EC2 instance has run out of disk space. How will you solve it?

**Answer:**

First:

```bash
df -h
```

Then identify large directories:

```bash
du -sh /var/*
```

Find large files:

```bash
find /var -type f -size +500M
```

I would check:

* Application logs
* Docker data
* Temporary files
* Old backups

If additional storage is required, I can increase the EBS volume and then extend the filesystem.

I would also configure proper log rotation and monitoring to prevent recurrence.

---

## Q24. Scenario: An application running on EC2 cannot access S3. What will you check?

**Answer:**

I would check the following:

### 1. IAM Role

Verify whether the EC2 instance has an IAM role attached.

### 2. IAM Policy

Verify the role has required S3 permissions.

For example:

```text
s3:GetObject
s3:PutObject
```

as appropriate.

### 3. Bucket Policy

Check whether the S3 bucket policy denies the request.

### 4. Region and endpoint

Verify the application is using the correct bucket and AWS configuration.

### 5. Network

If the EC2 instance is in a private subnet, verify appropriate connectivity through:

* NAT Gateway, or
* S3 VPC endpoint

I would also check CloudTrail and application logs for authorization errors.

---

## Q25. Scenario: Your website is down, but all EC2 instances are running. What will you check?

**Answer:**

I would troubleshoot the complete request path:

```text
User
 ↓
DNS
 ↓
Load Balancer
 ↓
Target Group
 ↓
EC2
 ↓
Application
```

I would check:

1. Route 53 DNS resolution
2. Load balancer status
3. Target group health
4. Security Groups
5. EC2 application
6. Application port
7. Nginx/Apache status
8. Application logs

For example:

```bash
curl http://localhost:8080
```

If instances are healthy but the target group is unhealthy, I would investigate the load balancer health-check configuration.

---

## Q26. Scenario: ALB target health checks are failing. What will you troubleshoot?

**Answer:**

First identify the health-check configuration:

```text
Protocol
Port
Path
Success codes
Timeout
Interval
```

Then check the application locally:

```bash
curl http://localhost/health
```

I would verify:

* Application is running
* Correct port is listening
* Health endpoint exists
* Security Group allows traffic from the load balancer
* Application returns expected HTTP status
* Network configuration is correct

The health-check path and port must match the application.

---

## Q27. Scenario: Auto Scaling is not launching new EC2 instances even though CPU is high. What will you check?

**Answer:**

I would check:

1. CloudWatch CPU metric
2. Auto Scaling policy
3. Alarm state
4. Launch Template
5. Desired/min/max capacity
6. Subnet availability
7. IAM permissions
8. EC2 service limits
9. Instance launch failures

I would inspect Auto Scaling activity history because it can show why a launch failed.

---

## Q28. Scenario: An EC2 instance is running but cannot access the internet. What will you check?

**Answer:**

For a public subnet, I would verify:

```text
EC2
 ↓
Subnet Route Table
 ↓
Internet Gateway
 ↓
Internet
```

Check:

* Public IPv4 address
* Route table
* Internet Gateway
* Security Group
* NACL

For a private subnet, I would normally check:

```text
Private Subnet
      ↓
NAT Gateway
      ↓
Internet Gateway
      ↓
Internet
```

I would also verify DNS configuration.

---

## Q29. Scenario: An application in a private subnet needs to access S3. What architecture would you use?

**Answer:**

A preferred option is an **S3 VPC endpoint**, where appropriate.

Architecture:

```text
Private EC2
     ↓
Private Subnet
     ↓
S3 VPC Endpoint
     ↓
S3
```

This can avoid sending S3 traffic through a NAT Gateway and can provide private connectivity.

I would configure the endpoint policy and routing appropriately.

---

## Q30. Scenario: An EC2 application cannot connect to an RDS database. How will you troubleshoot?

**Answer:**

I would check:

### Security Group

The RDS Security Group should allow the database port from the application server's Security Group.

For MySQL:

```text
TCP 3306
```

For PostgreSQL:

```text
TCP 5432
```

### Network

Check:

* VPC
* Subnets
* Route tables
* NACLs

### Database

Check:

* RDS status
* Endpoint
* Port
* Credentials
* Database availability

### Connectivity

From the application server:

```bash
nc -zv RDS_ENDPOINT 3306
```

Then investigate application logs for authentication or connection errors.

---

## Q31. Scenario: You accidentally made an S3 bucket publicly accessible. What will you do?

**Answer:**

First I would immediately restrict public access if public access is not required.

I would check:

* S3 Block Public Access
* Bucket policy
* Bucket ACLs where applicable
* Object-level access configuration
* IAM policies

Then I would determine whether any sensitive data was exposed.

I would review CloudTrail/access logs as appropriate and document the incident.

Finally, I would implement preventive controls such as organization-level guardrails and continuous monitoring.

---

## Q32. Scenario: An S3 object was accidentally deleted. How can you recover it?

**Answer:**

Recovery depends on how the bucket was configured.

If **S3 Versioning** was enabled, previous object versions may be recoverable.

I would:

1. Check bucket versioning.
2. Identify the previous object version.
3. Restore/recover the required version.
4. Review who deleted the object using CloudTrail if available.
5. Improve protection if necessary.

For critical data, versioning and appropriate backup/retention strategies should be considered.

---

## Q33. Scenario: Your AWS bill suddenly increased. How will you investigate?

**Answer:**

I would use AWS billing and cost-management tools to identify:

* Which service increased
* Which Region increased
* Which account/resource caused the increase
* When the increase started

Common causes include:

```text
EC2 instances
NAT Gateway
EBS volumes
Data transfer
S3
RDS
Load Balancers
```

I would compare current usage with previous periods.

I would also check CloudTrail for unexpected resource creation and then take corrective action.

---

## Q34. Scenario: An EC2 instance was terminated accidentally. How will you recover?

**Answer:**

First determine whether the instance can be recreated from:

* AMI
* EBS snapshots
* Launch Template
* Auto Scaling Group
* Infrastructure-as-Code such as Terraform

If the data was stored on a separately preserved EBS volume or backed up, it may be possible to restore it.

For future protection:

* Use automated backups
* Use infrastructure as code
* Protect critical resources
* Avoid storing important data only on ephemeral instance storage

---

## Q35. Scenario: You need high availability for an application in AWS. What architecture would you design?

**Answer:**

I would deploy the application across multiple Availability Zones.

Example:

```text
                Internet
                    ↓
              Route 53
                    ↓
              Load Balancer
              /           \
             ↓             ↓
         AZ-1 EC2       AZ-2 EC2
             \             /
              \           /
                 RDS
              Multi-AZ
```

I would use:

* ALB
* Auto Scaling Group
* Multiple AZs
* Multi-AZ database where appropriate
* CloudWatch monitoring

This reduces dependency on a single Availability Zone.

---

## Q36. Scenario: You need to deploy a new application version without downtime. What AWS architecture would you use?

**Answer:**

I could use a load-balanced Auto Scaling architecture with a deployment strategy such as:

```text
Blue-Green Deployment
```

or:

```text
Rolling Deployment
```

For example:

```text
Load Balancer
     ↓
Blue → Current Version

Green → New Version
```

I would test the new version and gradually shift traffic.

If problems occur, traffic can be shifted back to the previous version.

---

## Q37. Scenario: An application is receiving a sudden traffic spike. How will you handle it?

**Answer:**

I would first verify the traffic spike using CloudWatch and load balancer metrics.

Then I would check:

* ALB request count
* CPU
* Memory
* Response time
* Error rate

For scalable applications, I would use:

```text
Route 53
   ↓
ALB
   ↓
Auto Scaling Group
   ↓
EC2
```

Auto Scaling can add instances according to the configured policy.

I would also investigate whether the traffic is legitimate or potentially malicious.

---

## Q38. Scenario: Your application has high latency even though CPU utilization is low. What will you investigate?

**Answer:**

Low CPU does not mean the application is healthy.

I would investigate:

* Network latency
* Database latency
* Disk I/O
* External API calls
* Connection pools
* Load balancer metrics
* Application response time
* DNS
* Cache performance

I would use CloudWatch metrics and application-level monitoring to identify where the latency is occurring.

---

## Q39. Scenario: An employee needs temporary access to an AWS resource. What would you use?

**Answer:**

I would prefer temporary access through an **IAM role** rather than creating a permanent access key.

For example:

```text
Employee
   ↓
Assume IAM Role
   ↓
Temporary Credentials
   ↓
AWS Resource
```

I would follow least privilege and provide only the permissions required.

---

## Q40. Scenario: You find AWS access keys hard-coded inside a Git repository. What will you do?

**Answer:**

I would treat the credentials as compromised.

### Immediate actions

1. Disable or rotate the exposed credentials.
2. Identify where they were used.
3. Check CloudTrail for suspicious activity.
4. Remove the credentials from the repository.
5. Rewrite Git history if required by the incident procedure.

Then replace hard-coded credentials with safer mechanisms such as:

```text
IAM Roles
Secrets Manager
Systems Manager Parameter Store
```

The most important point is **rotating/revoking the exposed credentials**, not just deleting the file from the latest Git commit.

---

## Q41. Scenario: An application running on EC2 needs AWS permissions. Would you create an access key?

**Answer:**

No, my first choice would be an **IAM role attached to the EC2 instance**.

Architecture:

```text
EC2
 ↓
IAM Instance Role
 ↓
Temporary Credentials
 ↓
AWS Service
```

This avoids storing long-term AWS access keys on the server.

---

## Q42. Scenario: An EC2 instance is terminated, but you still need its data. What will you check?

**Answer:**

I would check whether the data was stored on:

* EBS volume
* EBS snapshot
* S3
* EFS
* Database
* Backup system

If an EBS volume was configured to persist after instance termination, it may still be available.

If snapshots exist, I can create a new volume from the snapshot.

This highlights why application data should not depend solely on the lifecycle of an EC2 instance.

---

## Q43. Scenario: CloudWatch shows that an EC2 instance is healthy, but the application is down. Why can this happen?

**Answer:**

EC2 health checks mainly indicate infrastructure/instance health, not whether the application itself is functioning correctly.

The application could be:

* Crashed
* Stuck
* Listening on the wrong port
* Returning errors
* Unable to connect to its database

I would check:

```bash
systemctl status application
ss -tulpn
curl http://localhost:PORT
```

and application logs.

I would also configure application-level health checks and monitoring.

---

## Q44. Scenario: You need centralized logging for multiple EC2 instances. What would you use?

**Answer:**

I would use **Amazon CloudWatch Logs**, with an appropriate log collection mechanism such as the CloudWatch Agent.

Architecture:

```text
EC2-1 ─┐
EC2-2 ─┼──→ CloudWatch Logs
EC2-3 ─┘
```

This allows centralized log collection and searching.

I could then create:

* Metric filters
* Alarms
* Dashboards

for important events.

---

## Q45. Scenario: An EC2 instance needs administrative access, but you don't want to open SSH port 22 to the internet. What would you use?

**Answer:**

I would consider **AWS Systems Manager Session Manager**.

Architecture:

```text
Administrator
      ↓
Systems Manager
      ↓
EC2 Instance
```

This can reduce the need for publicly exposed SSH access.

The instance needs the appropriate IAM role and Systems Manager connectivity/configuration.

---

## Q46. Scenario: Your RDS database is running out of storage. What will you do?

**Answer:**

First check the RDS storage metrics in CloudWatch.

I would investigate:

* Current storage usage
* Database growth
* Logs
* Temporary data
* Large tables

Then increase allocated storage or enable appropriate storage autoscaling if supported and suitable.

I would also investigate why storage is growing and establish monitoring/alarms before it becomes critical.

---

## Q47. Scenario: An application needs secrets such as database passwords. Where should you store them?

**Answer:**

I would not store passwords directly in:

```text
Git
Dockerfile
Source code
AMI
Plain-text configuration
```

Instead, I would consider:

```text
AWS Secrets Manager
AWS Systems Manager Parameter Store
```

The application can retrieve the secret using an IAM role with least-privilege permissions.

---

## Q48. Scenario: You need to restrict an EC2 instance so that only the load balancer can access port 8080. How would you configure it?

**Answer:**

I would configure the EC2 Security Group to allow:

```text
TCP 8080
Source → Load Balancer Security Group
```

rather than allowing:

```text
0.0.0.0/0
```

This creates a controlled traffic path:

```text
Internet
   ↓
ALB
   ↓
EC2:8080
```

The EC2 instance does not need to expose port 8080 directly to the internet.

---

## Q49. Scenario: You need to identify who changed an AWS security group. How will you investigate?

**Answer:**

I would use **AWS CloudTrail**.

I would search for API events related to the security group and identify:

* User or role
* API action
* Timestamp
* Source IP
* Resource
* Request details

For example, I could investigate changes involving:

```text
AuthorizeSecurityGroupIngress
RevokeSecurityGroupIngress
ModifySecurityGroupRules
```

This helps determine who made the change and when.

---

## Q50. Scenario: You are asked to design a production-ready three-tier application on AWS. What architecture would you propose?

**Answer:**

I would design the application using separate presentation, application, and database tiers.

Example:

```text
                         Internet
                            │
                            ↓
                       Route 53
                            │
                            ↓
                      Application
                    Load Balancer
                            │
              ┌─────────────┴─────────────┐
              ↓                           ↓
          Private Subnet              Private Subnet
              │                           │
           EC2 / ECS                  EC2 / ECS
          Application                Application
              │                           │
              └─────────────┬─────────────┘
                            ↓
                     RDS Multi-AZ
```

### Network Design

I would use:

```text
VPC
│
├── Public Subnets
│   └── Load Balancer
│
├── Private Application Subnets
│   └── Application Servers
│
└── Private Database Subnets
    └── RDS
```

### Security

I would use:

* IAM roles
* Least-privilege policies
* Security Groups
* Private subnets
* Secrets Manager
* Encryption
* CloudTrail

### Availability

I would use:

* Multiple Availability Zones
* Auto Scaling
* Load Balancer
* RDS Multi-AZ where appropriate

### Monitoring

I would use:

* CloudWatch
* CloudWatch Logs
* CloudTrail
* Alarms
* Application monitoring

### Deployment

For deployment, I could integrate:

```text
GitHub
   ↓
Jenkins
   ↓
Docker
   ↓
ECR
   ↓
ECS / EKS / EC2
```

The exact deployment platform would depend on the application's requirements.

---

# 🎯 AWS Troubleshooting Cheat Sheet

| Problem                  | First Things to Check                                     |
| ------------------------ | --------------------------------------------------------- |
| EC2 SSH failure          | SG → NACL → Route → Public IP → SSH service               |
| EC2 CPU high             | CloudWatch → `top` → process → logs                       |
| Disk full                | `df -h` → `du` → large files                              |
| Website down             | Route 53 → ALB → Target Group → EC2 → Application         |
| ALB unhealthy            | Health path → Port → SG → Application                     |
| Auto Scaling not working | Alarm → Policy → ASG → Launch Template → Activity History |
| EC2 no internet          | Route Table → IGW/NAT → SG → NACL                         |
| EC2 cannot access S3     | IAM Role → Policy → Bucket Policy → Endpoint/NAT          |
| EC2 cannot access RDS    | SG → Port → Route → RDS Status → Credentials              |
| S3 accidentally public   | Block Public Access → Bucket Policy → ACL → Audit         |
| AWS bill increased       | Cost Explorer → Service → Region → Resource               |
| RDS storage full         | CloudWatch → Storage → DB growth → Increase storage       |
| AWS change investigation | CloudTrail                                                |
| Centralized logs         | CloudWatch Logs                                           |
| Secure server access     | Systems Manager Session Manager                           |

---

# 🧠 AWS Interview Troubleshooting Framework

When an interviewer gives you an AWS production scenario, don't immediately jump to one solution.

Use this approach:

```text
1. Understand the problem
          ↓
2. Identify affected AWS service
          ↓
3. Check CloudWatch metrics/logs
          ↓
4. Check networking
          ↓
5. Check IAM/security
          ↓
6. Check application configuration
          ↓
7. Identify root cause
          ↓
8. Apply the safest fix
          ↓
9. Verify recovery
          ↓
10. Add monitoring/prevention
```

---

# 🎤 Important AWS Interview Topics

Before an interview, make sure you can explain:

* [ ] AWS Regions
* [ ] Availability Zones
* [ ] EC2
* [ ] AMI
* [ ] EBS
* [ ] S3
* [ ] IAM
* [ ] IAM Roles
* [ ] VPC
* [ ] Public/Private Subnets
* [ ] Route Tables
* [ ] Internet Gateway
* [ ] NAT Gateway
* [ ] Security Groups
* [ ] NACL
* [ ] Load Balancers
* [ ] Auto Scaling
* [ ] RDS
* [ ] CloudWatch
* [ ] CloudTrail
* [ ] Route 53
* [ ] Lambda
* [ ] Systems Manager
* [ ] Secrets Manager
* [ ] High Availability
* [ ] Disaster Recovery
* [ ] Cost Optimization
* [ ] AWS Security

---

# 📊 AWS Interview Preparation

```text
20 Normal Questions
        +
30 Scenario-Based Questions
        =
50 AWS Interview Questions
```

