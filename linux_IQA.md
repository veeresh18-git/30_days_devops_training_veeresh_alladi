# 1. What are 10 Linux commands you use daily?

**Answer (interview style):**

“In day-to-day SRE/DevOps work, these are my most-used Linux commands:”

```bash
ls -lrt        # list files by time
cd / pwd       # navigation
grep           # search logs/configs
find           # locate files
tail -f        # live log monitoring
cat/less       # read files
df -h          # filesystem usage
du -sh         # folder size analysis
ps -ef         # process listing
top / htop     # CPU/memory monitoring
systemctl      # service management
journalctl     # systemd logs
netstat / ss   # check listening ports
chmod/chown    # permissions
scp/rsync      # file transfer
```

**Example:**
“If app is down, I usually start with `systemctl status`, then `journalctl`, then `ss -tulpn`, then `top`.”

---

# 2. Can you restore a lost PEM file?

**Answer:**

“Short answer: **No**, if the private key is lost, it cannot be regenerated because AWS stores only the public key.”

**What I do instead:**

1. Stop instance (if required)
2. Detach root volume
3. Attach to another helper EC2
4. Update `~/.ssh/authorized_keys`
5. Reattach volume
6. Start instance
7. Login with new key

**Alternative:** Use **AWS SSM Session Manager** if enabled.

**Interview line:**
“I always recommend enabling SSM to avoid dependency on PEM files.”

---

# 3. /var is 90% full — what will you do?

**Answer:**

“My first goal is to identify what is consuming space.”

Step 1:

```bash
df -h
```

Step 2:

```bash
du -sh /var/* | sort -hr
```

Typical culprits:

* `/var/log`
* `/var/lib/docker`
* `/var/cache`
* old package files
* rotated logs

Cleanup:

```bash
journalctl --vacuum-time=7d
docker system prune -a
yum clean all
```

Preventive:

* logrotate
* monitoring alert at 80%

**Interview line:**
“I don’t delete blindly; I first identify safe-to-remove data.”

---

# 4. Linux server high CPU — how to fix?

**Answer:**

“My approach is systematic.”

Step 1:

```bash
top
```

Find process.

Step 2:

```bash
ps -ef | grep <pid>
```

Check:

* expected workload?
* stuck loop?
* memory leak?
* zombie?

Step 3:

```bash
strace -p <pid>
```

(if needed)

Temporary:

```bash
renice +10 <pid>
kill -15 <pid>
```

Permanent:

* app bug fix
* autoscaling
* optimize queries/code

**Interview line:**
“I treat symptom first, then root cause.”

---

# 5. Nginx connection refused — troubleshoot

**Answer:**

Check service:

```bash
systemctl status nginx
```

Check port:

```bash
ss -tulpn | grep 80
```

Config validation:

```bash
nginx -t
```

Logs:

```bash
tail -f /var/log/nginx/error.log
```

Check firewall:

```bash
firewall-cmd --list-all
```

Cloud:

* Security Group
* NACL

Common causes:

* service down
* wrong bind IP
* port blocked
* bad config

---

# 6. SSH not working — debugging steps

**Answer:**

Layer-by-layer:

1. Network

```bash
ping server-ip
telnet server-ip 22
```

2. SSH service

```bash
systemctl status sshd
```

3. Port listening

```bash
ss -tulpn | grep 22
```

4. Logs

```bash
journalctl -u sshd
```

5. Permissions

```bash
chmod 400 key.pem
chmod 700 ~/.ssh
```

6. Firewall / SG

Debug:

```bash
ssh -vvv user@host
```

**Interview line:**
“Verbose SSH output often tells exactly where failure occurs.”

---

# 7. Find logs older than 7 days

```bash
find /var/log -type f -mtime +7
```

Explanation:

* `-type f` = files only
* `+7` = older than 7 days

---

# 8. Delete logs older than 30 days

Safer:

```bash
find /var/log -type f -mtime +30 -exec rm -f {} \;
```

Dry run first:

```bash
find /var/log -type f -mtime +30
```

**Interview line:**
“I always dry-run before delete.”

---

# 9. Cron + shell script for log rotation

Script:

```bash
#!/bin/bash
find /app/logs -type f -mtime +7 -exec gzip {} \;
find /app/logs -type f -mtime +30 -delete
```

Cron:

```bash
0 2 * * * /opt/scripts/logrotate.sh
```

Meaning:
Runs daily at 2 AM.

---

# 10. Bulk user creation from CSV

CSV:

```text
user1
user2
user3
```

Script:

```bash
while read user
do
 useradd $user
 echo "Temp@123" | passwd --stdin $user
done < users.csv
```

Verify:

```bash
id user1
```

---

# 11. Service health monitoring script

```bash
#!/bin/bash
systemctl is-active nginx >/dev/null
if [ $? -ne 0 ]; then
   systemctl restart nginx
   echo "nginx restarted" | mail -s "Alert" admin@company.com
fi
```

**Real-world:** usually integrate with:

* Prometheus
* Grafana
* PagerDuty

---

# 12. Delete files larger than 100MB

Find:

```bash
find / -type f -size +100M
```

Delete:

```bash
find /tmp -type f -size +100M -delete
```

---

# 13. List users logged in today

```bash
who
```

More detail:

```bash
last | head
```

Current:

```bash
w
```

---

# 14. Website not loading — investigation steps

My layered approach:

### Layer 1: DNS

```bash
nslookup domain.com
```

### Layer 2: Reachability

```bash
ping
curl -v
```

### Layer 3: LB / Proxy

Check ALB/Nginx

### Layer 4: App

```bash
systemctl status app
```

### Layer 5: DB

DB connections?

### Layer 6: Logs

App + nginx + system

**Interview line:**
“I isolate layer-by-layer instead of guessing.”

---

# 15. Remove first and last line using sed

```bash
sed '1d;$d' file.txt
```

Meaning:

* `1d` = delete first
* `$d` = delete last

---

# 16. Types of variables in Linux

1. **Local variable**

```bash
name=veeresh
```

2. **Environment variable**

```bash
export JAVA_HOME=/usr/java
```

3. **System variable**
   Examples:

```bash
$PATH
$HOME
$USER
```

4. **Special variables**

```bash
$0
$1
$#
$?
$$
```

---

# 17. kill vs kill -9

`kill` = sends **SIGTERM (15)**
Graceful stop.

```bash
kill 1234
```

App can cleanup.

---

`kill -9` = sends **SIGKILL (9)**
Force kill.

```bash
kill -9 1234
```

Cannot be ignored.

**Interview line:**
“I always try `kill` first, use `kill -9` only as last resort.”
