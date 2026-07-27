# Project Setup Guide

This guide explains how to reproduce the Docker Volumes project on a local development machine.

By the end of this guide, you will have:

- Docker installed and running
- A Docker volume created
- A container using the volume
- Persistent data stored in the volume
- Verified that the data survives container recreation

---

# Prerequisites

Ensure the following software is installed before starting.

| Requirement | Purpose |
|------------|---------|
| Docker Desktop (or Docker Engine) | Run containers |
| Git | Clone the repository |
| Terminal | Execute Docker commands |
| Code Editor (Optional) | View and edit project files |

Verify Docker is installed.

```bash
docker --version
```

Example output:

```text
Docker version 28.x.x
```

Verify Docker is running.

```bash
docker info
```

If Docker is running successfully, system information will be displayed.

---

# Clone the Repository

Clone the project from GitHub.

```bash
git clone https://github.com/your-username/docker-volumes-persistence.git
```

Navigate into the project directory.

```bash
cd docker-volumes-persistence
```

---

# Step 1 — Create a Docker Volume

Create a named volume.

```bash
docker volume create my-volume
```

Verify that the volume exists.

```bash
docker volume ls
```

Expected output:

```text
DRIVER    VOLUME NAME
local     my-volume
```

---

# Step 2 — Start a Container

Run an Nginx container with the Docker volume mounted.

```bash
docker run -d \
--name demo-container \
-v my-volume:/data \
nginx
```

Verify that the container is running.

```bash
docker ps
```

---

# Step 3 — Create Persistent Data

Open a shell inside the container.

```bash
docker exec -it demo-container sh
```

Create a file inside the mounted directory.

```bash
echo "Docker Volumes Persist Data" > /data/example.txt
```

Verify the file.

```bash
cat /data/example.txt
```

Exit the container.

```bash
exit
```

---

# Step 4 — Remove the Container

Remove the container.

```bash
docker rm -f demo-container
```

The container is deleted, but the Docker volume still exists.

Verify the volume.

```bash
docker volume ls
```

---

# Step 5 — Verify Persistent Storage

Create a new container using the same volume.

```bash
docker run -it \
--name verification-container \
-v my-volume:/data \
ubuntu bash
```

List the files.

```bash
ls /data
```

Display the contents.

```bash
cat /data/example.txt
```

If the file appears, the volume has successfully preserved the data.

Exit the container.

```bash
exit
```

---

# Step 6 — Inspect the Volume

View detailed information about the Docker volume.

```bash
docker volume inspect my-volume
```

Review the output, including:

- Volume name
- Driver
- Mount point
- Scope

---

# Step 7 — Demonstrate a Bind Mount

Create a bind mount using a directory on your local machine.

Replace `/path/to/local/folder` with a directory that exists on your computer.

```bash
docker run -d \
--name bind-demo \
-v /path/to/local/folder:/app \
nginx
```

Verify that changes made in the host directory are visible inside the container.

---

# Cleanup

Remove the containers.

```bash
docker rm -f verification-container
```

```bash
docker rm -f bind-demo
```

Remove the Docker volume.

```bash
docker volume rm my-volume
```

Remove any unused volumes.

```bash
docker volume prune
```

---

# Verification Checklist

Confirm that you have successfully completed the project.

- Docker installed and running
- Docker volume created
- Container started successfully
- Volume mounted correctly
- File created in the mounted directory
- Container removed
- New container created
- Original file still exists
- Volume inspected
- Bind mount tested

---

# Next Steps

After completing this project, consider exploring the following topics:

- Docker Compose volumes
- Volume backup and restore
- Named versus anonymous volumes
- Volume drivers
- Multi-container applications with shared storage
- Kubernetes Persistent Volumes (PV)
- Kubernetes Persistent Volume Claims (PVC)