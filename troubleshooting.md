# Troubleshooting Guide

This document provides a structured approach to diagnosing and resolving common issues encountered while working with Docker volumes and persistent storage.

The project focuses on:

- Docker Volumes
- Persistent Storage
- Bind Mounts
- Container Lifecycle
- Volume Lifecycle

---

# Table of Contents

- Troubleshooting Workflow
- Docker Volume Not Found
- Volume Mount Issues
- Data Not Persisting
- Permission Issues
- Bind Mount Problems
- Container Cannot Access Files
- Volume Removal Errors
- Inspecting Volumes
- Storage Cleanup
- Useful Commands
- Troubleshooting Checklist
- Best Practices

---

# Troubleshooting Workflow

Always troubleshoot methodically.

```text
Problem Detected
        │
        ▼
Read Error Message
        │
        ▼
Verify Docker Engine
        │
        ▼
Verify Volume Exists
        │
        ▼
Inspect Volume
        │
        ▼
Verify Mount Configuration
        │
        ▼
Inspect Container
        │
        ▼
Verify Stored Data
        │
        ▼
Apply Fix
        │
        ▼
Retest
```

---

# Issue 1 — Docker Volume Does Not Exist

## Symptoms

```text
Error response from daemon:
volume not found
```

## Possible Causes

- Incorrect volume name
- Typographical error
- Volume was deleted

## Resolution

List all available volumes.

```bash
docker volume ls
```

Inspect the required volume.

```bash
docker volume inspect project-data
```

If the volume does not exist, recreate it.

```bash
docker volume create project-data
```

---

# Issue 2 — Data Does Not Persist

## Symptoms

Files disappear after deleting the container.

## Possible Causes

- Data stored inside the container instead of the mounted volume.
- Wrong mount path.
- Volume was never attached.

## Resolution

Inspect the container.

```bash
docker inspect demo-container
```

Verify the **Mounts** section.

Ensure the application writes data to the mounted directory rather than another location.

---

# Issue 3 — Incorrect Mount Path

## Symptoms

Files are not visible inside the mounted directory.

## Possible Causes

- Wrong destination path
- Typographical error
- Incorrect application configuration

## Resolution

Review the container configuration.

```bash
docker inspect demo-container
```

Confirm:

- Volume name
- Destination path
- Mount type

---

# Issue 4 — Volume Cannot Be Removed

## Symptoms

```text
volume is in use
```

## Possible Causes

A running or stopped container is still attached to the volume.

## Resolution

List all containers.

```bash
docker ps -a
```

Remove the dependent container.

```bash
docker rm -f demo-container
```

Retry removing the volume.

```bash
docker volume rm project-data
```

---

# Issue 5 — Permission Denied

## Symptoms

```text
Permission denied
```

## Possible Causes

- Linux file permissions
- Application user lacks write access
- Host directory permissions

## Resolution

Verify permissions inside the container.

```bash
ls -l
```

Review ownership and permissions before restarting the application.

---

# Issue 6 — Bind Mount Not Working

## Symptoms

Host files are not visible inside the container.

## Possible Causes

- Incorrect host path
- Directory does not exist
- Permission issues

## Resolution

Verify the host directory.

```bash
ls /home/ubuntu/project
```

Restart the container after correcting the mount path.

---

# Issue 7 — Container Cannot Access Mounted Files

## Symptoms

Application cannot read or write files.

## Possible Causes

- Wrong mount destination
- Read-only mount
- Incorrect permissions

## Resolution

Inspect the container.

```bash
docker inspect demo-container
```

Review the **Mounts** section and verify that the mount is writable.

---

# Issue 8 — Volume Not Listed

## Symptoms

Expected volume is missing.

## Possible Causes

- Volume removed
- Incorrect Docker context
- Typographical error

## Resolution

Display all volumes.

```bash
docker volume ls
```

If necessary, recreate the volume.

---

# Issue 9 — Storage Consuming Too Much Space

## Symptoms

Docker storage usage continues to grow.

## Resolution

Display Docker disk usage.

```bash
docker system df
```

Remove unused volumes.

```bash
docker volume prune
```

Remove unused Docker resources.

```bash
docker system prune
```

---

# Inspecting Docker Volumes

Inspect detailed volume information.

```bash
docker volume inspect project-data
```

Review:

- Driver
- Mount point
- Scope
- Labels

---

# Inspecting Container Mounts

Inspect the running container.

```bash
docker inspect demo-container
```

Verify:

- Mounted volume
- Destination
- Mount type
- Read/write status

---

# Useful Diagnostic Commands

| Command | Purpose |
|----------|---------|
| `docker volume ls` | List Docker volumes |
| `docker volume inspect` | Inspect a volume |
| `docker inspect` | Inspect container mounts |
| `docker ps` | View running containers |
| `docker system df` | Display Docker storage usage |
| `docker volume prune` | Remove unused volumes |
| `docker system prune` | Clean Docker resources |

---

# Troubleshooting Checklist

Before concluding the project is functioning correctly, verify the following:

- Docker Engine is running.
- Volume exists.
- Volume is mounted correctly.
- Data is written to the mounted directory.
- Data persists after container removal.
- Bind mount functions correctly.
- Storage usage is within expected limits.
- No critical errors appear during execution.

---

# Best Practices

- Use named Docker volumes for persistent application data.
- Store application data outside the container filesystem.
- Use bind mounts primarily for development workflows.
- Verify mounted volumes before troubleshooting applications.
- Use descriptive volume names.
- Remove unused volumes periodically.
- Back up important persistent data before cleanup.
- Document storage architecture alongside deployment documentation.

---

# Conclusion

Persistent storage is a critical component of stateful containerized applications. Docker volumes provide a reliable mechanism for preserving data beyond the lifecycle of individual containers, while bind mounts offer flexibility for development workflows. By following a structured troubleshooting process, inspecting volumes and mounts, and understanding the separation between container and storage lifecycles, most storage-related issues can be diagnosed and resolved efficiently.
