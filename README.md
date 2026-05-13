# 📘 DevOps & Cloud Interview Questions Repository


***

# 🔹 Git

*   Git Fork vs Git Clone
*   Explain a scenario where you used Git Fork instead of Git Clone
*   Git Fetch vs Git Pull
*   Show how Git Fetch and Pull work in real-time
*   Which command do you use mostly – Fetch or Pull and why?
*   Git Rebase vs Git Merge
*   Explain Git Merge vs Git Rebase in interviews (short)
*   Explain Git branching strategy used in your organization
*   Explain challenges faced with Git
*   Describe a recent Git issue and how you solved it
*   How do you handle merge conflicts?
*   Explain Git merge strategies (Ours vs Theirs)
*   Have you used Git tags? Why?
*   How do you combine multiple commits into one?
*   Explain 10 Git commands you use daily
*   How do you ignore a file in Git?
*   What is the purpose of the `.git` folder?
*   Can you restore a deleted `.git` folder?
*   Secret leaked in Git – how do you handle it?

***

# 🔹 Linux

*   10 Linux commands used daily
*   Can you restore a lost PEM file?
*   `/var` is 90% full – what will you do?
*   Linux server high CPU – how to fix?
*   Nginx connection refused – how to troubleshoot?
*   SSH not working – debugging steps
*   Find logs older than 7 days
*   Delete logs older than 30 days
*   Cron + shell script for log rotation
*   Bulk user creation from CSV
*   Service health monitoring script
*   Delete files larger than 100MB
*   List users logged in today
*   Website not loading – investigation steps
*   Remove first and last line using `sed`
*   Types of variables in Linux
*   kill vs kill -9

***

# 🔹 Networking

*   Explain DNS
*   OSI model and request flow
*   Forward proxy vs Reverse proxy
*   User reports slowness – approach
*   Curl works with IP but not domain – why?
*   502 error – possible causes
*   Difference between 0.0.0.0 and 127.0.0.1
*   Public vs Private subnet
*   Fix wrongly created private subnet

***

# 🔹 CI/CD (Jenkins)

*   What are Jenkins shared libraries?
*   Build targets used daily
*   Artifact repository usage
*   Artifactory setup in Maven
*   Build passes locally but fails in CI
*   CI succeeds but prod app is broken
*   Pipeline slowing down over time
*   Pipeline not triggering on push
*   Dependency download failure in build
*   Python build fails in CI but not locally
*   Python application build process
*   Static code analysis use cases
*   Static analysis slowing pipeline – fix
*   ArgoCD OutOfSync without Git changes
*   Send email on Jenkins failure

***

# 🔹 Terraform

*   for\_each vs for
*   What are Terraform modules?
*   Purpose of statefile
*   Should statefile be stored in Git?
*   Terraform statefile management
*   Concurrent state updates issue
*   Where to store statefile without cloud?
*   Terraform Enterprise vs Community
*   What is OpenTofu?
*   Resource vs Data Source

***

# 🔹 Docker

*   Container exits immediately – troubleshooting
*   Purpose of EXPOSE in Dockerfile
*   Port not accessible after mapping
*   Data loss after container restart
*   Image updated but changes not reflected
*   Permission denied inside container
*   Docker host disk full – cleanup
*   Debug live container
*   Container registry used
*   CMD vs ENTRYPOINT
*   Docker commands used daily
*   Force remove container

***

# 🔹 Kubernetes

*   Explain Kubernetes architecture
*   kubectl apply flow
*   Purpose of services
*   Why not use Pod IP directly?
*   Types of services
*   Labels and selectors
*   NodePort vs LoadBalancer
*   Kube-proxy role
*   LoadBalancer disadvantages
*   Headless service use cases
*   Cross-namespace communication
*   Restrict DB access to one app
*   Deployment strategy used
*   Rollback strategy
*   Avoid rollbacks design
*   CoreDNS role
*   Node taints handling
*   CrashLoopBackOff debugging
*   Liveness vs Readiness probes
*   Ingress vs LoadBalancer
*   Ingress troubleshooting
*   Why need Ingress controller?
*   Deployment replicas mismatch
*   ConfigMap not updating
*   Node affinity usage
*   Node affinity vs node selector
*   Container runtime in Kubernetes
*   Kubernetes QoS
*   Requests vs limits
*   Challenges faced in Kubernetes
*   Can master node schedule pods?
*   Horizontal vs Vertical scaling
*   Kubernetes secrets types

***

# 🔹 Observability

*   Monitoring vs Observability
*   Custom logs and metrics
*   Prometheus metrics usage
*   Observability implementation experience
*   Logs vs Metrics vs Traces
*   Push vs Pull monitoring
*   Observability tools used
*   App slowness without logs/CPU issues
*   Distributed tracing in microservices
*   OOMKilled debugging
*   Reducing alert noise

***

# 🔹 AWS

*   Design scalable multi-tier app
*   What is NAT Gateway?
*   Private subnet internet access
*   Subnet communication in VPC
*   NACL vs Security Group
*   EC2 unexpected termination
*   Lambda failure troubleshooting
*   RDS storage full
*   Accidental resource deletion (S3/RDS/EC2)
*   Cost optimization example
*   AWS challenge faced
*   ASG not launching instances
*   AWS services used daily
*   EFS usage and issues
*   EFS vs EBS
*   Disable IAM user console access
*   Cross-account S3 access from Lambda
*   STS usage
*   Trust policy
*   Cross-account DynamoDB access
*   EBS issues in Kubernetes
*   Secrets Manager vs Parameter Store
*   Database-related activities
*   Lambda use case experience
*   IAM user vs role

***

# 🔹 Azure

*   Azure VPN types
*   Azure AD authentication in VPN
*   Point-to-site vs Site-to-site VPN
*   Azure networking basics

***


senior level interview questions

***

# 🔥 1. System Design (VERY IMPORTANT)

👉 Most candidates fail here.

### Missing Questions:

*   Design a **highly available system (end-to-end)**
*   Design **rate limiting system**
*   Design **CI/CD pipeline for microservices**
*   Design **logging/monitoring architecture**
*   How would you design **zero-downtime deployment?**
*   How will you design **multi-region failover?**

### Interview Expectation:

> They want architecture thinking, not tools.

***

# 🔥 2. Production & Incident Handling (SRE Core)

👉 This is CRITICAL for SRE roles.

### Missing Questions:

*   Walk me through a **production incident you handled**
*   What is your **incident response process?**
*   How do you perform **RCA (Root Cause Analysis)?**
*   What is **Postmortem and how do you write it?**
*   What is **SLA, SLO, SLI?**
*   How do you **reduce MTTR?**
*   What is **error budget?**

***

# 🔥 3. Security (Often Asked, Missing)

### Missing Questions:

*   How do you secure:
    *   Kubernetes cluster?
    *   CI/CD pipeline?
*   What is **RBAC in Kubernetes?**
*   How do you manage **secrets securely?**
*   How do you prevent **secret leaks in Git?**
*   What is **IAM least privilege principle?**
*   Difference between:
    *   TLS vs SSL
    *   OAuth vs JWT

***

# 🔥 4. Kubernetes Advanced (Some Gaps)

You covered basics well, but missing:

*   What is **etcd and how backup works?**
*   What is **CNI (Container Network Interface)?**
*   How does **kube-proxy work internally?**
*   What is **HPA vs VPA vs Cluster Autoscaler?**
*   What is **PodDisruptionBudget?**
*   What is **DaemonSet vs StatefulSet?**
*   What is **NetworkPolicy?**

***

# 🔥 5. CI/CD Advanced

Missing practical depth:

*   Blue-Green vs Canary vs Rolling (deep comparison)
*   How do you secure pipelines?
*   How do you implement:
    *   **Approval gates**
    *   **Rollback automation**
*   GitOps vs CI/CD difference
*   How ArgoCD works internally

***

# 🔥 6. Observability Deep Dive

You covered basics, but not depth:

*   What is **Golden Signals?**
*   What is **RED method / USE method?**
*   Difference:
    *   Alerting vs Monitoring
*   How to design **alerting strategy**
*   How to avoid alert fatigue (very important)

***

# 🔥 7. Cloud (AWS/Azure Real Depth Missing)

### Missing AWS:

*   VPC design architecture (real-world)
*   ELB types: ALB vs NLB
*   Route53 routing policies
*   Disaster recovery strategies:
    *   Pilot light
    *   Warm standby

### Missing Azure:

*   Azure networking (VNet, NSG)
*   Azure DevOps pipelines
*   Managed identity
*   Key Vault

***

# 🔥 8. Performance & Scalability

*   How do you debug:
    *   memory leak?
    *   latency issue?
*   Difference:
    *   horizontal vs vertical scaling (deep)
*   How to handle:
    *   traffic spike
    *   DDoS (basic level)

***

# 🔥 9. Database / State Management (BIG GAP)

👉 This is often asked and missing.

*   SQL vs NoSQL
*   Indexing basics
*   Read replicas vs sharding
*   DB backup strategies
*   Handling DB bottlenecks


🔥 1. Production Incidents

App down but pods are running ✅ what next?
DB latency increased suddenly ✅ how to debug?
Sudden spike in 5xx errors ✅ steps?
Deployment succeeded but app broken ✅ why?


🔥 2. Debugging Unknown Issues

Logs ✅ OK
CPU ✅ OK
Memory ✅ OK
👉 Then what?

👉 This is where most fail.

🔥 3. Multi-layer Troubleshooting
Example:

User → Load Balancer → Ingress → Service → Pod → DB

👉 Where is the failure?

🔥 4. Trade-off Questions

Why Kubernetes vs ECS?
Why Terraform vs CloudFormation?
When NOT to use microservices?
When NOT to use Docker?


🔥 5. “Tell Me About a Time…” (Behavioral + Tech)

Tell me about a production outage
Tell me about a failed deployment
Tell me about a mistake you made

👉 These are make-or-break questions

🔥 6. Scale / Performance Scenarios

Traffic increased 10x overnight
One service becomes bottleneck
Latency issue only in production





