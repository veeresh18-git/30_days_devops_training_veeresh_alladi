# 1. Container exits immediately — how do you troubleshoot?

**Answer:**

“First, I check whether the main process inside the container exited.”

Step 1:

```bash
docker ps -a
```

Look for:

```text id="y2b0r0"
Exited (1)
```

Step 2:

```bash
docker logs <container_id>
```

Common reasons:

* app crash
* wrong command
* missing env vars
* DB connection failure

Step 3:
Run interactive shell:

```bash
docker run -it image_name /bin/sh
```

Check:

* files
* env vars
* permissions

**Interview line:**
“A container lives only as long as PID 1 is alive.”

---

# 2. Purpose of `EXPOSE` in Dockerfile

Example:

```dockerfile
EXPOSE 8080
```

**Answer:**

“`EXPOSE` is documentation inside the image. It tells which port the app listens on, but it does **not publish** the port.”

Still need:

```bash
docker run -p 8080:8080 app
```

Important distinction.

---

# 3. Port not accessible after mapping

Example:

```bash
docker run -p 8080:80 nginx
```

Troubleshooting:

1. Verify mapping:

```bash
docker ps
```

2. Check inside container:

```bash
docker exec -it container sh
netstat -tulpn
```

3. App binding issue:
   Wrong:

```text id="zj6n2z"
127.0.0.1:80
```

Correct:

```text id="71nt54"
0.0.0.0:80
```

4. Check firewall/security groups.

Common root cause: app listening only on localhost.

---

# 4. Data loss after container restart

**Answer:**

“Container filesystem is ephemeral.”

Wrong:

```bash
docker run mydb
```

Correct:

```bash
docker run -v myvolume:/var/lib/mysql mysql
```

Or bind mount:

```bash
-v /host/data:/app/data
```

Use volumes for:

* databases
* logs
* uploads

---

# 5. Image updated but changes not reflected

Check:

```bash
docker images
```

Possible causes:

* cached layers
* old container still running

Fix:

```bash
docker build --no-cache -t app:v2 .
docker stop old
docker rm old
docker run app:v2
```

For Docker Compose:

```bash
docker compose up --build
```

---

# 6. Permission denied inside container

Check:

```bash
ls -lrt
whoami
id
```

Cause:
host file ownership mismatch.

Fix:

```dockerfile
RUN chown -R appuser:appuser /app
USER appuser
```

Or:

```bash
docker run --user 1000:1000
```

Common with:

* mounted volumes
* Jenkins
* Kubernetes pods

---

# 7. Docker host disk full — cleanup

Check:

```bash
df -h
docker system df
```

Cleanup:

Unused containers:

```bash
docker container prune
```

Unused images:

```bash
docker image prune -a
```

Unused volumes:

```bash
docker volume prune
```

Full cleanup:

```bash
docker system prune -a --volumes
```

**Warning:** verify before pruning.

---

# 8. Debug live container

Attach:

```bash
docker exec -it container /bin/sh
```

Check:

```bash
ps -ef
env
netstat -tulpn
df -h
top
```

Logs:

```bash
docker logs -f container
```

Copy files:

```bash
docker cp
```

This is my standard live-debug workflow.

---

# 9. Container registry used

Common ones:

* [Docker Hub](https://hub.docker.com/?utm_source=chatgpt.com)
* [Amazon ECR](https://aws.amazon.com/ecr/?utm_source=chatgpt.com)
* [Azure Container Registry](https://azure.microsoft.com/en-us/products/container-registry/?utm_source=chatgpt.com)
* [Google Artifact Registry](https://cloud.google.com/artifact-registry?utm_source=chatgpt.com)
* [GitHub Container Registry](https://ghcr.io/?utm_source=chatgpt.com)
* [JFrog Artifactory](https://jfrog.com/artifactory/?utm_source=chatgpt.com)

**Interview answer:**
“In enterprise, I mostly used [Amazon ECR](https://aws.amazon.com/ecr/?utm_source=chatgpt.com) integrated with CI/CD.”

---

# 10. CMD vs ENTRYPOINT

### CMD

Default command; overridable.

```dockerfile
CMD ["python","app.py"]
```

Run:

```bash
docker run app bash
```

Overrides CMD.

---

### ENTRYPOINT

Fixed executable.

```dockerfile
ENTRYPOINT ["python"]
CMD ["app.py"]
```

Run:

```bash
docker run app test.py
```

Executes:

```text id="72i1s6"
python test.py
```

**Interview line:**
“ENTRYPOINT defines executable; CMD provides default arguments.”

---

# 11. Docker commands used daily

```bash
docker ps
docker ps -a
docker images
docker logs -f
docker exec -it
docker build
docker run
docker stop
docker rm
docker rmi
docker pull
docker push
docker inspect
docker stats
docker system df
```

Daily SRE toolbox.

---

# 12. Force remove container

Stop + remove:

```bash
docker rm -f container_name
```

What it does:

* sends SIGKILL
* removes container immediately

Useful for:

* stuck containers
* hung deployments

Here’s how I’d answer this in an **interview** — explain **what it is, when I use it, and a real-world example**.

---

# 1. `docker inspect`

### What is it?

Returns **low-level JSON metadata** about a container/image/network/volume.

Command:

```bash
docker inspect <container_name>
```

---

### When do I use it?

I use it when I need **deep debugging information**, like:

* container IP address
* mounted volumes
* environment variables
* restart policy
* port mappings
* entrypoint/cmd
* network details

---

### Real-world example 1: Find container IP

```bash
docker inspect nginx | grep IPAddress
```

Output:

```text
172.17.0.3
```

Useful when debugging container-to-container communication.

---

### Real-world example 2: Verify volume mount

```bash
docker inspect mysql
```

Look for:

```json
"Mounts": [
 "/host/db:/var/lib/mysql"
]
```

Helps debug “why data not persisting”.

---

### Interview line:

“I use `docker inspect` when logs are not enough and I need runtime metadata.”

---

# 2. `docker stats`

### What is it?

Shows **live resource usage**.

Command:

```bash
docker stats
```

Shows:

* CPU %
* Memory
* Network I/O
* Disk I/O
* PIDs

---

### When do I use it?

When:

* container slow
* high CPU
* memory leak suspicion
* pod/container throttling

---

### Real-world example

Output:

```text
nginx    90% CPU
redis    120MB RAM
```

Interpretation:

* nginx under heavy load
* maybe scale horizontally
* maybe bad code loop

---

### Interview line:

“I use `docker stats` during incidents to identify noisy containers quickly.”

---

# 3. `docker system df`

### What is it?

Shows **Docker disk usage summary**.

Command:

```bash
docker system df
```

Output:

```text
Images      20GB
Containers   2GB
Volumes      8GB
Build cache 15GB
```

---

### When do I use it?

When:

* Docker host disk is full
* CI runner storage issue
* old image cleanup needed

---

### Real-world example

Disk alert:

```text
/var/lib/docker 95%
```

Run:

```bash
docker system df
```

See:

```text
Build cache 30GB
```

Then:

```bash
docker builder prune
```

Recovered 30GB.

---

### Interview line:

“It helps me identify what exactly is consuming Docker storage before pruning.”

---

# 4. `docker save`

### Purpose:

Exports **Docker image** as tar file.

Command:

```bash
docker save -o nginx.tar nginx:latest
```

Creates:

```text
nginx.tar
```

---

### When used?

* offline transfer
* air-gapped environment
* backup image
* move image without registry

---

### Example

Prod server has no internet.

Laptop:

```bash
docker save -o myapp.tar myapp:v1
scp myapp.tar server:
```

On server:

```bash
docker load -i myapp.tar
```

---

### Interview line:

“I use `save/load` for offline image movement.”

---

# 5. `docker export`

### Purpose:

Exports **container filesystem only**.

Command:

```bash
docker export container1 > app.tar
```

---

### Difference:

No:

* image layers
* metadata
* history

Just files.

---

### When used?

* backup running container filesystem
* migration
* forensic/debugging

---

### Example

Need app logs/config from broken container:

```bash
docker export myapp > backup.tar
```

Extract:

```bash
tar -xvf backup.tar
```

Useful for incident recovery.

---

# `save` vs `export`

|               | docker save | docker export     |
| ------------- | ----------- | ----------------- |
| works on      | image       | container         |
| keeps layers  | yes         | no                |
| keeps history | yes         | no                |
| use case      | move image  | backup filesystem |

---

### Interview answer:

“`docker save` is for **images**, preserving layers and metadata.
`docker export` is for **containers**, exporting only filesystem state.”

