---

# 1. What is Terraform?

Terraform is an IaC (**Infrastructure as Code**) tool by [HashiCorp](https://www.hashicorp.com/?utm_source=chatgpt.com) used to **provision and manage infrastructure declaratively**.

Examples:

* Create Amazon Web Services EC2
* Create Microsoft Azure VMs
* Create Google Cloud resources
* Kubernetes clusters

**Benefits**

* version control
* reusable
* automated
* consistent infra

---

# 2. Imperative vs Declarative

### Imperative:

"Do this, then this"

Example:

```bash
aws ec2 run-instances ...
```

### Declarative:

"Desired state"

```hcl
resource "aws_instance" "web" {
  ami = "ami-123"
}
```

Terraform ensures desired state.

---

# 3. Terraform lifecycle (very important)

```text
Write code → init → plan → apply → destroy
```

### terraform init

Downloads providers/plugins.

Example:

```bash
terraform init
```

---

### terraform plan

Shows execution plan.

```bash
terraform plan
```

Output:

```text
+ create
~ update
- destroy
```

---

### terraform apply

Executes changes.

```bash
terraform apply
```

---

### terraform destroy

Deletes resources.

```bash
terraform destroy
```

---

# 4. Provider

Provider = plugin to talk to cloud.

Examples:

* Amazon Web Services
* Microsoft Azure
* Google Cloud
* Kubernetes

Example:

```hcl
provider "aws" {
  region = "ap-south-1"
}
```

Interview:
**Q:** What happens during init?
**A:** Downloads provider binaries and initializes backend.

---

# 5. Resource vs Data Source (important)

## Resource

Creates/manages infra.

```hcl
resource "aws_s3_bucket" "demo" {}
```

Creates bucket.

---

## Data Source

Reads existing infra.

```hcl
data "aws_vpc" "default" {
  default = true
}
```

Reads existing VPC.

Interview answer:
Resource = create/manage
Data source = fetch/read

---

# 6. Variables

Used for dynamic values.

```hcl
variable "region" {
  default = "ap-south-1"
}
```

Use:

```hcl
region = var.region
```

Pass:

```bash
terraform apply -var="region=us-east-1"
```

---

# 7. Outputs

Expose values.

```hcl
output "ip" {
 value = aws_instance.web.public_ip
}
```

Useful in CI/CD.

---

# 8. State file (most asked)

File:

```text
terraform.tfstate
```

Stores:

* current infra state
* resource IDs
* metadata

Why needed?
Terraform compares:

```text
Desired code vs Actual state
```

Interview:
Without state → Terraform doesn't know what exists.

---

# 9. Should state be in Git?

NO.

Why?
Contains:

* secrets
* resource IDs
* drift risks
* conflicts

Use remote backend.

---

# 10. Remote backend

Example: Amazon S3 + Amazon DynamoDB

```hcl
terraform {
 backend "s3" {
   bucket = "tf-state"
   key    = "prod.tfstate"
   region = "ap-south-1"
   dynamodb_table = "tf-lock"
 }
}
```

Benefits:

* shared
* versioning
* locking

---

# 11. State locking (very important)

Problem:
Two engineers run apply simultaneously.

Issue:
State corruption.

Solution:
DynamoDB lock.

Interview:
How avoid concurrent updates?
Answer:
Use remote backend + locking.

---

# 12. Terraform refresh / drift

Drift = manual change outside Terraform.

Example:
Delete EC2 manually.

Check:

```bash
terraform plan
```

Detects drift.

---

# 13. terraform import

Existing infra → Terraform

Example:

```bash
terraform import aws_instance.web i-12345
```

Real-time:
Legacy infra onboarding.

---

# 14. taint / replace

Force recreation.

Old:

```bash
terraform taint aws_instance.web
```

New:

```bash
terraform apply -replace=aws_instance.web
```

---

# 15. count vs for_each

## count

Numeric.

```hcl
count = 3
```

Creates 3 instances.

---

## for_each

Map/list based.

```hcl
for_each = toset(["dev","qa"])
```

Better when names matter.

Interview:
Why prefer for_each?
Stable identity.

---

# 16. Modules (must know)

Reusable code.

Example:

```text
modules/
   ec2/
   vpc/
```

Call:

```hcl
module "vpc" {
 source = "./modules/vpc"
}
```

Benefits:

* reuse
* standardization

---

# 17. Workspaces

Used for environments.

```bash
terraform workspace new dev
terraform workspace new prod
```

Separate states.

Good for:
dev/qa/prod

---

# 18. Depends_on

Explicit dependency.

```hcl
depends_on = [aws_vpc.main]
```

Use when implicit dependency fails.

---

# 19. Null resource

Runs scripts.

```hcl
resource "null_resource" "test" {
 provisioner "local-exec" {
   command = "echo hello"
 }
}
```

Use sparingly.

---

# 20. Provisioners (not preferred)

Types:

* local-exec
* remote-exec

Used post-create.

Avoid when possible.

Prefer:
[cloud-init/user-data docs](https://cloudinit.readthedocs.io/?utm_source=chatgpt.com)

---

# 21. Backend vs Provider

Interview trap.

Backend:
Where state stored.

Provider:
Who creates infra.

Example:
S3 backend + AWS provider.

---

# 22. Sensitive variables

```hcl
variable "password" {
 sensitive = true
}
```

Hide output.

Use:
[HashiCorp Vault](https://www.vaultproject.io/?utm_source=chatgpt.com) or secrets manager.

---

# 23. Terraform fmt

Format code.

```bash
terraform fmt
```

Always run.

---

# 24. Validate

Syntax check.

```bash
terraform validate
```

---

# 25. Graph

Dependency graph.

```bash
terraform graph
```

Debug dependencies.

---

# Real-time Scenario Questions

---

## Scenario 1:

State file deleted. What do you do?

Answer:

* recover from S3 versioning
* restore backup
* if not available → import resources

---

## Scenario 2:

Someone changed infra manually.

Answer:
Drift.
Run:

```bash
terraform plan
```

---

## Scenario 3:

Apply failed midway.

Answer:
State partially updated.
Check:

```bash
terraform state list
terraform plan
```

Then reapply.

---

## Scenario 4:

Need same infra in dev/prod.

Answer:
Use:

* modules
* workspaces
* separate tfvars

---

## Scenario 5:

Secrets in code.

Wrong:

```hcl
password="abc123"
```

Right:

* env vars
* Vault
* secrets manager

---

# Common interview traps

### Q: Does Terraform create resources sequentially?

No.
Parallel by default.

---

### Q: Can Terraform rollback?

No native rollback.

Use:
Git revert + apply.

---

### Q: Can one resource have multiple providers?

Yes using aliases.

Example:

```hcl
provider "aws" {
 alias="west"
}
```

---

### Q: What is OpenTofu?

[OpenTofu](https://opentofu.org/?utm_source=chatgpt.com) is open-source fork of Terraform after HashiCorp license change.

---

# Hands-on must practice

Build:

1. VPC
2. subnet
3. EC2
4. SG
5. S3
6. remote backend
7. module
8. workspace

Commands:

```bash
terraform init
terraform fmt
terraform validate
terraform plan
terraform apply
terraform state list
terraform output
terraform destroy
```

---

# Interview one-liners

**What is Terraform?**
Declarative IaC tool.

**State file?**
Tracks infra.

**Module?**
Reusable code block.

**Backend?**
Stores state.

**Provider?**
Cloud plugin.

**count vs for_each?**
Number vs key-based.

**Import?**
Bring existing infra.

**Drift?**
Manual infra change.

**Locking?**
Avoid concurrent changes.

---


* [Terraform Cloud](https://app.terraform.io/?utm_source=chatgpt.com)
* policy as code ([Sentinel](https://developer.hashicorp.com/sentinel?utm_source=chatgpt.com))
* CI/CD with [GitHub Actions](https://github.com/features/actions?utm_source=chatgpt.com) / [Jenkins](https://www.jenkins.io/?utm_source=chatgpt.com)
* multi-account AWS
* reusable module design
* zero-downtime infra changes





# 1. `for_each` vs `count` (Terraform this is usually `for_each` vs `count`)

This is a very common interview question.

---

## `count`

Used when creating **multiple identical resources**.

Example:

```hcl id="4zy66j"
resource "aws_instance" "web" {
  count = 3
  ami   = "ami-123"
}
```

Creates:

```text id="hl0v13"
web[0]
web[1]
web[2]
```

---

### Problem with `count`

If one item removed from middle:

```hcl id="jlwm0r"
count = 2
```

Terraform may recreate resources because indexes shift.

Bad for production.

---

## `for_each`

Better when resources are unique.

Example:

```hcl id="40hqbh"
resource "aws_instance" "web" {
  for_each = {
    dev  = "t2.micro"
    prod = "t3.large"
  }

  instance_type = each.value
}
```

Creates:

```text id="m9r6p8"
web["dev"]
web["prod"]
```

Stable.

---

## Interview answer

“I use `count` for identical resources and `for_each` for named/unique resources because it avoids index-shift issues.”

Strong answer.

---

# 2. What are Terraform modules?

A module is reusable Terraform code.

Like a function in programming.

Example:

```text id="2pt91t"
modules/
  vpc/
  ec2/
  rds/
```

Call:

```hcl id="j3cnvh"
module "vpc" {
  source = "./modules/vpc"
  cidr   = "10.0.0.0/16"
}
```

---

## Why use modules?

Benefits:

* reusable
* standardization
* less duplication
* easier maintenance

---

## Real-world

“In my org, we had standard modules for:

* VPC
* EKS
* RDS
* IAM

Teams reused same modules.”

Very strong answer.

---

# 3. Purpose of statefile

File:

```text id="m11a3q"
terraform.tfstate
```

Stores:

* resource IDs
* metadata
* current infra mapping

Without it:
Terraform doesn’t know what exists.

Example:

```text id="v8o4lg"
EC2 id = i-123456
```

Stored in state.

---

## Why important?

Terraform compares:

```text id="1b2tsn"
Desired state (code)
vs
Current state (tfstate)
```

Then generates plan.

---

# 4. Should statefile be stored in Git?

**No.**

Why:

* contains secrets
* frequent changes
* locking issues
* corruption risk

Bad:

```text id="ej6z3h"
terraform.tfstate
```

Should be in:

```text id="vjwn5r"
.gitignore
```

---

## Add:

```gitignore id="5q8w8q"
*.tfstate
*.tfstate.backup
```

---

# 5. Terraform statefile management

Best practice:

Use remote backend.

Example with Amazon S3:

```hcl id="jlwmjb"
terraform {
 backend "s3" {
   bucket = "tf-state-prod"
   key    = "network/vpc.tfstate"
   region = "us-east-1"
   dynamodb_table = "tf-lock"
 }
}
```

Benefits:

* central
* versioned
* team access
* locking

---

# 6. Concurrent state updates issue

Problem:
Two engineers run:

```bash id="7lvr4q"
terraform apply
```

Same time.

Risk:
state corruption.

---

## Fix

Use locking.

AWS:
Amazon DynamoDB lock table.

Example:

```text id="o6x4sl"
LockID
```

Then second user gets:

```text id="m86if9"
Error acquiring state lock
```

Good protection.

---

# 7. Where to store statefile without cloud?

Options:

### 1. local shared storage

Example:

```text id="01x35u"
/nfs/terraform/state
```

Not ideal.

---

### 2. GitLab backend

[GitLab Terraform State](https://docs.gitlab.com/ee/user/infrastructure/iac/terraform_state.html?utm_source=chatgpt.com)

Good option.

---

### 3. HashiCorp Consul

```hcl id="t56f0c"
backend "consul" {}
```

Used on-prem.

---

### 4. HashiCorp Vault (not common for state)

---

## Interview answer

“In on-prem, I’ve used NFS and Consul; preferred is Consul because of locking.”

---

# 8. Terraform Enterprise vs Community

---

## Community (OSS)

Free:

```bash id="ozxljl"
terraform init
terraform apply
```

Need to manage:

* state
* security
* pipelines

yourself.

---

## Terraform Enterprise

Adds:

* UI
* RBAC
* policy as code
* remote runs
* private module registry
* team governance

Used in enterprises.

---

## Interview answer

“We used OSS for small teams; Enterprise for governance-heavy environments.”

---

# 9. What is OpenTofu?

OpenTofu is fork of Terraform.

Why?
After HashiCorp license change.

Commands same:

```bash id="84mbs3"
tofu init
tofu plan
tofu apply
```

Benefits:

* open source
* community governed

Good to mention.

---

# 10. Resource vs Data Source

Very common.

---

## Resource

Creates/manages infrastructure.

Example:

```hcl id="g4vf9r"
resource "aws_s3_bucket" "logs" {
  bucket = "my-logs"
}
```

Creates bucket.

---

## Data Source

Reads existing resource.

Example:

```hcl id="b9sqpk"
data "aws_vpc" "default" {
  default = true
}
```

Reads existing VPC.

Does not create.

---

## Real-world

“I often use data sources to fetch existing VPC, subnets, AMIs, and resource blocks to create new infra.”

Strong answer.

---

# Bonus interview question: what if state lost?

Recover:

```bash id="ukn31k"
terraform import
```

Example:

```bash id="j41qdb"
terraform import aws_instance.web i-12345
```

Rebuilds state.

Excellent answer.

---

# Final interview tip

Always say:

**“In production…”**
Example:

> “In production, we store Terraform state in versioned S3 with DynamoDB locking to prevent concurrent corruption.”



HANDS_ON

Perfect — this is **core Terraform interview territory**

***

## 🔹 1. `for_each` vs `for`

### ✅ Concept

*   `for_each` → **create multiple resources**
*   `for` → **iterate and transform values**

***

### ✅ Hands‑On Example

#### `for_each` (Resource Creation)

```hcl
variable "users" {
  type = set(string)
  default = ["dev", "qa", "prod"]
}

resource "aws_iam_user" "users" {
  for_each = var.users
  name     = each.value
}
```

✅ Creates **3 IAM users**

***

#### `for` (Data Transformation)

```hcl
output "upper_users" {
  value = [for u in var.users : upper(u)]
}
```

✅ Transforms data, does NOT create resources

***

### ✅ Real Production Insight

> I use `for_each` when I want Terraform to **track lifecycle per resource**, because it avoids index shifting problems that `count` causes.

***

### ✅ Interview Line

> `for_each` is for resource creation, `for` is for data manipulation.

***

## 🔹 2. What Are Terraform Modules?

### ✅ Concept

Modules are **reusable Terraform components**.

***

### ✅ Hands‑On Structure

    modules/
      vpc/
        main.tf
        variables.tf
        outputs.tf

***

### ✅ Example Module Usage

```hcl
module "vpc" {
  source = "./modules/vpc"
  cidr   = "10.0.0.0/16"
}
```

***

### ✅ Why I Use Modules (Experience)

*   Reusability
*   Standardization
*   Easier reviews
*   Reduced mistakes

***

### ✅ Interview Line

> Modules help us implement DRY principles and enforce infrastructure standards across environments.

***

## 🔹 3. Purpose of Terraform Statefile

### ✅ What Statefile Does

*   Maps **real infrastructure ↔ Terraform config**
*   Tracks:
    *   Resource IDs
    *   Dependencies
    *   Drift

***

### ✅ Example

```bash
terraform state list
terraform state show aws_instance.web
```

***

### ✅ Production Insight

> Without statefile, Terraform would not know whether to create, update, or destroy resources.

***

## 🔹 4. Should Statefile Be Stored in Git?

### ❌ NO — NEVER

### ✅ Why?

*   Contains secrets
*   Causes merge conflicts
*   No locking
*   High risk

***

### ✅ Interview Line

> Statefiles should never be stored in Git due to security and concurrency risks.

***

## 🔹 5. Terraform Statefile Management

### ✅ Best Practice (Hands‑On)

Use **remote backend with locking**

```hcl
terraform {
  backend "s3" {
    bucket         = "tf-state-prod"
    key            = "vpc/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "terraform-locks"
  }
}
```

***

### ✅ Why?

*   Central state
*   Locking
*   Team collaboration
*   Backup enabled

***

## 🔹 6. Concurrent State Updates Issue

### ✅ What Happens?

Two engineers run `terraform apply` simultaneously → **state corruption risk**

***

### ✅ How Terraform Prevents It

*   State locking (DynamoDB / Terraform Cloud)
*   Second run fails with lock error

***

### ✅ Interview Line

> Terraform uses state locking to prevent concurrent updates and race conditions.

***

## 🔹 7. Where to Store Statefile Without Cloud?

### ✅ Options

*   Terraform Cloud (Free tier)
*   Self‑hosted backend:
    *   Consul
    *   NFS (not recommended)
*   Encrypted object storage

***

### ✅ Real Answer

> If cloud is not allowed, I prefer Terraform Cloud because it provides locking, encryption, and versioning out of the box.

***

## 🔹 8. Terraform Enterprise vs Community

| Feature        | Community | Enterprise |
| -------------- | --------- | ---------- |
| State locking  | ✅         | ✅          |
| RBAC           | ❌         | ✅          |
| Policy as Code | ❌         | ✅          |
| Audit logs     | ❌         | ✅          |

***

### ✅ Experience Insight

> For regulated environments, Terraform Enterprise is preferred due to RBAC and policy enforcement.

***

## 🔹 9. What is OpenTofu?

### ✅ Concept

*   Open‑source fork of Terraform
*   Community‑driven
*   Same HCL syntax

***

### ✅ Hands‑On Reality

```bash
tofu init
tofu plan
tofu apply
```

***

### ✅ Interview Line

> OpenTofu is a community‑maintained alternative to Terraform with vendor neutrality.

***

## 🔹 10. Resource vs Data Source

### ✅ Resource

Creates or manages infrastructure

```hcl
resource "aws_instance" "web" {
  ami           = "ami-xyz"
  instance_type = "t3.micro"
}
```

***

### ✅ Data Source

Reads existing infrastructure

```hcl
data "aws_vpc" "default" {
  default = true
}
```


