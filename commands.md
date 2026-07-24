# Docker Volumes Commands Reference

This document contains the Docker volume commands used throughout this project, along with explanations of their purpose and common use cases.

---

# Table of Contents

- Verify Docker Installation
- Create Docker Volumes
- List Volumes
- Inspect Volumes
- Mount Volumes
- Verify Data Persistence
- Remove Volumes
- Bind Mount Commands
- Storage Inspection
- Cleanup Commands
- Command Summary

---

# Verify Docker Installation

Verify Docker is installed correctly.

```bash
docker --version
```

Display Docker system information.

```bash
docker info
```

---

# Create a Docker Volume

Create a named volume.

```bash
docker volume create project-data
```

Purpose

- Creates Docker-managed persistent storage.
- Stores application data outside the container lifecycle.

---

# List Docker Volumes

Display all Docker volumes.

```bash
docker volume ls
```

Useful for:

- Verifying volume creation.
- Viewing available storage resources.

---

# Inspect a Docker Volume

Display detailed information about a volume.

```bash
docker volume inspect project-data
```

Information includes:

- Driver
- Mount point
- Labels
- Scope
- Volume name

---

# Remove a Docker Volume

Remove a specific volume.

```bash
docker volume rm project-data
```

A volume cannot be removed while it is in use by a container.

---

# Remove Unused Volumes

```bash
docker volume prune
```

Deletes all unused Docker volumes.

Use with caution because deleted volume data cannot be recovered without a backup.

---

# Mount a Volume to a Container

Attach a Docker volume during container creation.

```bash
docker run -d \
--name nginx-volume \
-v project-data:/usr/share/nginx/html \
nginx
```

This mounts the `project-data` volume inside the container.

---

# Verify Mounted Volumes

Inspect the container.

```bash
docker inspect nginx-volume
```

Review the **Mounts** section.

Verify:

- Volume name
- Mount path
- Mount type
- Read/write permissions

---

# Verify Data Persistence

Create a file.

```bash
echo "Persistent Storage" > /usr/share/nginx/html/index.html
```

Remove the container.

```bash
docker rm -f nginx-volume
```

Create another container using the same volume.

```bash
docker run -d \
--name nginx-volume-new \
-v project-data:/usr/share/nginx/html \
nginx
```

Verify that the file still exists.

---

# List Container Mounts

```bash
docker inspect nginx-volume-new
```

Useful for confirming:

- Mounted volume
- Destination path
- Driver
- Access mode

---

# Bind Mount Example

Mount a host directory.

```bash
docker run -d \
-v /home/ubuntu/project:/app \
nginx
```

Changes made in the host directory are immediately reflected inside the container.

---

# Display Docker Disk Usage

```bash
docker system df
```

Displays:

- Images
- Containers
- Volumes
- Build cache

Useful for monitoring Docker storage consumption.

---

# View Running Containers

```bash
docker ps
```

Confirms which containers are currently using mounted volumes.

---

# Display Docker Events

```bash
docker events
```

Monitor Docker activity in real time, including volume creation and container lifecycle events.

---

# Remove Stopped Containers

```bash
docker container prune
```

Removes all stopped containers while preserving volumes.

---

# Remove Unused Images

```bash
docker image prune
```

Removes unused images to reclaim storage.

---

# Remove All Unused Docker Resources

```bash
docker system prune
```

Removes:

- Stopped containers
- Unused networks
- Dangling images
- Build cache

Remove all unused images as well.

```bash
docker system prune -a
```

---

# Typical Workflow

```bash
docker volume create project-data

docker volume ls

docker run -d \
-v project-data:/data \
--name demo-container \
nginx

docker inspect demo-container

docker rm -f demo-container

docker run -d \
-v project-data:/data \
--name demo-container-2 \
nginx

docker volume inspect project-data

docker volume prune
```

This demonstrates the complete lifecycle of creating, using, verifying, and cleaning up Docker volumes.

---

# Command Summary

| Command | Purpose |
|----------|---------|
| `docker volume create` | Create a Docker volume |
| `docker volume ls` | List all volumes |
| `docker volume inspect` | Inspect a volume |
| `docker volume rm` | Remove a volume |
| `docker volume prune` | Remove unused volumes |
| `docker inspect` | View mounted volumes |
| `docker system df` | Display Docker storage usage |
| `docker ps` | List running containers |
| `docker events` | Monitor Docker activity |
| `docker container prune` | Remove stopped containers |
| `docker image prune` | Remove unused images |
| `docker system prune` | Clean Docker resources |

---

# Conclusion

Docker volumes provide a reliable mechanism for preserving application data beyond the lifecycle of individual containers. Mastering these commands enables effective management of persistent storage, simplifies troubleshooting, and supports the deployment of stateful applications in containerized environments.
