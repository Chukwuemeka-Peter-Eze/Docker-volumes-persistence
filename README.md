# Docker Volumes for Persistent Data Storage on AWS

<p align="center">

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Docker Volumes](https://img.shields.io/badge/Docker_Volumes-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)

</p>

---

# Project Overview

This project demonstrates how Docker volumes provide persistent storage for containerized applications running on AWS.

One of the fundamental characteristics of containers is that their writable layer is **ephemeral**. When a container is removed, any data stored only inside that container is also removed.

Docker volumes solve this challenge by storing data outside the container's lifecycle. A volume remains available even after the container that used it has been stopped, removed, or recreated.

Throughout this project, I created and managed Docker volumes, attached them to containers, verified data persistence, explored bind mounts, inspected Docker-managed storage, and documented the complete workflow.

The project was completed on an AWS EC2 instance using Ubuntu Linux and Docker Engine.

---

# Project Objectives

The objectives of this project were to:

- Understand Docker's storage architecture.
- Learn why containers require persistent storage.
- Create Docker volumes.
- Mount volumes into containers.
- Verify that data persists after container removal.
- Inspect Docker-managed volumes.
- Compare Docker volumes with bind mounts.
- Learn volume management commands.
- Document the complete engineering workflow.

---

# Why Persistent Storage Matters

Containers are intentionally designed to be lightweight and disposable.

This makes deployments fast and repeatable, but it also introduces a challenge:

> **What happens to application data when a container is deleted?**

Without persistent storage:

- Uploaded files disappear.
- Database records are lost.
- Application logs are removed.
- Configuration changes are discarded.

Persistent storage separates **application data** from the **container itself**, ensuring that important information survives container replacement.

---

# What Are Docker Volumes?

A Docker volume is a Docker-managed storage location that exists independently of any individual container.

Volumes allow multiple containers to share data and ensure that information remains available even after containers are recreated.

Key characteristics include:

- Managed by Docker
- Independent of container lifecycle
- Optimized for performance
- Easy to back up and restore
- Reusable across containers

---

# Docker Volumes vs Bind Mounts

Docker provides two common approaches for persistent storage.

| Feature | Docker Volume | Bind Mount |
|----------|---------------|------------|
| Managed by Docker | ✅ | ❌ |
| Uses Host Directory | ❌ | ✅ |
| Portable | ✅ | Limited |
| Performance | Optimized | Host-dependent |
| Easy Backup | ✅ | Manual |
| Recommended for Production | ✅ | Depends on use case |

---

# Project Architecture

This project demonstrates how data remains available even after containers are removed.

```text
          Developer
               │
               ▼
        AWS EC2 Instance
               │
               ▼
        Docker Engine
               │
               ▼
      Running Container
               │
      Mounted Docker Volume
               │
               ▼
      Persistent Storage
               │
Container Removed & Recreated
               │
               ▼
        Data Still Exists
```

Unlike the container, the Docker volume has its own lifecycle and continues to exist until it is explicitly removed.

---

# Technologies Used

| Technology | Purpose |
|------------|---------|
| Docker Engine | Container Runtime |
| Docker Volumes | Persistent Storage |
| Bind Mounts | Host File System Integration |
| Ubuntu Linux | Operating System |
| AWS EC2 | Cloud Compute Environment |
| Git | Version Control |
| GitHub | Repository Hosting |

---

# Key Skills Demonstrated

This project demonstrates practical experience with:

- Docker volumes
- Persistent storage
- Bind mounts
- Volume lifecycle management
- Container lifecycle management
- Docker storage architecture
- Linux file systems
- AWS EC2
- Docker CLI
- Infrastructure documentation

---

# Architecture Diagram

> Replace the placeholder below after creating your Draw.io architecture diagram.

```text
images/architecture.png
```

<p align="center">

![Docker Volumes Architecture](images/architecture.png)

</p>

---

# High-Level Workflow

```text
Create Docker Volume
        │
        ▼
Run Container
        │
        ▼
Mount Volume
        │
        ▼
Write Data
        │
        ▼
Remove Container
        │
        ▼
Create New Container
        │
        ▼
Mount Same Volume
        │
        ▼
Verify Data Still Exists
```

This workflow demonstrates the separation between **container lifecycle** and **data lifecycle**.

---

# Repository Structure

```text
Docker-volumes-persistence/
│
├── README.md
├── LICENSE
├── .gitignore
│
├── architecture/
│   └── architecture.drawio
│
├── docs/
│   ├── setup.md
│   ├── troubleshooting.md
│   └── lessons-learned.md
│
├── images/
│   ├── architecture.png
│   ├── volume-create.png
│   ├── volume-list.png
│   ├── volume-inspect.png
│   ├── container-with-volume.png
│   ├── data-created.png
│   ├── container-removed.png
│   ├── data-persisted.png
│   ├── bind-mount.png
│   └── cleanup.png
│
├── commands.md
│
└── video-script.md
```

---

# Learning Outcomes

By completing this project, I gained practical experience in:

- Creating Docker volumes.
- Attaching persistent storage to containers.
- Understanding container storage.
- Inspecting Docker-managed storage.
- Managing Docker volumes.
- Using bind mounts.
- Preserving application data across container recreation.
- Deploying and managing containers on AWS.
- Documenting storage architectures and operational workflows.

---

---

# Prerequisites

Before working with Docker volumes, ensure the following requirements are available.

| Requirement | Description |
|-------------|-------------|
| AWS Account | Cloud environment |
| Amazon EC2 Instance | Linux virtual machine |
| Ubuntu Linux | Operating System |
| Docker Engine | Container Runtime |
| Git | Version Control |
| SSH Client | Remote server access |
| Web Browser | Verification and documentation |

---

# Environment Details

The project was completed using the following environment.

| Component | Value |
|-----------|-------|
| Cloud Provider | Amazon Web Services (AWS) |
| Compute Service | Amazon EC2 |
| Operating System | Ubuntu Linux |
| Container Runtime | Docker Engine |
| Storage | Docker Volumes |
| Storage Alternative | Bind Mounts |
| Version Control | Git |
| Repository Hosting | GitHub |

---

# Understanding Docker Storage

Docker containers have their own writable filesystem.

When a container is deleted:

- Files created inside the container are removed.
- Installed packages inside the writable layer disappear.
- Temporary application data is lost.

This behavior is intentional because containers are designed to be **ephemeral**.

Persistent storage solves this limitation.

---

# Docker Storage Architecture

```text
                Docker Engine
                      │
        ┌─────────────┴─────────────┐
        │                           │
        ▼                           ▼
  Running Container          Docker Volume
        │                           │
        │                           │
        └──────────Mount────────────┘
                    │
                    ▼
          Persistent Application Data
```

The key concept is that **the volume exists independently of the container**.

---

# Container Lifecycle vs Volume Lifecycle

One of the most important concepts demonstrated in this project is that containers and volumes have different lifecycles.

```text
Create Container
        │
        ▼
Write Data
        │
        ▼
Delete Container
        │
        ▼
Volume Still Exists
        │
        ▼
Create New Container
        │
        ▼
Reuse Existing Volume
        │
        ▼
Original Data Available
```

This separation enables reliable storage for databases, uploaded files, application assets, and logs.

---

# Creating a Docker Volume

Create a named Docker volume.

```bash
docker volume create my-volume
```

Docker allocates storage and manages the location automatically.

---

## Screenshot Placeholder — Volume Creation

```text
images/volume-create.png
```

<p align="center">

![Docker Volume Creation](images/volume-create.png)

</p>

---

# Listing Docker Volumes

Display all available Docker volumes.

```bash
docker volume ls
```

The output should include:

- Volume name
- Driver
- Status

Listing volumes is useful for verifying that persistent storage has been created successfully.

---

## Screenshot Placeholder — Volume List

```text
images/volume-list.png
```

<p align="center">

![Docker Volume List](images/volume-list.png)

</p>

---

# Inspecting a Volume

Inspect detailed information about a Docker volume.

```bash
docker volume inspect my-volume
```

The inspection output includes:

- Volume name
- Driver
- Mount point
- Labels
- Scope

This information helps verify where Docker stores persistent data on the host system.

---

## Screenshot Placeholder — Volume Inspection

```text
images/volume-inspect.png
```

<p align="center">

![Docker Volume Inspection](images/volume-inspect.png)

</p>

---

# Mounting a Volume to a Container

Attach the volume when starting a container.

Example:

```bash
docker run -d \
-v my-volume:/data \
--name demo-container \
nginx
```

The `-v` option mounts the Docker-managed volume into the container.

Any data written to `/data` is stored in the volume rather than the container's writable layer.

---

## Screenshot Placeholder — Container with Mounted Volume

```text
images/container-with-volume.png
```

<p align="center">

![Container with Docker Volume](images/container-with-volume.png)

</p>

---

# Creating Data

Create a file inside the mounted directory.

Example:

```bash
echo "Docker Volumes Persist Data" > /data/example.txt
```

The file is written to the mounted volume rather than being stored only inside the container.

---

## Screenshot Placeholder — Data Created

```text
images/data-created.png
```

<p align="center">

![Creating Persistent Data](images/data-created.png)

</p>

---

# Removing the Container

Delete the container while leaving the Docker volume intact.

```bash
docker rm -f demo-container
```

The container is removed, but the volume remains because it is managed independently.

---

## Screenshot Placeholder — Container Removed

```text
images/container-removed.png
```

<p align="center">

![Container Removed](images/container-removed.png)

</p>

---

# Verifying Persistent Data

Create a new container and mount the same volume.

```bash
docker run -it \
-v my-volume:/data \
ubuntu bash
```

Verify that the previously created file is still present.

```bash
ls /data

cat /data/example.txt
```

Successfully reading the file confirms that the data persisted beyond the original container's lifecycle.

---

## Screenshot Placeholder — Data Persisted

```text
images/data-persisted.png
```

<p align="center">

![Persistent Data Verification](images/data-persisted.png)

</p>

---

# Working with Bind Mounts

In addition to Docker volumes, Docker also supports bind mounts.

Example:

```bash
docker run -d \
-v /home/ubuntu/project:/app \
nginx
```

Unlike Docker volumes, a bind mount maps a directory from the host filesystem directly into the container.

Common use cases include:

- Local development
- Configuration files
- Source code synchronization
- Log collection

---

## Screenshot Placeholder — Bind Mount

```text
images/bind-mount.png
```

<p align="center">

![Bind Mount Example](images/bind-mount.png)

</p>

---

# Deployment Summary

At this stage, the following objectives have been completed:

- Docker volume created.
- Volume mounted into a container.
- Persistent data written.
- Container removed.
- New container created.
- Original data verified.
- Docker volume inspected.
- Bind mount demonstrated.

This implementation demonstrates how Docker volumes provide reliable, reusable storage that is independent of the lifecycle of individual containers.

---

---

# Understanding Docker Volume Management

Managing Docker volumes effectively is essential for maintaining persistent application data.

Docker provides dedicated commands for creating, inspecting, listing, removing, backing up, and restoring volumes.

Unlike containers, volumes are intended to outlive the applications that use them.

---

# Volume Lifecycle

The lifecycle of a Docker volume is independent of the container lifecycle.

```text
Create Volume
      │
      ▼
Attach to Container
      │
      ▼
Application Writes Data
      │
      ▼
Stop Container
      │
      ▼
Remove Container
      │
      ▼
Volume Still Exists
      │
      ▼
Attach to New Container
      │
      ▼
Existing Data Available
```

This separation is one of Docker's most important storage features.

---

# Managing Docker Volumes

## Create a Volume

```bash
docker volume create project-data
```

Creates a Docker-managed storage location.

---

## List Volumes

```bash
docker volume ls
```

Displays all available Docker volumes.

---

## Inspect a Volume

```bash
docker volume inspect project-data
```

Displays:

- Driver
- Mount point
- Labels
- Scope
- Volume name

---

## Remove a Volume

```bash
docker volume rm project-data
```

A volume can only be removed if it is no longer being used by any container.

---

## Remove Unused Volumes

```bash
docker volume prune
```

Removes all unused Docker volumes to reclaim disk space.

Use this command carefully, as deleted data cannot be recovered unless a backup exists.

---

# Understanding Bind Mounts

Bind mounts provide another method of sharing data between the host and a container.

Unlike Docker volumes, bind mounts use directories that already exist on the host machine.

Example:

```bash
docker run \
-v /home/ubuntu/project:/app \
nginx
```

In this example:

- `/home/ubuntu/project` exists on the host.
- `/app` exists inside the container.

Changes made in either location are reflected immediately in the other.

---

# Docker Volumes vs Bind Mounts

| Feature | Docker Volumes | Bind Mounts |
|----------|----------------|-------------|
| Managed by Docker | ✅ | ❌ |
| Uses Existing Host Directory | ❌ | ✅ |
| Portable Across Hosts | ✅ | Limited |
| Easier Backup | ✅ | Manual |
| Common in Production | ✅ | Sometimes |
| Common in Development | Yes | Yes |

### When to Use Docker Volumes

Docker volumes are generally preferred when:

- Storing database data
- Preserving uploaded files
- Persisting application logs
- Sharing data between containers
- Building production-ready applications

### When to Use Bind Mounts

Bind mounts are commonly used for:

- Local software development
- Editing source code without rebuilding images
- Sharing configuration files
- Testing application changes in real time

---

# Common Volume Commands

| Command | Purpose |
|----------|---------|
| `docker volume create` | Create a volume |
| `docker volume ls` | List volumes |
| `docker volume inspect` | Inspect a volume |
| `docker volume rm` | Remove a volume |
| `docker volume prune` | Remove unused volumes |

---

# Monitoring Storage

Display Docker disk usage.

```bash
docker system df
```

This command provides information about:

- Images
- Containers
- Volumes
- Build cache

It helps identify unused storage that can be cleaned up.

---

# Inspecting Container Mounts

Display detailed container information.

```bash
docker inspect demo-container
```

Review the **Mounts** section to confirm:

- Volume name
- Destination path
- Mount type
- Read/write status

This is useful when troubleshooting storage issues.

---

# Screenshot Gallery

Replace each placeholder with your corresponding implementation screenshot.

| Activity | Screenshot |
|----------|------------|
| Volume Creation | `images/volume-create.png` |
| Listing Volumes | `images/volume-list.png` |
| Inspecting a Volume | `images/volume-inspect.png` |
| Running Container with Volume | `images/container-with-volume.png` |
| Creating Data | `images/data-created.png` |
| Removing Container | `images/container-removed.png` |
| Verifying Persistent Data | `images/data-persisted.png` |
| Bind Mount Demonstration | `images/bind-mount.png` |
| Cleanup Commands | `images/cleanup.png` |

---

# Common Challenges

## Data Lost After Restart

### Possible Causes

- Data stored inside the container instead of the mounted volume.
- Incorrect mount path.
- Volume not attached.

### Resolution

Verify the mounted volume.

```bash
docker inspect demo-container
```

Confirm that files are being written to the mounted directory.

---

## Volume Cannot Be Removed

### Possible Causes

A running or stopped container is still using the volume.

### Resolution

Identify dependent containers.

```bash
docker ps -a
```

Remove the container before deleting the volume.

---

## Bind Mount Not Working

### Possible Causes

- Incorrect host directory.
- Directory permissions.
- Invalid mount path.

### Resolution

Verify that the host directory exists and has appropriate permissions before starting the container.

---

## Volume Not Visible

### Possible Causes

- Wrong volume name.
- Volume creation failed.
- Typographical error.

### Resolution

List all Docker volumes.

```bash
docker volume ls
```

Inspect the expected volume.

```bash
docker volume inspect project-data
```

---

# Best Practices

The following practices were applied throughout this project:

- Use named volumes for persistent application data.
- Separate application data from the container filesystem.
- Use bind mounts primarily for development workflows.
- Keep volume names descriptive and consistent.
- Inspect mounted volumes during troubleshooting.
- Regularly remove unused volumes.
- Avoid storing critical data only inside containers.
- Document storage architecture alongside application documentation.

---

# Future Improvements

Possible enhancements include:

- Backing up Docker volumes.
- Restoring volumes from backups.
- Encrypting persistent storage.
- Using cloud-managed storage services.
- Sharing volumes across multiple containers.
- Integrating persistent storage into Docker Compose.
- Exploring storage management in Kubernetes with Persistent Volumes (PVs) and Persistent Volume Claims (PVCs).

---

# References

Useful resources for further study:

- Docker Documentation
- Docker Volumes Documentation
- Docker Storage Documentation
- Linux Filesystem Documentation
- AWS EC2 Documentation

---

# Project Summary

This project demonstrates how Docker volumes provide persistent storage for containerized applications by separating application data from the container lifecycle. Through creating, mounting, inspecting, and reusing Docker volumes, the project validates that important data remains available even after containers are stopped or removed.

The comparison between Docker volumes and bind mounts highlights appropriate storage strategies for both production and development environments. These concepts form an essential foundation for building reliable, stateful containerized applications and are widely used in modern DevOps and cloud engineering workflows.

---

# Connect With Me

If you found this repository helpful or would like to discuss Docker, DevOps, Cloud Engineering, or Infrastructure Automation, feel free to connect.

- **GitHub:** https://github.com/Chukwuemeka-Peter-Eze
- **LinkedIn:** https://www.linkedin.com/in/chukwuemekapetereze/
- **Notion:** https://lumpy-bubble-7b0.notion.site/Containers-with-Docker-3a546a96f97480a88041ff2ff82a6b5f?source=copy_link

If you found this repository useful, consider giving it a ⭐ to support the project.

---
