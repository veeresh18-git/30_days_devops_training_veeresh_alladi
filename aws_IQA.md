
---

# 1. Design scalable multi-tier app

## What I say in interview

“I usually design for **high availability, fault tolerance, scalability, and security**.”

Architecture:

```text
Users
 |
:contentReference[oaicite:0]{index=0}
 |
:contentReference[oaicite:1]{index=1}
 |
:contentReference[oaicite:2]{index=2}
 |
:contentReference[oaicite:3]{index=3}
 |
App tier (EC2 ASG / EKS) in private subnet
 |
:contentReference[oaicite:4]{index=4} Multi-AZ
 |
:contentReference[oaicite:5]{index=5} for static files
```

---

## Hands-on steps

### Step 1: Create VPC

```bash
aws ec2 create-vpc --cidr-block 10.0.0.0/16
```

---

### Step 2: Create subnets

Public:

```text
10.0.1.0/24
10.0.2.0/24
```

Private:

```text
10.0.3.0/24
10.0.4.0/24
```

---

### Step 3: Attach Internet Gateway

```bash
aws ec2 attach-internet-gateway
```

---

### Step 4: NAT for private subnet

Create AWS NAT Gateway.

---

### Step 5: Deploy app in ASG

```bash
aws autoscaling create-auto-scaling-group
```

min=2 max=10

---

### Step 6: DB

Enable:

* Multi-AZ
* automated backups
* encryption

---

### Best practice

* private DB
* ALB public only
* IAM roles
* CloudWatch alarms

---

# 2. What is NAT Gateway?

## Answer

“NAT lets private subnet instances access internet outbound only.”

Example:

```text
Private EC2 -> NAT -> Internet
```

Use cases:

* yum update
* apt install
* docker pull
* S3 access (if no endpoint)

---

## Hands-on

Create Elastic IP:

```bash
aws ec2 allocate-address
```

Create NAT:

```bash
aws ec2 create-nat-gateway
```

Update route:

```text
0.0.0.0/0 -> nat-xxxx
```

---

## Troubleshooting

Inside private EC2:

```bash
curl google.com
```

If fail:
check:

* route table
* NAT status
* security group
* NACL

---

# 3. Private subnet internet access

How:

```text
Private subnet route:
0.0.0.0/0 -> NAT
```

Verify:

```bash
ip route
curl https://google.com
```

---

Common issue:
No route table attached.

Fix:
Attach correct private route table.

---

# 4. Subnet communication in VPC

Default route:

```text
10.0.0.0/16 local
```

Means all subnets can talk.

Example:
App:

```text
10.0.1.10
```

DB:

```text
10.0.2.20
```

Test:

```bash
telnet 10.0.2.20 3306
```

If blocked:
check SG/NACL.

---

# 5. NACL vs Security Group

## Security Group

Attached to ENI/instance.

Example:
Allow:

```text
TCP 22 from office IP
TCP 443 from ALB
```

Stateful.

---

## NACL

Subnet level.

Can deny:

```text
DENY all from 1.2.3.4
```

Stateless.

---

Hands-on:
Console:
VPC → Security Groups / Network ACLs

CLI:

```bash
aws ec2 describe-security-groups
```

---

# 6. EC2 unexpected termination

My flow:

### Check state reason

Console:
EC2 → Status checks

CLI:

```bash
aws ec2 describe-instances
```

Look:

```text
StateReason
```

---

### Check CloudWatch

CPU spike?
Memory?

---

### Check logs

```bash
journalctl -xe
dmesg
```

Look for:

```text
OOM killed
```

---

### Common causes

* spot interruption
* ASG replaced unhealthy node
* kernel panic
* EBS issue

---

# 7. Lambda failure troubleshooting

First:
CloudWatch logs.

```bash
aws logs tail /aws/lambda/myfn --follow
```

Look:

```text
Timeout
AccessDenied
Memory exceeded
```

---

Increase timeout:

```bash
aws lambda update-function-configuration \
--timeout 60
```

Increase memory:

```bash
--memory-size 1024
```

---

Common issue:
IAM deny.

Fix:
attach role permission.

---

# 8. RDS storage full

Check metric:
Amazon CloudWatch → FreeStorageSpace

---

Increase:

```bash
aws rds modify-db-instance \
--allocated-storage 200
```

---

Cleanup:
MySQL:

```sql
purge binary logs;
```

Postgres:
vacuum.

---

Enable autoscaling.

---

# 9. Accidental resource deletion

## S3

If versioning enabled:
restore previous object.

CLI:

```bash
aws s3api list-object-versions
```

---

## RDS

Restore snapshot:

```bash
aws rds restore-db-instance-from-db-snapshot
```

---

## EC2

Use AMI or EBS snapshot.

Best practice:
enable deletion protection.

---

# 10. Cost optimization

What I do monthly:

### EC2

Check idle:
AWS Cost Explorer

Rightsize:
m5.large → t3.medium

---

### EBS

Find unattached:

```bash
aws ec2 describe-volumes \
--filters Name=status,Values=available
```

Delete old volumes.

---

### S3 lifecycle

Move:
Standard → Glacier

---

Savings:
Usually 20–30%.

---

# 11. ASG not launching instances

Check:
ASG Activity tab.

Common:

```text
Launch failed
```

Reasons:

* bad AMI
* subnet full
* quota exceeded

CLI:

```bash
aws autoscaling describe-scaling-activities
```

---

# 12. EFS usage and issues

Use:
shared storage for multiple EC2/EKS pods.

Mount:

```bash
sudo mount -t nfs4 fs-123:/ /mnt/efs
```

Issues:

* port 2049 blocked
* mount target missing
* latency

Check:

```bash
telnet efs-endpoint 2049
```

---

# 13. EFS vs EBS

## EBS

One instance.
Best for DB.

Attach:

```bash
aws ec2 attach-volume
```

---

## EFS

Many instances.
Best for shared app data.

---

# 14. Disable IAM user console access

CLI:

```bash
aws iam delete-login-profile \
--user-name veeresh
```

Now only API keys work.

---

# 15. Cross-account S3 from Lambda

Lambda role in Account A.

Assume role in Account B:

Python:

```python
sts.assume_role(...)
```

Need:

* trust policy
* bucket policy

---

# 16. STS usage

Temporary credentials.

CLI:

```bash
aws sts assume-role \
--role-arn arn:aws:iam::123:role/demo
```

Returns:

* AccessKey
* Secret
* SessionToken

---

# 17. Trust policy

Defines **who can assume role**.

Example:

```json
{
 "Principal": {
   "AWS": "arn:aws:iam::111111111111:root"
 }
}
```

Without this:
AssumeRole fails.

---

# 18. EBS issues in Kubernetes

Check:

```bash
kubectl get pvc
kubectl describe pvc
```

Common:
PVC pending.

Cause:
wrong AZ.

Fix:
ensure node + EBS same AZ.

---

# 19. Secrets Manager vs Parameter Store

Secrets:
DB password rotation.

Fetch:

```bash
aws secretsmanager get-secret-value
```

---

Parameter store:
app configs:

```bash
aws ssm get-parameter
```

---

# 20. IAM user vs role

Never use IAM user for apps.

Bad:
hardcoded keys.

Good:
EC2/Lambda role.

Example:
Attach role to EC2:

```bash
aws ec2 associate-iam-instance-profile
```

No secrets in code.

---

# Interview tip

Always answer like this:

> “In one incident, our Lambda failed due to timeout. I checked CloudWatch logs, increased memory from 256MB to 1024MB, optimized code, and added alarms to prevent recurrence.”

That sounds **senior**.
