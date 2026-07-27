# Troubleshooting Guide

This document provides solutions to common issues that may occur while working with Docker volumes and bind mounts.

The goal is to help identify problems quickly, understand why they occur, and apply the correct solution.

---

# Issue 1 — Data Is Lost After Recreating a Container

## Symptoms

- Files created inside the container are missing.
- Application data disappears after removing the container.
- Expected files cannot be found after starting a new container.

## Possible Causes

- Data was written to the container's writable filesystem instead of the mounted volume.
- The Docker volume was not mounted correctly.
- An incorrect mount path was specified.

## How to Verify

Inspect the container.

```bash
docker inspect demo-container
```

Review the **Mounts** section and confirm that the expected Docker volume is attached.

Inside the container, verify where the file was created.

```bash
pwd

ls /data
```

## Resolution

- Ensure the volume is mounted correctly.
- Write application data only inside the mounted directory.
- Recreate the container if necessary using the correct volume mapping.

---

# Issue 2 — Docker Volume Does Not Exist

## Symptoms

Running the following command returns an error:

```bash
docker volume inspect my-volume
```

Example:

```text
Error: No such volume: my-volume
```

## Possible Causes

- The volume was never created.
- The volume name is incorrect.
- A typographical error was made.

## Resolution

List all available volumes.

```bash
docker volume ls
```

If the expected volume is missing, create it.

```bash
docker volume create my-volume
```

---

# Issue 3 — Unable to Remove a Docker Volume

## Symptoms

Running:

```bash
docker volume rm my-volume
```

returns an error indicating that the volume is still in use.

## Possible Cause

A running or stopped container is still attached to the volume.

## Resolution

List all containers.

```bash
docker ps -a
```

Inspect the containers if necessary.

```bash
docker inspect <container-name>
```

Remove any container using the volume.

```bash
docker rm -f <container-name>
```

Retry removing the volume.

```bash
docker volume rm my-volume
```

---

# Issue 4 — Bind Mount Is Not Working

## Symptoms

- Files from the host are not visible inside the container.
- Changes made on the host are not reflected in the container.

## Possible Causes

- Incorrect host directory.
- Typographical error in the mount path.
- The directory does not exist.
- Docker does not have permission to access the directory.

## Resolution

Verify that the directory exists.

Example:

```bash
ls /path/to/local/folder
```

Restart the container using the correct path.

---

# Issue 5 — Container Fails to Start

## Symptoms

The container exits immediately after starting.

Check its status.

```bash
docker ps -a
```

View the logs.

```bash
docker logs demo-container
```

## Possible Causes

- Invalid image name.
- Incorrect command.
- Application startup failure.
- Volume mounted to an unexpected location.

## Resolution

Review the error logs and verify:

- Image name
- Container command
- Mount destination
- Volume configuration

---

# Issue 6 — Cannot Access Files Inside the Volume

## Symptoms

The mounted directory exists, but expected files are missing.

## Possible Causes

- Files were created outside the mounted directory.
- A different volume was mounted.
- The wrong container is being inspected.

## Resolution

Verify the mounted directory.

```bash
docker inspect demo-container
```

Check the contents.

```bash
ls /data
```

Confirm the correct volume.

```bash
docker volume ls
```

---

# Issue 7 — Docker Storage Consumes Too Much Disk Space

## Symptoms

Docker uses a large amount of local storage.

## Resolution

View Docker disk usage.

```bash
docker system df
```

Remove unused containers.

```bash
docker container prune
```

Remove unused images.

```bash
docker image prune
```

Remove unused volumes.

```bash
docker volume prune
```

> **Warning:** These cleanup commands permanently remove unused Docker resources. Review them carefully before proceeding.

---

# General Troubleshooting Checklist

When diagnosing Docker volume issues, verify the following:

- Docker is running.
- The correct volume name is being used.
- The volume exists.
- The container is running.
- The volume is mounted correctly.
- Data is written inside the mounted directory.
- The correct container is being inspected.
- Docker has permission to access any bind-mounted directories.

---

# Helpful Diagnostic Commands

| Command | Purpose |
|----------|---------|
| `docker ps` | View running containers |
| `docker ps -a` | View all containers |
| `docker volume ls` | List Docker volumes |
| `docker volume inspect <volume>` | Inspect a volume |
| `docker inspect <container>` | Inspect a container |
| `docker logs <container>` | View container logs |
| `docker system df` | Display Docker disk usage |

---

# Key Takeaways

Most Docker volume issues fall into one of the following categories:

- Incorrect volume configuration
- Incorrect mount path
- Data written outside the mounted directory
- Containers still using a volume
- Typographical errors in Docker commands

A structured troubleshooting approach—checking the volume, the container, and the mount configuration—will resolve the majority of storage-related issues.