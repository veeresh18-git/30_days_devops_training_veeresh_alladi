Here’s a **clean, senior-level, interview-ready answer** you can use 👇

***

# ✅ ✅ `/var is 90% full – what will you do?`

### 🎯 Short Interview Answer

> I would first verify disk usage, identify which directories are consuming space (commonly `/var/log`), clean up safely, and then implement preventive measures like log rotation and monitoring.

***

# 🔍 Step-by-Step Approach (Structured – SRE Level)

## 🔹 1. Confirm Disk Usage

```bash
df -h
```

*   Verify `/var` partition utilization
*   Identify impacted filesystem

***

## 🔹 2. Identify Space Consumption

```bash
du -sh /var/* | sort -hr
```

👉 Focus on:

*   `/var/log`
*   `/var/lib`
*   `/var/tmp`
*   `/var/cache`

***

## 🔹 3. Drill Down

```bash
du -sh /var/log/* | sort -hr
```

👉 Most common root cause:

*   Large log files (nginx, app logs, syslog)

***

## 🔹 4. Find Large Files

```bash
find /var -type f -size +100M
```

***

## 🔹 5. Safe Cleanup

### ✅ Truncate Logs (Preferred)

```bash
truncate -s 0 /var/log/syslog
```

***

### ✅ Delete Old Logs

```bash
find /var/log -type f -mtime +7 -delete
```

***

### ✅ Clear Package Cache

```bash
apt clean
# or
yum clean all
```

***

### ✅ Clear Temp Files

```bash
rm -rf /var/tmp/*
```

***

## 🔹 6. Hidden Issue Check (Very Important)

👉 Sometimes files are deleted but still held by processes:

```bash
lsof | grep deleted
```

✅ Fix:

*   Restart the service holding the file

***

## 🔹 7. Root Cause Analysis

Ask:

*   Why did space grow?
    *   Log flooding?
    *   Debug logs enabled?
    *   Application bug?
    *   Old backups?
    *   Docker logs/images?

***

## 🔹 8. Prevention (Very Important)

### ✅ Enable Log Rotation

```bash
cat /etc/logrotate.conf
```

Force run:

```bash
logrotate -f /etc/logrotate.conf
```

***

### ✅ Monitoring & Alerts

*   Set alerts at **70–80%**
*   Use:
    *   CloudWatch / Prometheus

***

❌ Delete random files blindly  
❌ Remove entire `/var/log`  
❌ Restart system without analysis

***

# 🎯 Perfect Final Answer (Say This)

> I would check disk usage using `df -h`, then identify large directories using `du`, focusing on `/var/log`. I would clean or truncate large logs, remove old files, and check for processes holding deleted files using `lsof`. Finally, I would perform RCA and implement log rotation and monitoring to prevent recurrence.

