# Project Demonstration Video Script

**Project:** Docker Volumes for Persistent Data Storage on AWS

**Target Duration:** 8–10 Minutes

**Audience**

- Recruiters
- Hiring Managers
- DevOps Engineers
- Cloud Engineers
- Platform Engineers
- Site Reliability Engineers (SREs)

---

# Video Objective

The objective of this demonstration is to showcase how Docker volumes provide persistent storage for containerized applications.

This project demonstrates:

- Creating Docker volumes
- Mounting persistent storage
- Writing application data
- Removing containers
- Reusing existing volumes
- Verifying data persistence
- Comparing Docker volumes with bind mounts

The demonstration emphasizes that application data should remain available even when containers are replaced.

---

# Scene 1 — Introduction (45 seconds)

### Screen

Open the GitHub repository homepage.

### Narration

> Hello everyone, and welcome.

> In this project, I demonstrate how Docker volumes provide persistent storage for containerized applications running on AWS.

> One of the limitations of containers is that they are designed to be ephemeral. Docker volumes solve this problem by separating application data from the container lifecycle.

> Throughout this project, I documented the implementation process, architecture, storage concepts, troubleshooting techniques, and engineering lessons learned.

---

# Scene 2 — Repository Overview (1 minute)

### Screen

Scroll through the repository.

Highlight:

- README
- Commands Guide
- Setup Guide
- Troubleshooting Guide
- Lessons Learned
- Architecture Diagram

### Narration

> This repository is organized to make the implementation easy to reproduce.

> It contains detailed documentation explaining Docker storage architecture, persistent storage concepts, setup procedures, troubleshooting steps, and operational best practices.

---

# Scene 3 — Architecture Diagram (1 minute)

### Screen

Open:

```text
images/architecture.png
```

Zoom into the architecture.

### Narration

> This architecture demonstrates the relationship between a running container and a Docker volume.

> The container mounts a Docker-managed volume where application data is stored.

> Even if the container is removed, the Docker volume continues to exist independently, allowing a new container to reuse the same data.

---

# Scene 4 — Creating a Docker Volume (1 minute)

### Screen

Run:

```bash
docker volume create project-data
```

Then:

```bash
docker volume ls
```

### Narration

> Docker volumes are created and managed directly by Docker.

> Listing the available volumes confirms that the new persistent storage has been successfully created.

---

# Scene 5 — Running a Container with a Mounted Volume (1 minute)

### Screen

Run:

```bash
docker run -d \
-v project-data:/usr/share/nginx/html \
--name nginx-volume \
nginx
```

Show:

```bash
docker inspect nginx-volume
```

Highlight the **Mounts** section.

### Narration

> This container mounts the Docker volume into its filesystem.

> The mounted directory is where persistent application data will be stored.

---

# Scene 6 — Demonstrating Data Persistence (1 minute)

### Screen

Access the container.

Create a file.

```bash
echo "Docker Volume Test" > /usr/share/nginx/html/index.html
```

Remove the container.

```bash
docker rm -f nginx-volume
```

Create another container using the same volume.

Display:

```bash
cat /usr/share/nginx/html/index.html
```

### Narration

> Although the original container has been removed, the data remains available because it is stored in the Docker volume rather than the container itself.

> This demonstrates one of the most important concepts in containerized applications: separating storage from compute.

---

# Scene 7 — Demonstrating Bind Mounts (1 minute)

### Screen

Run:

```bash
docker run -d \
-v /home/ubuntu/project:/usr/share/nginx/html \
nginx
```

Show the host directory and the corresponding files inside the container.

### Narration

> Bind mounts provide another way of sharing data by mapping an existing directory from the host into the container.

> They are commonly used during development because changes made on the host are immediately visible inside the container.

---

# Scene 8 — Inspecting Docker Storage (1 minute)

### Screen

Run:

```bash
docker volume inspect project-data
```

Then:

```bash
docker system df
```

### Narration

> Docker provides tools for inspecting storage configuration and monitoring disk usage.

> These commands help verify volume configuration and identify unused storage resources.

---

# Scene 9 — Lessons Learned (1 minute)

### Screen

Open the "Lessons Learned" section in the repository.

### Narration

> This project reinforced several important concepts, including the separation of container and storage lifecycles, the role of Docker volumes in stateful applications, and the differences between Docker volumes and bind mounts.

> It also highlighted the importance of documenting infrastructure and validating persistent storage during deployments.

---

# Scene 10 — Conclusion (30–45 seconds)

### Screen

Return to the repository homepage.

### Narration

> Thank you for watching this demonstration.

> This project provides a practical example of implementing persistent storage using Docker volumes on AWS.

> Feedback and suggestions are always welcome. Thank you for your time.

---

# Recording Checklist

Before recording, verify the following:

- Terminal font size is readable.
- Docker Engine is running.
- Docker volume created successfully.
- Volume mounted correctly.
- Data persistence verified.
- Bind mount demonstration completed.
- Architecture diagram added.
- README updated.
- Sensitive information removed from the terminal.
- Screenshots included in the repository.
- Desktop notifications disabled.

---

# Suggested Repository Assets

Include the following assets to strengthen the repository:

- `architecture.png`
- Screenshot of volume creation
- Screenshot of volume list
- Screenshot of volume inspection
- Screenshot of mounted container
- Screenshot showing created data
- Screenshot after container removal
- Screenshot proving data persistence
- Screenshot demonstrating bind mounts
- Screenshot of cleanup commands
- Short GIF showing the complete persistence workflow
- Project thumbnail image

---

# Estimated Timeline

| Section | Duration |
|----------|----------|
| Introduction | 0:45 |
| Repository Overview | 1:00 |
| Architecture | 1:00 |
| Create Volume | 1:00 |
| Mount Volume | 1:00 |
| Data Persistence | 1:00 |
| Bind Mounts | 1:00 |
| Storage Inspection | 1:00 |
| Lessons Learned | 1:00 |
| Conclusion | 0:30 |

**Total Duration:** Approximately **9–10 minutes**

---

# Final Notes

This demonstration should emphasize not only how Docker volumes are created and mounted, but also why persistent storage is essential for stateful applications. Clearly explaining the difference between the lifecycle of a container and the lifecycle of a volume will help showcase your understanding of container storage concepts and practical DevOps workflows. The combination of architecture, live demonstrations, troubleshooting, and documentation provides a comprehensive overview of persistent storage management in Docker.
