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

📝 _These commands are part of the daily toolbox for anyone exploring Docker in DevOps. Bookmark or store them in your GitHub repo for quick access._
