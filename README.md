<p align="center"><strong>Built run commands by developers, for developers.</strong></p>

<p align="center">
  <img width="150" height="150" alt="image" src="https://github.com/user-attachments/assets/ac3a20f2-0dbe-468e-8cf4-059b4e16c5e5" />
</p>

---

# Docker Run Basics

This repository provides a clear and practical explanation of how docker run works and what actually happens behind the scenes when a container starts.

The objective is to build a solid mental model of container execution. Instead of focusing on advanced features, this repository concentrates on fundamentals:

- How Docker transforms an image into a running container
- What steps are executed internally when docker run is triggered
- How container lifecycle is directly tied to the main process (PID 1)
- How to inspect, stop, and remove containers properly

Understanding docker run is essential because it represents the bridge between static images and running workloads. Every higher-level abstraction in Docker (Compose, Swarm) or even Kubernetes ultimately depends on this core container execution model.

The examples in this repository are intentionally minimal to emphasize clarity and behavior over complexity.

## 📌 Table of Contents

- [Overview](#overview)
- [What `docker run` Does](#what-docker-run-does)
- [Running a Container](#running-a-container)
- [Foreground vs Detached Mode](#foreground-vs-detached-mode)
- [Listing Running Containers](#listing-running-containers)
- [Stopping a Container](#stopping-a-container)
- [Removing a Container](#removing-a-container)
- [Key Concepts](#key-concepts)
- [Container Lifecycle](#container-lifecycle)

---

## Overview

This repository explains what happens when you execute the docker run command. The goal is to understand how Docker creates and starts containers from images, and how to manage their lifecycle using basic commands.

## What `docker run` Does

The docker run command performs multiple actions in a single step:

1. Checks if the image exists locally.
2. Pulls the image from a registry if it does not exist.
3. Creates a new container from the image.
4. Starts the container.
5. Attaches the container’s output to your terminal (by default).

Basic syntax: `docker run <image>`

### Illustrative Image

<img width="1240" height="467" alt="image" src="https://github.com/user-attachments/assets/4d20ff72-e783-4a3f-a747-b503b9876ffa" />


## Running a Container

Example using the official Nginx image:

```dockerfile
docker run nginx
```

If the image is not available locally, Docker will download it automatically.
After the container starts, it runs the main process defined by the image.

In this case, Nginx starts and runs in the foreground.

## Foreground vs Detached Mode

By default, containers run in the foreground. Your terminal is attached to the container’s output.

To run a container in detached mode (background):

```dockerfile
docker run -d nginx
```

The `-d` flag tells Docker to run the container in the background and return control to your terminal immediately.

## Listing Running Containers

To see active containers:

```dockerfile
docker ps
```

This shows:

- Container ID
- Image used
- Command executed
- Creation time
- Status
- Exposed ports
- Container name

To see all containers (including stopped ones):

```dockerfile
docker ps -a
```

## Stopping a Container

To stop a running container:

```dockerfile
docker stop <container_id>
```

You can use either the full container ID or the container name.

## Removing a Container

After a container is stopped, it can be removed:

```dockerfile
docker rm <container_id>
```

To automatically remove a container after it stops:

```dockerfile
docker run --rm nginx
```

The `--rm` flag ensures the container is deleted once it exits.

## Key Concepts

- An image is a blueprint.
- A container is a running instance of an image.
- docker run creates and starts a container.
- Containers are isolated processes.
- Detached mode allows containers to run in the background.
- Containers are ephemeral unless explicitly preserved.

## Container Lifecycle

When you run:

```dockerfile
docker run nginx
```

The lifecycle is:

1. Verifies if the image exists locally.
2. Pulls the image if it is not available.
3. Creates a new container from the image.
4. Allocates a writable layer for the container.
5. Starts the container’s main process (PID 1).
6. Attaches the container output to your terminal (unless -d is used).
7. Keeps the container running while the main process is alive.
8. Stops the container when the main process exits.
