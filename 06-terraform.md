# Terraform Interview Questions & Answers

## 📌 Overview

This document contains **50 Terraform interview questions and answers** for DevOps engineers.

### Structure

* **20 Normal Interview Questions**
* **30 Scenario-Based Interview Questions**
* Beginner → Intermediate → Advanced
* Practical Terraform commands
* AWS infrastructure examples
* State management
* Modules
* Variables
* Remote backend
* Terraform troubleshooting
* CI/CD integration
* Production best practices

---

# 🟢 PART 1 — NORMAL INTERVIEW QUESTIONS

## Q1. What is Terraform?

**Answer:**

Terraform is an **Infrastructure as Code (IaC)** tool developed by HashiCorp.

It allows us to define infrastructure using configuration files and create resources automatically.

Terraform can manage:

* AWS
* Azure
* Google Cloud
* Kubernetes
* GitHub
* Databases
* Networking
* SaaS platforms

Example:

```hcl
resource "aws_instance" "web" {
  ami           = "ami-xxxxxxxx"
  instance_type = "t3.micro"
}
```

Terraform can create the EC2 instance from this configuration.

---

## Q2. What is Infrastructure as Code?

**Answer:**

Infrastructure as Code means managing infrastructure using configuration files instead of manually creating resources through a cloud console.

### Manual approach

```text
AWS Console
    ↓
Create VPC
    ↓
Create Subnet
    ↓
Create EC2
    ↓
Create Security Group
```

### IaC approach

```text
Terraform Code
      ↓
terraform plan
      ↓
terraform apply
      ↓
AWS Infrastructure
```

Benefits include:

* Automation
* Repeatability
* Version control
* Consistency
* Faster provisioning
* Easier disaster recovery

---

## Q3. What is a Terraform provider?

**Answer:**

A provider is a Terraform plugin that allows Terraform to communicate with an external platform or API.

Examples:

* AWS provider
* Azure provider
* Google provider
* Kubernetes provider
* GitHub provider

Example:

```hcl
provider "aws" {
  region = "ap-south-1"
}
```

Terraform then uses the AWS provider to communicate with AWS APIs.

---

## Q4. What is a Terraform resource?

**Answer:**

A resource represents an infrastructure object managed by Terraform.

Example:

```hcl
resource "aws_instance" "web" {
  ami           = "ami-xxxxxxxx"
  instance_type = "t3.micro"
}
```

Here:

```text
aws_instance = resource type
web          = resource name
```

---

## Q5. What is the Terraform workflow?

**Answer:**

The standard Terraform workflow is:

```text
Write Code
    ↓
terraform init
    ↓
terraform fmt
    ↓
terraform validate
    ↓
terraform plan
    ↓
terraform apply
    ↓
Infrastructure
```

When changes are needed:

```text
Modify Code
    ↓
terraform plan
    ↓
terraform apply
```

---

## Q6. What does `terraform init` do?

**Answer:**

`terraform init` initializes a Terraform working directory.

It can:

* Download providers
* Initialize modules
* Configure the backend
* Prepare the working directory

Run:

```bash
terraform init
```

After initialization, Terraform creates the `.terraform` directory and typically maintains dependency information through the lock file.

---

## Q7. What does `terraform plan` do?

**Answer:**

`terraform plan` previews the changes Terraform intends to make.

Example:

```bash
terraform plan
```

Terraform may show:

```text
+ create
~ update
- destroy
```

It is useful for reviewing changes before applying them.

---

## Q8. What does `terraform apply` do?

**Answer:**

`terraform apply` applies the Terraform configuration and creates or modifies infrastructure.

```bash
terraform apply
```

Terraform normally asks for confirmation.

You can also use a previously generated plan:

```bash
terraform plan -out=tfplan
terraform apply tfplan
```

This is useful in controlled CI/CD workflows.

---

## Q9. What is Terraform state?

**Answer:**

Terraform state is the record Terraform uses to map configuration resources to real infrastructure.

The default state file is:

```text
terraform.tfstate
```

It may contain:

* Resource IDs
* Attributes
* Dependencies
* Metadata

Example:

```text
Terraform Configuration
        |
        v
Terraform State
        |
        v
AWS Resources
```

Terraform uses state to determine what has changed.

---

## Q10. Why is Terraform state important?

**Answer:**

Terraform uses state to compare:

```text
Desired State
      vs
Current State
```

For example:

```text
Terraform Code:
EC2 = t3.micro

Actual Infrastructure:
EC2 = t3.small
```

Terraform can detect the difference and determine whether a change is required.

State is therefore critical to Terraform's operation.

---

## Q11. What is a Terraform backend?

**Answer:**

A backend determines where Terraform stores its state.

Example local backend:

```text
terraform.tfstate
```

A common AWS production setup uses an S3-based backend:

```hcl
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "production/terraform.tfstate"
    region = "ap-south-1"
  }
}
```

Remote state provides centralized state management and helps teams collaborate.

---

## Q12. What are Terraform variables?

**Answer:**

Variables allow us to make Terraform configurations reusable.

Example:

```hcl
variable "instance_type" {
  type    = string
  default = "t3.micro"
}
```

Use:

```hcl
resource "aws_instance" "web" {
  instance_type = var.instance_type
}
```

Provide a value:

```bash
terraform apply -var="instance_type=t3.small"
```

---

## Q13. What are Terraform outputs?

**Answer:**

Outputs display useful information after Terraform creates infrastructure.

Example:

```hcl
output "instance_ip" {
  value = aws_instance.web.public_ip
}
```

Run:

```bash
terraform output
```

Outputs are useful for:

* IP addresses
* DNS names
* Load balancer URLs
* Resource IDs

---

## Q14. What are Terraform modules?

**Answer:**

A module is a reusable collection of Terraform configurations.

Example:

```text
terraform-project/
├── main.tf
├── variables.tf
├── outputs.tf
└── modules/
    └── vpc/
        ├── main.tf
        ├── variables.tf
        └── outputs.tf
```

A module can be reused:

```hcl
module "vpc" {
  source = "./modules/vpc"

  vpc_cidr = "10.0.0.0/16"
}
```

Modules help improve:

* Reusability
* Maintainability
* Standardization

---

## Q15. What is the difference between `count` and `for_each`?

**Answer:**

Both are used to create multiple resources.

### count

Useful when resources are almost identical and indexed numerically.

```hcl
resource "aws_instance" "web" {
  count = 3

  ami           = "ami-xxxxxxxx"
  instance_type = "t3.micro"
}
```

Resources:

```text
aws_instance.web[0]
aws_instance.web[1]
aws_instance.web[2]
```

### for_each

Useful when resources are based on a map or set and need stable keys.

```hcl
resource "aws_instance" "web" {
  for_each = {
    app = "t3.micro"
    db  = "t3.small"
  }

  ami           = "ami-xxxxxxxx"
  instance_type = each.value
}
```

---

## Q16. What is the Terraform lock file?

**Answer:**

Terraform creates:

```text
.terraform.lock.hcl
```

The lock file records provider dependency selections and checksums.

It helps ensure consistent provider versions across environments.

It should normally be committed to version control.

---

## Q17. What is `terraform fmt`?

**Answer:**

`terraform fmt` formats Terraform configuration files.

Run:

```bash
terraform fmt
```

For a recursive formatting operation:

```bash
terraform fmt -recursive
```

This helps maintain consistent code formatting.

---

## Q18. What is `terraform validate`?

**Answer:**

`terraform validate` checks whether Terraform configuration is syntactically valid and internally consistent.

Run:

```bash
terraform validate
```

Typical workflow:

```bash
terraform fmt
terraform validate
terraform plan
```

`validate` does not replace `plan`; it checks configuration correctness rather than showing the complete infrastructure change plan.

---

## Q19. What is Terraform drift?

**Answer:**

Drift occurs when infrastructure changes outside Terraform and Terraform configuration/state no longer represents the actual desired setup.

Example:

```text
Terraform:
EC2 instance type = t3.micro

Someone manually changes AWS:
EC2 instance type = t3.small
```

Terraform may detect this difference during planning or refresh-related operations.

Best practice is to avoid manually changing Terraform-managed resources unless there is a controlled reason and the Terraform code is updated accordingly.

---

## Q20. What is the difference between Terraform and CloudFormation?

**Answer:**

| Terraform                      | CloudFormation               |
| ------------------------------ | ---------------------------- |
| Multi-cloud                    | AWS-focused                  |
| HCL                            | YAML/JSON                    |
| Uses providers                 | AWS-native service           |
| Large provider ecosystem       | Deep AWS integration         |
| Terraform state                | CloudFormation stack state   |
| Can manage many SaaS platforms | Primarily AWS infrastructure |

Terraform is often selected when organizations want a common IaC workflow across multiple platforms.

---

# 🔴 PART 2 — SCENARIO-BASED INTERVIEW QUESTIONS

## Q21. Terraform says "No configuration files". What would you check?

**Answer:**

First check the current directory:

```bash
pwd
ls -la
```

Terraform expects `.tf` files in the current working directory.

For example:

```text
project/
├── main.tf
├── variables.tf
└── outputs.tf
```

If the files exist elsewhere:

```bash
cd project
terraform init
```

Then run:

```bash
terraform plan
```

---

## Q22. `terraform init` fails. How would you troubleshoot it?

**Answer:**

First read the exact error.

Then check:

```bash
terraform version
```

Verify provider configuration:

```bash
cat main.tf
```

Check internet connectivity and provider registry access.

If appropriate, retry initialization:

```bash
terraform init
```

For backend or provider configuration changes:

```bash
terraform init -reconfigure
```

I would not delete `.terraform` or the state file blindly. First determine whether the problem is related to the provider, backend, credentials, or network.

---

## Q23. Terraform cannot authenticate to AWS. What would you check?

**Answer:**

I would verify AWS credentials and identity.

For AWS CLI:

```bash
aws sts get-caller-identity
```

Then check:

```bash
aws configure
```

If using an IAM role, verify the role is attached and has the required permissions.

I would avoid hard-coding AWS access keys inside Terraform files.

Preferred authentication methods include:

* IAM roles
* Environment variables
* AWS CLI credential chain
* CI/CD credentials
* Workload identity mechanisms where supported

---

## Q24. Terraform plan wants to destroy an important production resource. What would you do?

**Answer:**

I would **not immediately run `terraform apply`**.

First investigate:

```bash
terraform plan
```

I would check:

* What configuration changed?
* Was the resource renamed?
* Was a module changed?
* Was state lost?
* Was the resource modified manually?
* Is the resource address different?

If a resource was renamed in configuration, `terraform state mv` or a Terraform `moved` block may be appropriate depending on the situation.

For critical resources, I would review the plan with another engineer before applying it.

---

## Q25. Someone manually changed an AWS resource managed by Terraform. What happens?

**Answer:**

This creates infrastructure drift.

Example:

```text
Terraform:
instance_type = t3.micro

AWS:
instance_type = t3.small
```

Run:

```bash
terraform plan
```

Terraform can identify differences between the configuration/state and actual infrastructure.

I would decide whether:

1. The manual change should be reverted by Terraform, or
2. The Terraform configuration should be updated to intentionally preserve the new setting.

The goal is to return ownership to Infrastructure as Code.

---

## Q26. Terraform state file is accidentally deleted. What would you do?

**Answer:**

This is a serious situation.

If using a remote backend with versioning or state recovery, restore the appropriate state version.

I would:

1. Stop Terraform changes.
2. Identify the correct state backup/version.
3. Restore the state.
4. Run `terraform plan`.
5. Carefully review the resulting plan.

If the state cannot be recovered, I would avoid blindly running `terraform apply` because Terraform may attempt to recreate resources it no longer knows about.

Existing resources may need to be imported carefully.

---

## Q27. Two engineers run `terraform apply` at the same time. What problem can occur?

**Answer:**

Without proper state locking, concurrent Terraform operations can corrupt or conflict with state.

For team environments, I would use a remote backend with state locking/concurrency control appropriate to the backend and Terraform setup.

A common AWS architecture is:

```text
Developer 1 ─┐
             |
Developer 2 ─┼──> Remote Terraform State
             |
CI/CD ───────┘
```

Only one state-changing operation should proceed at a time for the same state.

---

## Q28. Terraform says the state is locked. What would you do?

**Answer:**

First determine whether another Terraform operation is actually running.

I would not immediately force-unlock.

Check:

* CI/CD jobs
* Other engineers
* Terraform processes
* Backend lock information

If the lock is definitely stale and no operation is running, then `terraform force-unlock` may be used carefully.

Example:

```bash
terraform force-unlock <LOCK_ID>
```

This should be treated as a recovery operation, not a normal workflow.

---

## Q29. Terraform shows "Unsupported argument" when using a module. What does it mean?

**Answer:**

Usually the module does not define the variable being passed.

For example:

```hcl
module "vpc" {
  source = "./modules/vpc"

  vpc_name = "production"
}
```

But the module may not contain:

```hcl
variable "vpc_name" {
}
```

I would inspect:

```text
modules/vpc/variables.tf
```

and make sure the variable name matches exactly.

Then run:

```bash
terraform validate
```

---

## Q30. A Terraform module works in one project but fails in another. What would you check?

**Answer:**

I would compare:

* Module version
* Provider version
* Terraform version
* Module variables
* Module outputs
* Provider configuration
* Backend
* Environment-specific values

Run:

```bash
terraform version
terraform providers
```

I would also inspect the module's input variables and outputs.

Pinning compatible Terraform/provider/module versions helps avoid unexpected behavior.

---

## Q31. Terraform is trying to recreate an EC2 instance after a small configuration change. Why?

**Answer:**

Some resource attributes cannot be changed in place.

Terraform may show:

```text
-/+ replace
```

instead of:

```text
~ update
```

I would inspect the plan carefully.

Example:

```bash
terraform plan
```

Look for:

```text
forces replacement
```

Before applying in production, I would confirm that replacement is actually intended and understand the availability impact.

---

## Q32. Terraform resource creation takes a very long time. How would you troubleshoot it?

**Answer:**

I would identify which resource is taking time.

Run:

```bash
terraform apply
```

and observe the resource operation.

I would check:

* AWS service status
* Dependency creation
* Networking
* IAM permissions
* Security groups
* Subnet configuration
* Resource-specific provisioning time

For EKS, for example, cluster creation can take significantly longer than a simple EC2 instance because multiple AWS components are involved.

I would avoid interrupting an operation without understanding what Terraform is currently doing.

---

## Q33. Terraform creates an EC2 instance, but the web server is not working. What do you check?

**Answer:**

I would separate the Terraform problem from the application problem.

First:

```bash
terraform output
```

Then check AWS:

* EC2 instance state
* Security Group
* Public/private subnet
* Route table
* Internet Gateway
* Network ACL
* User data
* Web server status

On the instance:

```bash
systemctl status nginx
```

Test:

```bash
curl localhost
```

If Nginx works locally but not externally, I would investigate networking/security rather than Terraform syntax.

---

## Q34. Terraform creates an EC2 instance, but user data did not work. What would you do?

**Answer:**

First verify that user data was actually passed to the instance.

On Ubuntu, inspect cloud-init logs:

```bash
sudo cloud-init status
```

and:

```bash
sudo cat /var/log/cloud-init-output.log
```

I would check:

* User data syntax
* Package installation commands
* Internet connectivity
* Operating system
* Required permissions
* Script execution errors

I would also make sure the script is compatible with the AMI being used.

---

## Q35. Terraform wants to recreate resources every time you run `terraform plan`. What could be wrong?

**Answer:**

This can happen when configuration values continuously differ from what the provider reports.

Possible causes:

* Incorrect configuration
* Provider behavior
* Computed attributes
* Changing external values
* Unstable expressions
* Resource defaults
* Incorrect use of timestamps/random values

I would inspect:

```bash
terraform plan
```

and identify exactly which attributes are changing.

I would not simply add `ignore_changes` without understanding the reason.

---

## Q36. How would you manage different environments such as dev, staging, and production?

**Answer:**

There are several approaches.

One common structure is:

```text
terraform/
├── modules/
│   ├── vpc/
│   ├── ec2/
│   └── alb/
│
└── environments/
    ├── dev/
    ├── staging/
    └── production/
```

Each environment can use reusable modules with different variables.

Example:

```text
dev:
  instance_count = 1

production:
  instance_count = 3
```

For production, I would also use separate state and appropriate access controls.

---

## Q37. Your Terraform state contains database passwords. Is that safe?

**Answer:**

Terraform state can contain sensitive values even if they are marked sensitive in configuration.

Marking a variable as sensitive primarily affects CLI display; it does not automatically remove the value from state.

Therefore I would:

* Secure the backend
* Restrict state access
* Enable encryption
* Enable versioning/recovery where appropriate
* Avoid unnecessary secrets in Terraform
* Use dedicated secret-management systems when appropriate

For AWS, services such as Secrets Manager can be used for application secrets.

---

## Q38. How would you protect Terraform state in AWS?

**Answer:**

I would use a secured remote backend.

For example, an S3-based backend can provide centralized state storage.

I would apply:

* Encryption
* Restricted IAM permissions
* Versioning
* Controlled access
* Separate state per environment
* Auditing

Example:

```hcl
terraform {
  backend "s3" {
    bucket = "company-terraform-state"
    key    = "production/network/terraform.tfstate"
    region = "ap-south-1"
  }
}
```

State access should be treated as highly sensitive because state can contain infrastructure details and potentially sensitive values.

---

## Q39. Your Terraform code has hundreds of lines in one `main.tf`. How would you improve it?

**Answer:**

I would organize the project into logical files and reusable modules.

Example:

```text
terraform/
├── main.tf
├── variables.tf
├── outputs.tf
├── providers.tf
├── versions.tf
├── locals.tf
└── modules/
    ├── vpc/
    ├── ec2/
    ├── alb/
    └── autoscaling/
```

This improves:

* Maintainability
* Reusability
* Code review
* Troubleshooting

I would avoid creating modules simply for the sake of having many modules; modules should represent meaningful reusable infrastructure components.

---

## Q40. Terraform needs to create 10 EC2 instances with different names. Which feature would you use?

**Answer:**

I would use `for_each`.

Example:

```hcl
variable "servers" {
  default = {
    web1 = "t3.micro"
    web2 = "t3.micro"
    app1 = "t3.small"
  }
}

resource "aws_instance" "server" {
  for_each = var.servers

  ami           = "ami-xxxxxxxx"
  instance_type = each.value

  tags = {
    Name = each.key
  }
}
```

This provides stable resource keys such as:

```text
aws_instance.server["web1"]
aws_instance.server["web2"]
aws_instance.server["app1"]
```

---

## Q41. You need to create resources in multiple AWS regions. How can Terraform do this?

**Answer:**

Terraform can use multiple provider configurations with aliases.

Example:

```hcl
provider "aws" {
  region = "ap-south-1"
}

provider "aws" {
  alias  = "mumbai"
  region = "ap-south-1"
}

provider "aws" {
  alias  = "us_east"
  region = "us-east-1"
}
```

A resource can use a specific provider:

```hcl
resource "aws_instance" "us_server" {
  provider = aws.us_east

  ami           = "ami-xxxxxxxx"
  instance_type = "t3.micro"
}
```

The AMI must be valid in the selected region.

---

## Q42. You need to import an existing AWS resource into Terraform. How would you do it?

**Answer:**

First define the resource in Terraform configuration.

For example:

```hcl
resource "aws_instance" "existing" {
  # configuration
}
```

Then import the existing resource:

```bash
terraform import aws_instance.existing i-0123456789abcdef0
```

After import:

```bash
terraform plan
```

I would update the Terraform configuration until the plan accurately represents the imported infrastructure.

Importing is not a replacement for writing the desired configuration.

---

## Q43. Someone renamed a Terraform resource in the configuration. Terraform wants to destroy and recreate it. How can you prevent unnecessary recreation?

**Answer:**

If the real infrastructure resource is the same but its Terraform address changed, I would use a Terraform `moved` block when appropriate.

Example:

```hcl
moved {
  from = aws_instance.web
  to   = aws_instance.application
}
```

This tells Terraform that the resource address changed without requiring the underlying infrastructure to be recreated.

For older workflows, `terraform state mv` can also be used carefully.

---

## Q44. How would you use Terraform in a Jenkins CI/CD pipeline?

**Answer:**

A typical pipeline is:

```text
GitHub
   |
   v
Jenkins
   |
   +--> terraform fmt
   |
   +--> terraform validate
   |
   +--> terraform plan
   |
   v
Approval
   |
   v
terraform apply
   |
   v
AWS Infrastructure
```

Example commands:

```bash
terraform init
terraform fmt -check
terraform validate
terraform plan -out=tfplan
terraform apply tfplan
```

For production, I would separate plan and apply stages and require appropriate approval before infrastructure changes are applied.

---

## Q45. Terraform plan succeeds, but Terraform apply fails. What would you investigate?

**Answer:**

`terraform plan` is not a guarantee that the eventual API operation will succeed.

I would inspect the apply error and check:

* IAM permissions
* AWS quotas
* Resource limits
* Availability Zone capacity
* Dependency issues
* Invalid runtime configuration
* Provider/API errors

Then:

```bash
terraform plan
```

again after correcting the issue.

I would make sure the state accurately reflects what was actually created before retrying.

---

## Q46. Terraform apply partially succeeds and then fails. What happens to the created resources?

**Answer:**

Terraform does not automatically roll back all successfully created resources like a database transaction.

Some resources may already exist.

Terraform updates state as resources are successfully managed, so after fixing the error I would inspect:

```bash
terraform plan
```

The plan shows what Terraform believes remains to be done.

I would never assume that a failed `apply` means nothing was created.

---

## Q47. Your Terraform code works locally but fails in Jenkins. What would you check?

**Answer:**

I would compare the environments.

Check:

```bash
terraform version
```

Then verify:

* AWS credentials
* IAM permissions
* Terraform version
* Provider versions
* Environment variables
* Backend configuration
* Working directory
* Terraform variables
* Jenkins agent network access

A common issue is that the developer's machine has AWS credentials configured while the Jenkins agent does not.

I would use Jenkins credentials or an appropriate IAM role rather than copying personal AWS credentials into Jenkins.

---

## Q48. Your Terraform module output is not available in the root module. How do you troubleshoot it?

**Answer:**

The child module must define an output.

Inside the module:

```hcl
output "vpc_id" {
  value = aws_vpc.main.id
}
```

Then the root module can reference:

```hcl
module.vpc.vpc_id
```

If Terraform reports an unknown output, I would check:

* Output name
* Module source
* Module path
* Module initialization

Then run:

```bash
terraform init
terraform validate
```

---

## Q49. You are asked to design a production-ready Terraform project for AWS. What would your architecture look like?

**Answer:**

I would organize the project around reusable modules and separate environments.

```text
Terraform Repository
│
├── modules/
│   ├── vpc/
│   ├── security-group/
│   ├── alb/
│   ├── ec2/
│   ├── autoscaling/
│   └── rds/
│
└── environments/
    ├── dev/
    │   ├── main.tf
    │   ├── variables.tf
    │   └── outputs.tf
    │
    ├── staging/
    │
    └── production/
```

Production infrastructure could be:

```text
                    AWS
                     |
                    VPC
                     |
        +------------+------------+
        |                         |
 Public Subnets             Private Subnets
        |                         |
        v                         v
 Load Balancer             Application Nodes
                                  |
                                  v
                                RDS
```

Terraform state would be stored remotely with appropriate security and concurrency controls.

CI/CD:

```text
GitHub
   |
   v
Jenkins
   |
   +--> Format
   |
   +--> Validate
   |
   +--> Security Scan
   |
   +--> Plan
   |
   v
Manual Approval
   |
   v
Apply
```

---

## Q50. Explain your complete production Terraform workflow in an interview.

**Answer:**

I would explain it like this:

> "I use Terraform as Infrastructure as Code to provision and manage AWS resources. I keep the Terraform code in Git and organize reusable infrastructure into modules. Each environment has separate configuration and state. The Terraform state is stored in a secured remote backend with appropriate access controls and concurrency protection."

Then:

```text
Developer
    |
    v
GitHub
    |
    v
Jenkins
    |
    +----> terraform fmt
    |
    +----> terraform validate
    |
    +----> Security Scan
    |
    +----> terraform plan
    |
    v
Code Review / Approval
    |
    v
terraform apply
    |
    v
AWS Infrastructure
```

For production I would also use:

* Remote state
* State protection
* Version-controlled code
* Reusable modules
* Environment separation
* IAM least privilege
* Provider/version constraints
* Automated validation
* Security scanning
* Plan review
* Approval before apply
* Monitoring
* Disaster recovery procedures

The most important principle is:

> **Terraform code should be the source of truth for infrastructure.**

---

# 🛠️ Terraform Troubleshooting Cheat Sheet

## Check Terraform version

```bash
terraform version
```

## Initialize Terraform

```bash
terraform init
```

## Reconfigure backend

```bash
terraform init -reconfigure
```

## Format code

```bash
terraform fmt
```

## Format recursively

```bash
terraform fmt -recursive
```

## Validate configuration

```bash
terraform validate
```

## Create execution plan

```bash
terraform plan
```

## Save plan

```bash
terraform plan -out=tfplan
```

## Apply saved plan

```bash
terraform apply tfplan
```

## Apply configuration

```bash
terraform apply
```

## Destroy infrastructure

```bash
terraform destroy
```

⚠️ Always review the destroy plan carefully, especially in production.

## Show state

```bash
terraform show
```

## List resources in state

```bash
terraform state list
```

## Show a specific resource

```bash
terraform state show <resource>
```

## Terraform outputs

```bash
terraform output
```

## Show providers

```bash
terraform providers
```

## Import existing resource

```bash
terraform import <resource-address> <resource-id>
```

## Refresh/check infrastructure during planning

```bash
terraform plan
```

## Create a workspace

```bash
terraform workspace new dev
```

## List workspaces

```bash
terraform workspace list
```

## Select workspace

```bash
terraform workspace select dev
```

---

# 🧠 Terraform Troubleshooting Framework

When an interviewer gives you a Terraform production problem, follow this process:

```text
1. Understand the error
        ↓
2. Check Terraform version
        ↓
3. Check configuration
        ↓
4. Run terraform fmt
        ↓
5. Run terraform validate
        ↓
6. Run terraform plan
        ↓
7. Check provider
        ↓
8. Check credentials
        ↓
9. Check state
        ↓
10. Check backend
        ↓
11. Check cloud resource/API
        ↓
12. Identify root cause
        ↓
13. Fix the configuration
        ↓
14. Run plan again
        ↓
15. Apply after review
        ↓
16. Verify infrastructure
```

### Most important commands

```bash
terraform version
terraform init
terraform validate
terraform fmt
terraform plan
terraform show
terraform state list
terraform state show <resource>
terraform providers
terraform output
```

---

# 🔐 Terraform Security Best Practices

## 1. Never hard-code AWS credentials

Bad:

```hcl
provider "aws" {
  access_key = "AKIA..."
  secret_key = "..."
}
```

Use IAM roles or a secure credential mechanism instead.

---

## 2. Protect Terraform state

State may contain sensitive information.

Use:

* Remote backend
* Encryption
* IAM permissions
* Versioning/recovery
* Access auditing

---

## 3. Use least privilege

Terraform CI/CD should only have the AWS permissions required for its job.

---

## 4. Review plans

Always review:

```bash
terraform plan
```

before applying production changes.

---

## 5. Version providers

Use appropriate version constraints so provider upgrades are controlled.

---

## 6. Scan Terraform code

Use IaC security scanners in CI/CD to identify:

* Public S3 buckets
* Open security groups
* Weak IAM policies
* Unencrypted resources
* Other configuration risks

---

# 🎯 Terraform Interview Topics Checklist

Before attending a DevOps interview, make sure you understand:

* [ ] Infrastructure as Code
* [ ] Terraform architecture
* [ ] Terraform workflow
* [ ] Providers
* [ ] Resources
* [ ] Variables
* [ ] Outputs
* [ ] Locals
* [ ] Data sources
* [ ] Terraform state
* [ ] Remote state
* [ ] Backend
* [ ] State locking/concurrency
* [ ] Terraform modules
* [ ] Module inputs
* [ ] Module outputs
* [ ] `count`
* [ ] `for_each`
* [ ] Conditional expressions
* [ ] Functions
* [ ] Dynamic blocks
* [ ] Terraform lifecycle
* [ ] `depends_on`
* [ ] Resource dependencies
* [ ] `terraform init`
* [ ] `terraform plan`
* [ ] `terraform apply`
* [ ] `terraform destroy`
* [ ] `terraform fmt`
* [ ] `terraform validate`
* [ ] Terraform import
* [ ] Terraform state commands
* [ ] Terraform drift
* [ ] Terraform workspaces
* [ ] Provider versioning
* [ ] `.terraform.lock.hcl`
* [ ] AWS authentication
* [ ] Terraform security
* [ ] Terraform with AWS
* [ ] Terraform with EKS
* [ ] Terraform with Jenkins
* [ ] Terraform CI/CD
* [ ] Production infrastructure design
* [ ] Troubleshooting

---

# ⭐ Interview Tip

For Terraform scenario-based questions, don't answer only with a command.

Use this approach:

```text
Understand
    ↓
Inspect
    ↓
Identify Root Cause
    ↓
Check State
    ↓
Fix Code / Infrastructure
    ↓
terraform plan
    ↓
Review
    ↓
terraform apply
    ↓
Verify
    ↓
Prevent
```

### Example

**Interviewer:** "Terraform wants to destroy your production EC2 instance. What will you do?"

**Strong answer:**

> "I would not immediately run `terraform apply`. First I would inspect the plan to understand why Terraform wants to replace the instance. I would check whether the resource was renamed, whether the configuration changed an attribute that forces replacement, whether there is state drift, or whether the resource address changed. After identifying the root cause, I would correct the configuration or state mapping if appropriate, run `terraform plan` again, review the new plan, and only then apply the change after confirming there is no unintended production impact."

This demonstrates **Terraform knowledge, state awareness, production safety, and troubleshooting ability** rather than simply memorizing commands.
