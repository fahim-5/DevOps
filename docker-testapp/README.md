<p align="center">
  <strong>Fahim Faysal</strong><br>
  <sub>2025-08-09</sub>
</p>


## Docker

- Introduction to Docker  
- Docker Installation  
- Docker Commands  
- Docker Image Layers | Layers in Docker  
- Port Binding  
- Troubleshoot Commands | Logs, etc.  
- Docker vs Virtual Machine | Difference between them  
- Developing with Docker  
- Docker Compose  
- Dockerizing our application  
- Publishing Images to Docker Hub  
- Docker Volume


---

## **1. Introduction to Docker**

* **What is Docker?**
  Docker is a tool that lets you run applications in *containers*.
  A container is like a lightweight, portable box that includes your application and everything it needs to run (code, libraries, dependencies).
* **Why use it?**

  * Works the same everywhere (no "it works on my machine" problem).
  * Fast and lightweight compared to virtual machines.
  * Easy to share and deploy.
* **Example in real life:**
  Think of a shipping container: it can carry goods anywhere in the world without worrying about the ship, truck, or train—Docker containers carry software the same way.

---

## **2. Docker Installation**

The process depends on your operating system:

**For Windows & macOS:**

1. Download Docker Desktop from the official site: [https://www.docker.com/get-started](https://www.docker.com/get-started)
2. Install it like any normal application.
3. Run it and make sure Docker is running in the background.
4. Check if Docker is installed:

```bash
docker --version
```

<!-- ---
![Alt text](into.png) -->

---

## **3. Docker Commands**

Here are the most common ones for beginners:

| Command                      | Purpose                                 | Example              |
| ---------------------------- | --------------------------------------- | -------------------- |
| `docker --version`           | Check Docker version                    | `docker --version`   |
| `docker pull <image>`        | Download an image from Docker Hub       | `docker pull nginx`  |
| `docker images`              | List downloaded images                  | `docker images`      |
| `docker run <image>`         | Create and start a container            | `docker run nginx`   |
| `docker ps`                  | List running containers                 | `docker ps`          |
| `docker ps -a`               | List all containers (including stopped) | `docker ps -a`       |
| `docker stop <container_id>` | Stop a running container                | `docker stop abc123` |
| `docker rm <container_id>`   | Remove a stopped container              | `docker rm abc123`   |
| `docker rmi <image_id>`      | Remove an image                         | `docker rmi xyz456`  |

---


## **Docker Port Binding (Quick Notes)**

* **What:** Connects a port on your **host** to a port in the **container** so you can access the app from outside.
* **Syntax:**

  ```bash
  docker run -p HOST_PORT:CONTAINER_PORT image
  ```
* **Example:**

  ```bash
  docker run -p 8080:80 nginx
  # Host:8080 → Container:80
  ```
* **Why:** Without it, the app inside the container isn’t reachable from your machine/network.
* **Tip:** Only expose needed ports for security.


---
# Troubleshooting

### What is Troubleshooting in Docker?

It’s the **process of diagnosing and fixing issues** when containers don’t behave as expected — like crashes, errors, network problems, or app failures.

---

### Why do you need to troubleshoot?

* Containers can fail to start or crash unexpectedly.
* Network or volume issues can block data flow.
* Misconfigured environment variables can break the app.
* Debugging helps you **pinpoint root causes** quickly so you can fix and keep things running smoothly.

---

### Key Docker Troubleshooting Commands:

* **`docker ps`**: Lists all running containers — check if your container is actually running.
* **`docker logs <container_name>`**: Shows logs from a container’s output — like the console output of your app, helps find errors or warnings.
* **`docker exec -it <container_name> sh/bash`**: Opens a terminal inside the running container — super useful to poke around, inspect files, or run commands inside.
* **`docker inspect <container_name>`**: Gives detailed info about the container’s config and state — network settings, mounts, env vars, and more.
* **`docker restart <container_name>`**: Restarts a container if it crashes or freezes.
* **`docker rm <container_name>` & `docker rmi <image_name>`**: Remove containers or images to clear space or fix corrupt setups.


---

# Docker vs Virtual Machine 

## **1. Core Concept**

* **VM:** Runs a *full* operating system (OS) on top of your hardware, through a hypervisor.
* **Docker:** Runs applications inside isolated *containers* using the host OS kernel.

---

## **2. Structure**

**Virtual Machine Layers:**

```
Hardware → Host OS → Hypervisor → Guest OS → App
```

**Docker Layers:**

```
Hardware → Host OS → Docker Engine → Container → App
```

---

## **3. Key Differences Table**

| Feature            | Virtual Machine                                              | Docker (Containers)                                                   |
| ------------------ | ------------------------------------------------------------ | --------------------------------------------------------------------- |
| **OS requirement** | Needs a full Guest OS for each VM                            | Shares host OS kernel (no full OS inside)                             |
| **Size**           | Large (GBs, because OS + app)                                | Small (MBs, just app + dependencies)                                  |
| **Startup Time**   | Slow (minutes)                                               | Fast (seconds)                                                        |
| **Performance**    | More overhead (OS emulation)                                 | Near native performance                                               |
| **Isolation**      | Strong (separate OS, hardware-level isolation)               | Process-level isolation (less heavy, but secure enough for most uses) |
| **Portability**    | VM image works if hypervisor supports it                     | Container works anywhere Docker runs                                  |
| **Use Cases**      | Running multiple different OSes, legacy apps, full isolation | Fast deployment, microservices, DevOps pipelines                      |

---

## **4. Real-Life Analogy**

* **VM** → Like renting separate apartments for each person (each apartment has its own kitchen, bathroom, etc.).
* **Docker** → Like roommates sharing the same apartment but with their own locked bedrooms.

---




#      Developing with Docker: Setting Up a MERN Stack App

To run a MERN stack application using Docker, we need two Docker images:

1. **mongo**
2. **mongo-Express**

Before starting, it’s important to understand **Docker Networking**.

### What is a Docker Network?

A **Docker Network** is a virtual network that allows Docker containers to communicate securely and efficiently within isolated environments.

---

### **Step 1: Create a Docker Network**

Create a network named `mongo-network` (if not already created):

```bash
docker network create mongo-network
```

---

### **Step 2: Run the MongoDB Container**

Run the MongoDB container with the following command:

```bash
docker run -d \
  -p 27017:27017 \
  --name mongo \
  --network mongo-network \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=qwerty \
  mongo
```

---

### **Step 3: Run the Mongo-Express Container**

Run the Mongo-Express container and connect it to the MongoDB container:

```bash
docker run -d \
  -p 8081:8081 \
  --name mongo-express \
  --network mongo-network \
  -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
  -e ME_CONFIG_MONGODB_ADMINPASSWORD=qwerty \
  -e ME_CONFIG_MONGODB_URL="mongodb://admin:qwerty@mongo:27017" \
  mongo-express
```

**Note:**
The `ME_CONFIG_MONGODB_URL` variable tells Mongo-Express how to connect to MongoDB using the admin credentials.

---

### **Step 4: Verify Running Containers**

Check if both containers are running:

```bash
docker ps
```

---

### **Step 5: Access the Mongo-Express Interface**

Open your browser and visit:

```
http://localhost:8081
```

Login with:

* **Username:** `admin`
* **Password:** `pass`

Once logged in, you can visually manage your MongoDB data through the Mongo-Express web interface.



# **Docker Compose**

A tool that lets you **define and run multi-container Docker applications** using a single configuration file (`docker-compose.yml`).
Instead of running multiple `docker run` commands, you declare everything once and start all services together.

---

## **YAML File**

* **YAML** (YAML Ain’t Markup Language) is a human-readable format used to define settings in Docker Compose.
* Organizes configuration in **key-value pairs** using indentation (no brackets or commas).

---

## **How to Run**

* **Default file name (`docker-compose.yml`)** in current or root directory:

  ```bash
  docker compose up -d
  ```
* **Custom file name** (e.g., `mongo-setup.yaml`):

  ```bash
  docker compose -f mongo-setup.yaml up -d
  ```

**Flags:**

* `up` → Creates and starts all containers/services from the YAML.
* `-d` → Runs containers in **detached mode** (background).

---

## **How to Delete All Containers Together**

* **Stop and remove containers** (with networks and volumes defined in YAML):

  ```bash
  docker compose down
  ```
* **If custom YAML file**:

  ```bash
  docker compose -f mongo-setup.yaml down
  ```
* **Remove volumes too** (wipes data):

  ```bash
  docker compose down -v
  ```

---

## **Why No Need to Create Network**

* Docker Compose **automatically creates a default bridge network** for all services.
* Containers can talk to each other by **service name** (e.g., `mongo`).

---

## **Benefits**

1. **Simplicity** – One command to manage multiple containers.
2. **Automatic Networking** – No manual network setup.
3. **Reproducibility** – Works the same on any machine.
4. **Environment Management** – Set usernames, passwords, ports easily.

---

## **Example (`docker-compose.yml`)**

```yaml
version: '3.8'

services:
  mongo:
    image: mongo
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: qwerty
    
  mongo-express:
    image: mongo-express
    ports:
      - "8081:8081"
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: admin
      ME_CONFIG_MONGODB_ADMINPASSWORD: qwerty
      ME_CONFIG_MONGODB_URL: mongodb://admin:qwerty@mongo:27017/
```

---



Alright Fahim, let’s break this down step-by-step so it’s crystal clear — from the *definition* of **Dockerizing an app**, to **common Dockerfile commands**, and then the **full process** to actually do it.

---

# **1️⃣ Dockerizing an App**


## **1️⃣ Definition of Dockerizing an App**

**Dockerizing** an app means packaging your application **code + dependencies + environment configuration** into a **Docker image** so it can run anywhere without worrying about OS or setup differences.
It’s like putting your app in a sealed shipping container — it works the same whether it’s on your laptop, a server, or the cloud.

---

## **2️⃣ Common Dockerfile Commands (with explanations)**

A **Dockerfile** is a text file with step-by-step instructions for building your image.

| Command        | What it Does                                                                          |
| -------------- | ------------------------------------------------------------------------------------- |
| **FROM**       | Specifies the base image (e.g., `FROM node:18-alpine`).                               |
| **WORKDIR**    | Sets the working directory inside the container.                                      |
| **COPY**       | Copies files from your local machine to the container.                                |
| **ADD**        | Like `COPY` but can also fetch from URLs and extract archives.                        |
| **RUN**        | Runs commands in the container at build time (e.g., installing dependencies).         |
| **CMD**        | Default command to run when a container starts (only one allowed, can be overridden). |
| **ENTRYPOINT** | Like `CMD` but harder to override — best for fixed executables.                       |
| **EXPOSE**     | Declares the port the container will listen on.                                       |
| **ENV**        | Sets environment variables.                                                           |
| **VOLUME**     | Creates a mount point for persistent data.                                            |

---


### **Step 2: Create a Dockerfile**

Example for a **Node.js app**:

```dockerfile
FROM node
ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=qwerty
RUN mkdir -p testapp
COPY . /testapp
RUN npm install 
CMD ["node", "/testapp/server.js"]
```

---

### **Step 3: Build the Docker Image**

```bash
docker build -t my-app .
```

---

### **Step 4: Run the Container**

```bash
docker run -p 3000:3000 my-app
```

* `-p 3000:3000` maps container’s port 3000 to your local machine’s port 3000.

---

### **Step 5: Verify**

Visit `http://localhost:3000` and see your app running inside Docker.

---

## **4️⃣ Bonus Pro Tips**

* Always use **lightweight base images** like `node:18-alpine` or `python:3.12-slim` for faster builds.
* Use a `.dockerignore` file to avoid copying unnecessary files into the image.
* For faster builds, copy `package.json` & install dependencies *before* copying the rest of your source code (Docker layer caching).
* Use **multi-stage builds** to keep production images small.
* Keep environment-specific configs in `.env` files and load them at runtime.

---

# **Push images in the Docker Hub**

> To push your image to Docker Hub, first log in using `docker login` in the terminal and enter your credentials. Then log in to Docker Hub in your browser and create a repository. Finally, tag your local image with your Docker Hub username/repo name and push it from the terminal using `docker push`.

```bash
# 1. Log in to Docker Hub
docker login
# 3. Push the image
docker push <your_dockerhub_username>/<repo_name>:latest
```
