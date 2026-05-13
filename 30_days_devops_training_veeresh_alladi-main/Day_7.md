1. docker version
Shows Docker client + server versions.
docker version

Helps confirm Docker is installed.

2. docker pull
Downloads an image from Docker Hub.
Example (pull NGINX):
docker pull nginx

This matches the idea in the Docker Curriculum where images are the basis of containers and can be pulled from registries .

3. docker images
List all images on your machine.
docker images

Useful for checking what you have downloaded.

4. docker run
Runs a container from an image.
Example from Docker’s tutorial:
docker run -dp 80:80 docker/getting-started

General example:
docker run -d -p 8080:80 nginx


-d → run in background
-p → map host port → container port
This matches the port‑mapping explanation from GeeksforGeeks .


5. docker ps
List running containers.
docker ps

List all (stopped + running):
docker ps -a


6. docker stop
Gracefully stops a container.
docker stop <container_id>


7. docker kill
Force‑kills a container immediately.
docker kill <container_id>

Edureka explains the difference clearly:

stop = graceful
kill = immediate termination


8. docker rm
Remove containers.
Remove one:
docker rm <id>

Remove ALL:
docker rm -f $(docker ps -aq)


9. docker rmi
Delete images.
docker rmi nginx


10. docker logs
View logs from a running container.
docker logs <container_id>

Useful for debugging web servers, APIs, Python apps.

11. docker exec
Enter a running container.
docker exec -it <container_id> /bin/bash

Great for debugging file systems, configs, and source code.

12. docker build
Build an image from Dockerfile.
docker build -t myapp:v1 .

This follows DataCamp’s coverage of Dockerfile syntax and commands like FROM, RUN, COPY, CMD .

13. docker tag
Create a new tag for an image.
docker tag myapp:v1 myrepo/myapp:v1


14. docker push
Push your image to Docker Hub.
docker push myrepo/myapp:v1


15. docker pull (with tags and platforms)
Example from GreatLearning:
docker pull --platform linux/x86_64 httpd:latest


16. docker network (basic example)
Create a network:
docker network create mynet

Run containers in it:
docker run -d --network=mynet --name db mysql
docker run -d --network=mynet --name app myapp

Networking is included in the essential commands guide .

17. docker volume
Create a volume:
docker volume create mydata

Use it:
docker run -d -v mydata:/var/lib/mysql mysql


18. docker compose (docker‑compose)
Compose is included in the essential command set for developers .
Run multiple services:
docker compose up -d
docker compose down



Hands‑On Labs (Live Examples)
These examples reflect the real hands-on labs described in Docker 101  and K21Academy .

Lab 1: Run NGINX web server
docker pull nginx
docker run -d -p 80:80 nginx
docker ps
curl localhost


Lab 2: Enter Container & Explore
docker exec -it <id> /bin/bash
ls /
exit


Lab 3: Build Your Own App
Dockerfile:
FROM python:3.12-slim
WORKDIR /app
COPY . .
RUN pip install fastapi uvicorn
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]

Build and run:
docker build -t pythonapp .
docker run -d -p 8000:8000 pythonapp


Lab 4: Remove Everything
Stop + delete all containers:
docker rm -f $(docker ps -aq)

Delete all images:
docker rmi -f $(docker images -q)


If you want, I can prepare:

A printable Docker command cheatsheet
A full 2‑hour Docker Workshop Outline
A mini‑project your students can build today
