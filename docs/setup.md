# Project Setup Guide

This guide provides a step-by-step walkthrough for configuring Docker volumes on an AWS EC2 instance. It covers creating persistent storage, attaching volumes to containers, validating data persistence, and performing cleanup.

---

# Table of Contents

- Solution Architecture
- Prerequisites
- AWS Infrastructure
- Launch EC2 Instance
- Connect to EC2
- Install Docker
- Verify Installation
- Create Docker Volume
- Inspect Docker Volume
- Run Container with Volume
- Create Test Data
- Verify Persistent Storage
- Demonstrate Bind Mounts
- Cleanup
- Deployment Checklist
- Summary

---

# Solution Architecture

```text
Developer
     │
     ▼
GitHub Repository
     │
     ▼
AWS EC2 Instance
     │
     ▼
Ubuntu Linux
     │
     ▼
Docker Engine
     │
     ▼
Docker Volume
     │
     ▼
Running Container
     │
     ▼
Persistent Application Data
```

The important concept demonstrated throughout this guide is that **Docker volumes remain available even after the associated container has been removed.**

---

# Prerequisites

Before beginning, ensure the following are available.

- AWS Account
- Ubuntu EC2 Instance
- SSH Client
- Docker Engine
- Git
- Internet Connection

---

# AWS Infrastructure

The project was completed using the following resources.

| Component | Description |
|-----------|-------------|
| Cloud Provider | Amazon Web Services |
| Compute | Amazon EC2 |
| Operating System | Ubuntu Linux |
| Container Runtime | Docker Engine |
| Storage | Docker Volumes |
| Version Control | Git |
| Repository | GitHub |

---

# Step 1 — Launch an EC2 Instance

Create an Ubuntu EC2 instance.

Recommended configuration:

- Ubuntu Server LTS
- Public IP Enabled
- SSH Enabled
- Security Group allowing SSH

---

# Step 2 — Connect to the Server

Connect using SSH.

```bash
ssh -i your-key.pem ubuntu@<EC2-Public-IP>
```

Verify connectivity before continuing.

---

# Step 3 — Update Ubuntu

```bash
sudo apt update

sudo apt upgrade -y
```

Keeping packages updated improves compatibility and security.

---

# Step 4 — Verify Docker Installation

Confirm Docker is installed.

```bash
docker --version
```

Display system information.

```bash
docker info
```

Ensure the Docker daemon is running correctly.

---

# Step 5 — Create a Docker Volume

Create a named volume.

```bash
docker volume create project-data
```

Docker creates and manages the storage location automatically.

---

## Screenshot Placeholder

```text
images/volume-create.png
```

---

# Step 6 — Verify the Volume

List available volumes.

```bash
docker volume ls
```

Inspect the new volume.

```bash
docker volume inspect project-data
```

Review:

- Volume name
- Driver
- Mount point
- Scope

---

## Screenshot Placeholder

```text
images/volume-list.png
```

```text
images/volume-inspect.png
```

---

# Step 7 — Run a Container with the Volume

Create an Nginx container using the volume.

```bash
docker run -d \
--name nginx-volume \
-v project-data:/usr/share/nginx/html \
nginx
```

Docker mounts the volume inside the container.

---

## Screenshot Placeholder

```text
images/container-with-volume.png
```

---

# Step 8 — Create Test Data

Access the container.

```bash
docker exec -it nginx-volume bash
```

Create a test file.

```bash
echo "Docker Volume Test" > /usr/share/nginx/html/index.html
```

Exit the container.

```bash
exit
```

---

## Screenshot Placeholder

```text
images/data-created.png
```

---

# Step 9 — Remove the Container

Delete the container.

```bash
docker rm -f nginx-volume
```

Notice that only the container is removed.

The Docker volume remains available.

---

## Screenshot Placeholder

```text
images/container-removed.png
```

---

# Step 10 — Verify Persistent Storage

Create another container using the same volume.

```bash
docker run -d \
--name nginx-volume-new \
-v project-data:/usr/share/nginx/html \
nginx
```

Access the new container.

```bash
docker exec -it nginx-volume-new bash
```

Verify the file.

```bash
cat /usr/share/nginx/html/index.html
```

The original data should still exist.

---

## Screenshot Placeholder

```text
images/data-persisted.png
```

---

# Step 11 — Demonstrate a Bind Mount

Run another container using a bind mount.

```bash
docker run -d \
--name bind-demo \
-v /home/ubuntu/project:/usr/share/nginx/html \
nginx
```

This maps an existing host directory directly into the container.

---

## Screenshot Placeholder

```text
images/bind-mount.png
```

---

# Step 12 — Cleanup

Remove containers.

```bash
docker rm -f nginx-volume-new

docker rm -f bind-demo
```

Remove the Docker volume.

```bash
docker volume rm project-data
```

Remove unused volumes.

```bash
docker volume prune
```

Display Docker storage usage.

```bash
docker system df
```

---

## Screenshot Placeholder

```text
images/cleanup.png
```

---

# Deployment Verification Checklist

Verify the following before considering the exercise complete.

- Ubuntu EC2 instance running
- Docker Engine operational
- Docker volume created
- Volume successfully mounted
- Test data written
- Container removed
- New container created
- Original data still available
- Bind mount demonstrated
- Cleanup completed successfully

---

# Summary

This project demonstrates how Docker volumes provide persistent storage that is independent of the container lifecycle. By creating a volume, mounting it into a container, writing data, removing the container, and reusing the same volume with a new container, the persistence of application data is clearly verified.

Understanding Docker volumes is essential for running stateful applications such as databases, content management systems, and file-based services in containerized environments. This project establishes the practical foundation for more advanced storage concepts used in Docker Compose, Kubernetes Persistent Volumes, and cloud-native infrastructure.
```
