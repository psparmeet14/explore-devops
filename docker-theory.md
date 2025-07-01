# 📘 Docker Concepts (Theory)

This file provides a theoretical overview of Docker: how it works, its components, and key differences from other virtualization technologies like VMs.

---

## 1. What is Docker?

- Docker is a software platform that allows you to build, test, and deploy applications in a **containerized environment**.
- It is a form of **lightweight virtualization**.
- Packages the application with all its **dependencies**, **system tools**, **libraries**, and **configuration** into a single unit called an **image**.
- Enables **portability**: Docker artifacts can run consistently across any system that has Docker installed.

---

## 2. Development Process with Containers

- Start services as containers using the **same Docker command across all OSes**.
- Commands are **standardized** for different services.
- Allows **multiple versions** of the same app to run in isolation.
- Provides consistency in local development across teams.

---

## 3. Deployment Process with Containers

- Docker packages:  
  `[app source code + configuration + dependencies] => Docker Image`
- Docker images are **self-contained artifacts** with no external dependencies.
- Server configuration is minimal — just Docker runtime is needed.
- Operations team:
  - Installs Docker runtime
  - Runs Docker commands to fetch and start containers

---

## 4. Docker vs Virtual Machines (VMs)

### System Architecture Layers:
- **OS Applications Layer** (e.g., Chrome, Word)
- **OS Kernel** – The core that communicates with hardware
- **Hardware** (CPU, Memory, Disk, etc.)

### Docker:
- Virtualizes only the **application layer**.
- Reuses the **host kernel** (does not boot its own).
- Images are lightweight (MBs), containers start in seconds.
- Originally built for Linux, but **Docker Desktop** enables usage on Windows/Mac using a lightweight Linux VM.
- Uses Linux-based images (most containers use Linux kernel).

### Virtual Machines:
- Virtualize both **application + kernel layers**.
- Boots its own OS kernel (does not reuse host kernel).
- Images are large (GBs), VMs are slower to boot.
- Can run **any OS on any host OS**.

---

## 5. Docker Desktop

Includes:
- **Docker Engine**: the Docker server daemon (`dockerd`)
- **Docker CLI**: to run commands from terminal
- **GUI Dashboard**: manage containers and images graphically

---

## 6. Docker Registry

- Hosted at: [https://hub.docker.com](https://hub.docker.com)
- A **storage and distribution system** for Docker images.
- Provides:
  - Official images (e.g., Redis, Mongo, Postgres)
  - Community-maintained images
  - Public or private repositories
- Default Registry → `hub.docker.com`  
  Repository → `in28min/todo-rest-api-h2`  
  Tag → `1.0.0.RELEASE`
- Image reference: `hub.docker.com/r/in28min/todo-rest-api-h2`

### Registry vs Repository

- **Registry**:
  - Hosts multiple repositories
  - Can be public (Docker Hub) or private (AWS ECR, GCR, Azure, Nexus)

- **Repository**:
  - Stores related images with different **tags/versions**
  - Tags identify image versions (e.g., `1.0`, `latest`)
  - Best practice: **use specific tags**, avoid relying on `latest`

---

## 7. Docker Image

- A **read-only**, **immutable template** to create containers.
- An executable application artifact.
- Static version (Static template) - A set of bytes
- Contains:
  - Application code (e.g., JAR, binary)
  - Dependencies and runtime (e.g., JRE, Node)
  - OS Layer (usually Linux)
  - Environment variables and config files
- Like a **Class** in OOP (blueprint).

---

## 8. Docker Container

- A **running instance** of a Docker image.
- Like an **Object** in OOP (instantiated from a class).
- You can run **multiple containers** from the same image.
- Each container is isolated from others and from the host.

---

## 9. Dockerfile

- A **text file** containing instructions to build a Docker image.
- Used to automate image creation for application deployments.
- Custom images are created by defining:
  - Base image
  - Dependencies
  - Files to copy
  - Commands to run
- After the development, we want to deploy our application as a Docker Container. We need to create a "definition" of how to build an image from our application, and that definition is written in a file called "dockerfile"


### Common Dockerfile Directives

- `FROM` – Base image to build upon (required)
- `RUN` – Execute shell commands inside the container
- `COPY` – Copy files from host to container
- `WORKDIR` – Set the working directory inside the image
- `CMD` – Command to run when the container starts (entry point)

> Dockerfiles build on **layered images**, enabling caching and reusability for faster builds.

---

📝 *Docker simplifies both the development and deployment lifecycle, offering consistent, reproducible, and isolated environments across teams and platforms.*

---

## 📦 Dockerfile Example for Node.js Applications

> What does a Node.js app need? It needs **Node** and **npm** installed.

### Sample Dockerfile

FROM node:19-alpine  
- Ensures that Node.js and npm are available inside the container  
- Alpine is a lightweight Linux distribution for smaller image size

COPY package.json /app/  
COPY src /app/  
- Copies application files from host into the container

WORKDIR /app  
- Changes the working directory to `/app` inside the container

RUN npm install  
- Installs dependencies and creates `node_modules` inside the container

CMD ["node", "server.js"]  
- Starts the application  
- This is the final command run when the container starts

### What the Container Includes:
1. Linux Operating System  
2. Node.js and npm installed  
3. Application files copied from host  
4. Executed `npm install` to prepare the app environment

---

## 🌍 Docker Popularity

1. **Standardized Application Packaging**  
   - Same method for Java, Python, JS, etc.

2. **Multi-Platform Support**  
   - Works on local machines, data centers, and all major clouds (AWS, Azure, GCP)

3. **Light-Weight & Isolated**  
   - Containers are more efficient and faster than traditional VMs  
   - Each container runs in complete isolation

---

## 🛠️ Steps to Create a Docker Image for a Project

1. **Pre-requisite**: A valid `Dockerfile` must be present in the project root.
2. Open terminal and navigate to the root folder of your project:  
   `cd path/to/your/project`
3. Build the image using:  
   `docker build -t in28min/hello-world-python:0.0.2.RELEASE .`  
   - `-t` or `--tag` assigns a name and optional tag in the format `name:tag`  
   - `.` at the end includes all files in the build context

4. General form:  
   `docker build -t <image-name>:<tag> .`  
   - Builds an image from the current directory containing the Dockerfile

5. 🔍 **Remember**: Docker images are made up of **layers**
   - Each Dockerfile instruction creates a new layer
   - Layers are stacked and cached to improve build performance

6. Check the created image using:  
   `docker images`

---

## 🚀 Steps to Push Docker Image to Docker Hub

1. Login to Docker Hub:  
   `docker login`

2. Build the image using your Docker Hub username/repo name:  
   `docker build -t <your-docker-id>/<repo-name>:<tag> .`

3. Push the image to Docker Hub:  
   `docker push in28min/hello-world-python:0.0.2.RELEASE`

---

## 📦 Dockerfile Concept: ENTRYPOINT vs CMD

- **CMD**  
  - Default command to run when a container starts  
  - Can be **overridden** at runtime using command-line arguments

  Example:  
  `docker run -d -p 5001:5000 in28min/hello-world-nodejs:0.0.3.RELEASE ping google.com`  
  → CMD is overridden by `ping google.com`

- **ENTRYPOINT**  
  - Hard-coded executable that always runs unless explicitly overridden  
  - Preferred when the container must always run the same app

  Overriding ENTRYPOINT:  
  `docker run -d -p 5000:5000 --entrypoint <entrypoint> in28min/hello-world-java:0.0.2.RELEASE`

---

## 🧪 Dockerize Your Own Application

1. Write a `Dockerfile`
2. Build the Docker image:  
   `docker build -t your-app-name .`
3. Run as a container:  
   `docker run -d -p <host-port>:<container-port> your-app-name`

---

## ⚙️ Docker Architecture

1. **Docker Client**  
   - CLI tool that sends commands to the Docker daemon

2. **Docker Daemon (Engine)**  
   - Core service that:
     - Manages containers and images
     - Pulls images from registries
     - Builds new images
     - Pushes images to registries
