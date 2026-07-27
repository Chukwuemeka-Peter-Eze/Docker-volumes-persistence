# Docker Volumes Command Reference

This document contains the Docker commands used throughout this project, along with a brief explanation of what each command does and when it is commonly used.

---

# Create a Docker Volume

```bash
docker volume create my-volume
```

Creates a new named Docker volume managed by Docker.

**When to use:**

- Before attaching persistent storage to a container.
- When an application requires data to survive container removal.

---

# List Docker Volumes

```bash
docker volume ls
```

Displays all Docker volumes available on the local machine.

**Useful for:**

- Verifying that a volume was created successfully.
- Viewing existing Docker-managed storage.

---

# Inspect a Docker Volume

```bash
docker volume inspect my-volume
```

Displays detailed information about a Docker volume.

The output includes:

- Volume name
- Driver
- Mount point
- Labels
- Scope

**Useful for:**

- Troubleshooting
- Locating where Docker stores the volume
- Verifying configuration

---

# Remove a Docker Volume

```bash
docker volume rm my-volume
```

Deletes a Docker volume.

> A volume cannot be removed while it is still being used by a container.

---

# Remove All Unused Volumes

```bash
docker volume prune
```

Deletes every unused Docker volume.

**Use with caution.**

Once removed, the stored data cannot be recovered unless it has been backed up.

---

# Run a Container with a Docker Volume

```bash
docker run -d \
--name demo-container \
-v my-volume:/data \
nginx
```

Starts a new container and mounts the Docker volume at `/data`.

### Breakdown

- `-d` → Run in detached mode
- `--name` → Assign a container name
- `-v` → Mount the Docker volume
- `nginx` → Container image

---

# Access a Running Container

```bash
docker exec -it demo-container sh
```

Starts an interactive shell inside the running container.

**Useful for:**

- Creating files
- Viewing data
- Troubleshooting

---

# Create a File Inside the Mounted Volume

```bash
echo "Docker Volumes Persist Data" > /data/example.txt
```

Writes a file into the mounted Docker volume.

Because the file is stored in the volume, it remains available even if the container is deleted.

---

# View the File

```bash
cat /data/example.txt
```

Displays the contents of the file.

Useful for confirming that the data was written successfully.

---

# List Files Inside the Mounted Directory

```bash
ls /data
```

Displays all files stored inside the mounted volume.

Useful for verifying data persistence after recreating a container.

---

# Remove a Container

```bash
docker rm -f demo-container
```

Stops and removes the specified container.

The Docker volume is **not** removed.

---

# Create a New Container Using the Same Volume

```bash
docker run -it \
--name verification-container \
-v my-volume:/data \
ubuntu bash
```

Creates a new container and mounts the previously created Docker volume.

This demonstrates that the stored data survives container replacement.

---

# Inspect a Container

```bash
docker inspect demo-container
```

Displays detailed configuration information about a container.

The **Mounts** section shows:

- Mounted volumes
- Mount type
- Source
- Destination
- Read/write status

Useful for verifying that the correct volume has been attached.

---

# Display Docker Disk Usage

```bash
docker system df
```

Shows Docker disk usage for:

- Images
- Containers
- Local Volumes
- Build Cache

Useful for identifying unused resources and reclaiming storage.

---

# List Running Containers

```bash
docker ps
```

Displays all currently running containers.

---

# List All Containers

```bash
docker ps -a
```

Displays both running and stopped containers.

Useful when troubleshooting or locating containers that still reference a Docker volume.

---

# Demonstrate a Bind Mount

```bash
docker run -d \
--name bind-demo \
-v /path/to/local/folder:/app \
nginx
```

Maps an existing directory from the host machine into the container.

Unlike Docker volumes, the host directory is managed outside of Docker.

---

# Summary

The commands in this project demonstrate the complete lifecycle of working with Docker volumes:

1. Create a volume.
2. Attach it to a container.
3. Store data.
4. Remove the container.
5. Recreate the container.
6. Verify that the data persists.
7. Manage and inspect Docker storage.