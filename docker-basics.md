# 🐳 Docker Essentials: Images & Containers

This document provides commonly used Docker commands for managing **images** and **containers**, along with useful explanations and best practices.

---

## 🧾 Version Checks

**Check Docker version**  
`docker --version`

**Check Docker Compose version**  
`docker-compose --version`

---

## 🖼️ Working with Docker Images

### 1. View Local Images

**List all images stored locally:**  
`docker images`

- Displays all images pulled from a registry  
- Includes: REPOSITORY, TAG, IMAGE ID, CREATED, SIZE

---

### 2. Tag an Image

**Add or override a tag on an image:**  
`docker tag in28min/todo-rest-api-h2:1.0.0.RELEASE in28min/todo-rest-api-h2:latest`

- You can assign **multiple tags** to the same image  
- Tags help version control the image  
- `latest` is just a tag — it may not be the most recent version

---

### 3. Pull Images from Registry

**Pull an image (default from Docker Hub):**  
`docker pull mysql`

**Pull a specific version:**  
`docker pull nginx:1.23`

- Format: `docker pull <name>:<tag>`  
- The default registry is `docker.io` unless otherwise specified

---

### 4. Search for Images

**Search Docker Hub for images:**  
`docker search mysql`

- `[OK]` indicates an official image

---

### 5. Inspect Image History

**Show image build layers:**  
`docker image history <image-id or name:tag>`

- Displays how the image was built (base image → layers → app)  
- Helps debug large image sizes or Dockerfile issues

---

### 6. View Image Metadata

**Inspect image details:**  
`docker image inspect <image-id or name:tag>`

- Metadata includes:  
  - Tags, Digest  
  - Created date, Docker version  
  - Entrypoint, Exposed ports  
  - Environment variables  
  - Layer structure  
  - Containers created from it

---

### 7. Remove Images

**Delete an image from local system:**  
`docker image remove <image-id>`  
`docker image rm <image-id>`  
`docker rmi <image-id>`

- All are valid aliases  
- Use `-f` to force delete even if containers exist

---

## 📦 Working with Docker Containers

### 1. List Containers

**Running containers only:**  
`docker ps`  
`docker container ls`

**All containers (running + stopped):**  
`docker ps -a`  
`docker container ls -a`

**Search containers by name:**  
`docker ps -a | grep my-app`

---

### 2. Start / Stop Containers

**Start a stopped container:**  
`docker start <container-name or ID>`

**Stop a running container (graceful shutdown):**  
`docker stop <container-name or ID>`  
`docker container stop <container-ID>`

**Pause a running container:**  
`docker container pause <container-ID>`  
- Temporarily freezes the container (it won’t serve requests)

**Unpause a paused container:**  
`docker container unpause <container-ID>`

---

### 3. Inspect Containers

**View container details:**  
`docker container inspect <container-ID>`

- Shows mount points, network config, volumes, restart policy, and more

---

### 4. Remove Containers

**Remove a single container:**  
`docker rm <container-name or ID>`  
`docker container rm <container-ID>`

**Remove all stopped containers:**  
`docker container prune`  
- Use with caution, as it deletes multiple containers

---

### 5. Force Kill a Container

**Kill a running container immediately:**  
`docker container kill <container-ID>`

---

### 6. Stop vs Kill — What's the Difference?

- **Stop** → Sends `SIGTERM` → Allows graceful shutdown  
- **Kill** → Sends `SIGKILL` → Forcefully stops without cleanup  
  ✅ _Use `docker stop` when possible for safety_

---

## 🚀 Launching Containers

1. `docker run postgres`  
   - `docker run` creates a new container. It doesn't reuse previous containers.

2. `docker run redis`

3. `docker run nginx:1.23`  
   - Format: `docker run {name}:{tag}`  
   - Creates a container from the given image and starts it

4. `docker run -d nginx:1.23`  
   - `-d` or `--detach` runs container in the background and prints container ID

5. `docker run --name web-app -d -p 9000:80 nginx:1.23`  
   - `--name` assigns a name to the container

6. `docker run in28min/todo-rest-api-h2:1.0.0.RELEASE`  
   - Repository: `todo-rest-api-h2`  
   - Tag: `1.0.0.RELEASE`  
   - Downloads image from Docker registry and runs it

7. `docker run -p 5000:5000 in28min/todo-rest-api-h2:1.0.0.RELEASE`  
   - `-p {HostPort}:{ContainerPort}`  
     - Containers run in an internal bridge network  
     - Need to expose container ports to host to access externally  
     - Enables running multiple containers of the same app on different ports  
     - Port binding makes services accessible on host machine  
     - `-p` is short for `--publish`  
     - Standard practice: use the same port on host and container  
   - Open `localhost:5000` in browser to access app

8. `docker run -p 5000:5000 -d in28min/todo-rest-api-h2:1.0.0.RELEASE`  
   - `-d` runs the container in the background  
   - Keeps app running even if terminal is closed  
   - Returns container ID

9. `docker run -p 5001:5000 -d in28min/todo-rest-api-h2:1.0.0.RELEASE`  
   - Launches another container from the same image with a different host port

10. `docker container run -p 5000:5000 -d in28min/todo-rest-api-h2:1.0.0.RELEASE`  
    - Alternative syntax using `docker container run`

11. `docker run -p 5000:5000 -d --restart=always in28min/todo-rest-api-h2:1.0.0.RELEASE`  
    - Restart policy:  
      - Default is `no`  
      - `always` will auto-start container on Docker daemon restart  
      - Useful for services like databases or production apps

12. `docker run -p 5000:5000 -m 512m --cpu-quota=50000 -d in28min/todo-rest-api-h2:1.0.0.RELEASE`  
    - `-m 512m` sets memory limit  
    - `--cpu-quota=50000` (out of 100000) limits CPU usage to 50%  
    - Used to control resource allocation

13. `docker-compose up`  
    - Launches multi-container applications as defined in a `docker-compose.yml` file

---

🗒️ **Note:**  
- When you restart a container, all data/config inside it is lost  
- Containers are **ephemeral**  
- For **data persistence**, use **Docker Volumes**

---

## 📄 Viewing Container Logs

1. `docker logs <container>`  
   - View logs from the service running inside the container  
   - Shows logs present at the time of execution

2. `docker logs -f <container>`  
   - `-f` means "follow"  
   - Starts tailing the logs  
   - Press `Ctrl + C` to stop following

3. `docker logs <container> | tail`  
   - View the last part of the logs  
   - Useful for quick inspection of recent events

---

## 🖥️ Accessing the Container Terminal

Use the following commands to open a terminal inside a running container. This is helpful for inspecting logs, config files, directories, environment variables, etc.

### 1. Using Bash Shell

`docker exec -it <container> /bin/bash`

- `-it` enables interactive terminal mode  
- You are now inside the container as the root user  
- The container's virtual file system is accessible  
  - `ls` — list files  
  - `pwd` — print working directory  
  - `cd /` — navigate to root  
  - `env` — list environment variables  
  - `exit` — leave the container shell

### 2. Using sh Shell (alternative)

`docker exec -it <container> /bin/sh`

- Use when `/bin/bash` is not available in the container

---

## 🔐 SSH into MSSQL Docker Container

### 1. Find the container ID

`docker ps`

### 2. Connect to the MSSQL container

`docker exec -it <container_id> /bin/bash`

### 3. Connect to MSSQL Server

`/opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P <your_password>`

### 4. Run SQL Queries

- `USE YourDatabaseName;`  
- `SELECT * FROM YourTableName;`  
- `GO`

### 5. Exit sqlcmd

`QUIT`

### 6. Exit the Docker container

`exit`

---

## 🧩 Docker Compose

Docker Compose is used to run and manage **multiple containers** as a single service. It simplifies orchestration by handling network creation, dependencies, and shared configurations.

### ✅ Key Benefits

- Used for running multi-container applications  
- Automatically creates a **shared network** between the services  
- You don’t need to manually set up a Docker network

---

### ▶️ Starting Services Using a Compose File

`docker-compose -f mongo-docker-compose.yaml up`

- `-f` specifies the file to use (default is `docker-compose.yml`)

---

### ⏹️ Stopping Services

`docker-compose -f mongo-docker-compose.yaml down`

- Shuts down all services defined in the file  
- Also removes the containers, network, and volumes (if configured)

---

### 🧾 Sample `mongo-docker-compose.yaml`

This example sets up a custom app along with MongoDB and Mongo Express for visualization.

```yaml
version: '3'
services:
  my-app:
    image: 123123.dkr.ecr.eu-central-1.amazonaws.com/my-app:1.0
    # Note: You need to run `docker login` on the dev server to access private ECR
    ports:
      - 3000:3000

  mongodb:
    image: mongo
    ports:
      - 27017:27017
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin

  mongo-express:
    image: mongo-express
    ports:
      - 8080:8081
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
      - # (Add more env vars as needed)
```
📝 Docker Compose makes it easier to spin up full environments for local development, staging, and testing with just one command.

---

## 📄 Dockerfile for Node.js App (MongoDB + Mongo Express)

This section shows how to create a `Dockerfile` for a simple JavaScript application that connects to MongoDB, typically paired with **Mongo Express** for easy DB visualization.

> 🔁 In production, it's better to define environment variables in the `docker-compose.yml` file rather than directly in the Dockerfile.

---

### 🧪 Sample Dockerfile

```Dockerfile
FROM node:13-alpine

# Define environment variables (move to docker-compose if possible)
ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=password

# Create application directory inside the container
RUN mkdir -p /home/app

# Copy app files from host to container's filesystem
COPY ./app /home/app

# Set the default command to run the app
CMD ["node", "/home/app/server.js"]
```
## 🔍 Explanation of Key Instructions

**FROM node:13-alpine**  
- Uses a lightweight Node.js base image built on Alpine Linux  
- Smaller image size → faster builds and deployments  

---

**ENV**  
- Sets environment variables inside the image  
- It's better to define sensitive values like DB credentials in `docker-compose.yml` instead  

*Example for Compose:*  
```yaml
environment:
  - MONGO_DB_USERNAME=admin
  - MONGO_DB_PWD=password 
```
---

**RUN mkdir -p /home/app**  
- Creates a directory in the container’s filesystem to hold your application code  
- `-p` ensures the entire path is created if it doesn’t exist  

---

**COPY ./app /home/app**  
- Copies files from the host machine's `./app` directory into the container's `/home/app` directory  
- The source path is relative to the `Dockerfile` location  

---

**CMD ["node", "/home/app/server.js"]**  
- This is the container's default command  
- Runs your application using Node.js  
- Acts as the **entry point**, unless overridden by `docker run`  

---

## ✅ Best Practices

- Prefer using **multi-stage builds** for production to reduce image size (e.g., separate build and runtime stages)  
- Keep sensitive data like passwords and tokens **out of Dockerfiles**  
- Use `.dockerignore` to exclude unnecessary files from the build context  
- **Mount volumes** during local development instead of copying code into each image build  

---

📝 *This Dockerfile is often paired with a `docker-compose.yml` that runs MongoDB and Mongo Express alongside the Node.js app — all connected through a shared Docker network.*

---

## 🧰 Additional Docker Commands

This section lists useful Docker commands for monitoring, cleaning, networking, and container linking.

---

### 🔍 Monitoring & System Info

1. `docker events`  
   - Stream real-time events from the Docker daemon

2. `docker top <container>`  
   - Shows the top processes running inside a specific container

3. `docker stats <container>`  
   - Displays real-time usage statistics (CPU, memory, network I/O, etc.) for the container

4. `docker system df`  
   - Summarizes disk usage by Docker (images, containers, volumes, and build cache)  
   - Helps identify what’s taking up space

5. `docker system events`  
   - Displays system-wide events from the Docker daemon (like container start, stop, die)

6. `docker system prune -a`  
   - Removes:
     - All stopped containers  
     - All unused images (not referenced by any container)  
     - All unused networks  
     - All build cache  
   - ⚠️ Use with caution — this is a cleanup command

7. `docker system info`  
   - Displays detailed information about the Docker installation (OS, drivers, resources, etc.)

---

### 🌐 Docker Networking

8. `docker network ls`  
   - Lists all Docker networks (bridge, host, none, and custom networks)

9. `docker network inspect bridge`  
   - Provides configuration and connected containers for the default `bridge` network

10. `docker network create mongo-network`  
    - Creates a custom Docker network  
    - Useful for microservices that need to communicate with each other by name

**Example (launching MongoDB on a custom network):**  
`docker run -p 27017:27017 -d -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --name mongodb --net mongo-network mongo`

---

### 🔗 Linking Containers (Legacy Method)

> ⚠️ `--link` is deprecated and **not recommended** for modern projects. Prefer using custom networks and DNS-based service discovery.

11. Establishing links between microservices:

- Using `--link`:  
  `docker run -d -p 8100:8100 --name=currency-conversion --link currency-exchange in28min/currency-conversion:0.0.1-RELEASE`

- Setting environment variable explicitly:  
  `docker run -d -p 8100:8100 --env CURRENCY_EXCHANGE_SERVICE_HOST=http://currency-exchange --name=currency-conversion --link currency-exchange in28min/currency-conversion:0.0.1-RELEASE`
---

📝 _These commands are part of the daily toolbox for anyone exploring Docker in DevOps. Bookmark or store them in your GitHub repo for quick access._
