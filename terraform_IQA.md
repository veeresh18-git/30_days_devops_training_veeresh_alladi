---

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

That instantly sounds senior.
