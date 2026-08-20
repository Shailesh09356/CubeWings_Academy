# Docker Introduction — Fundamentals, Architecture, Images, Containers & VMs
## Table of Contents

- [1. What is Docker?](#1-what-is-docker)
- [2. Why Docker is Used](#2-why-docker-is-used)
- [3. What is Containerization?](#3-what-is-containerization)
- [4. Docker Fundamentals](#4-docker-fundamentals)
- [5. Docker Architecture](#5-docker-architecture)
- [6. Docker Client](#6-docker-client)
- [7. Docker Daemon](#7-docker-daemon)
- [8. Docker Engine](#8-docker-engine)
- [9. Container Runtime](#9-container-runtime)
- [10. Docker Registry](#10-docker-registry)
- [11. Docker Hub](#11-docker-hub)
- [12. What is a Docker Image?](#12-what-is-a-docker-image)
- [13. Image vs Container](#13-image-vs-container)
- [14. Docker Images are Immutable](#14-docker-images-are-immutable)
- [15. Image Layers](#15-image-layers)
- [16. Docker Container](#16-docker-container)
- [17. What Happens When You Run a Container?](#17-what-happens-when-you-run-a-container)
- [18. Container Lifecycle](#18-container-lifecycle)
- [19. Container Isolation](#19-container-isolation)
- [20. Containers Share the Host Kernel](#20-containers-share-the-host-kernel)
- [21. Virtual Machine Architecture](#21-virtual-machine-architecture)
- [22. Container vs Virtual Machine](#22-container-vs-virtual-machine)
- [23. VM vs Container — Main Differences](#23-vm-vs-container--main-differences)
- [24. Example: Running Three Applications](#24-example-running-three-applications)
- [25. Why Containers are Lightweight](#25-why-containers-are-lightweight)
- [26. Containers Are Not Lightweight VMs](#26-containers-are-not-lightweight-vms)
- [27. Can Containers Replace VMs?](#27-can-containers-replace-vms)
- [28. Dockerfile](#28-dockerfile)
- [29. Image → Container Workflow](#29-image--container-workflow)
- [30. Docker's Core Building Blocks](#30-dockers-core-building-blocks)
- [31. Important Terms to Remember](#31-important-terms-to-remember)
- [32. The Most Important Mental Model](#32-the-most-important-mental-model)
- [33. What You Should Learn Next](#33-what-you-should-learn-next)

---

## 1. What is Docker?

Docker is a platform used to **build, package, deploy, and run applications in containers**.

A container packages an application together with the dependencies it needs so that it can run consistently across different environments.

### Simple example

Without Docker:

```text
Developer's Laptop
    ↓
Application works
    ↓
Move to another server
    ↓
Different OS / libraries / versions
    ↓
Application may fail
```

With Docker:

```text
Application
+ Dependencies
+ Libraries
+ Configuration
        ↓
     Docker Image
        ↓
     Container
        ↓
Runs consistently across environments
```

### Simple definition

> Docker allows you to package an application and its dependencies into a portable container that can run consistently on different systems.

---

# 2. Why Docker is Used

Docker solves common problems such as:

- "It works on my machine."
- Dependency conflicts
- Different software versions
- Difficult application deployment
- Environment inconsistencies
- Slow application setup
- Resource-heavy virtual machines

Docker makes applications:

- Portable
- Consistent
- Isolated
- Easy to deploy
- Easy to scale
- Easy to reproduce

---

# 3. What is Containerization?

Containerization is the process of packaging an application and its required dependencies into an isolated environment called a **container**.

Example:

```text
Web Application
    +
Python
    +
Required Python Libraries
    +
Configuration
    ↓
Container
```

The container uses the host machine's **Linux kernel** while keeping the application environment isolated.

---

# 4. Docker Fundamentals

The most important Docker concepts are:

```text
Docker
│
├── Docker Engine
│
├── Images
│
├── Containers
│
├── Dockerfile
│
├── Docker Registry
│
├── Docker Network
│
└── Docker Volumes
```

For your initial learning, focus mainly on:

```text
Docker Engine
Images
Containers
Dockerfile
Docker Registry
```

---

# 5. Docker Architecture

Docker follows a **client-server architecture**.

The major components are:

```text
Docker Client
     │
     │ Docker API
     ↓
Docker Daemon
     │
     ├── Images
     ├── Containers
     ├── Networks
     └── Volumes
```

---

# 6. Docker Client

The Docker Client is the interface you use to interact with Docker.

The most common client is the Docker CLI.

Examples:

```bash
docker run nginx
docker ps
docker images
docker stop web
```

When you execute:

```bash
docker run nginx
```

the Docker CLI sends the request to the Docker daemon.

---

# 7. Docker Daemon

The Docker daemon is the background service responsible for managing Docker resources.

It manages:

- Containers
- Images
- Networks
- Volumes
- Container lifecycle

The daemon is commonly referred to as:

```text
dockerd
```

Conceptually:

```text
You
 ↓
docker run nginx
 ↓
Docker CLI
 ↓
Docker Daemon
 ↓
Create and start container
```

---

# 8. Docker Engine

Docker Engine is the core technology that allows you to build and run containers.

It includes the components necessary to:

- Build images
- Run containers
- Manage networks
- Manage storage
- Manage container lifecycle

Simplified:

```text
Docker Engine
│
├── Docker CLI
├── Docker Daemon
└── Container Runtime
```

---

# 9. Container Runtime

The container runtime is responsible for actually running containers.

Docker uses **containerd** as a major component of its container runtime architecture.

At a simplified level:

```text
Docker CLI
    ↓
Docker Daemon
    ↓
containerd
    ↓
runc
    ↓
Container
```

You do not need to deeply understand `containerd` and `runc` at the beginning, but know what they are.

---

# 10. Docker Registry

A Docker Registry stores and distributes Docker images.

The most well-known public registry is Docker Hub.

Conceptually:

```text
Docker Host
    ↕
Docker Registry
    ↕
Docker Images
```

Example:

```bash
docker pull nginx
```

Docker downloads the Nginx image from a registry.

You can also upload your own image:

```bash
docker push username/myapp:v1
```

---

# 11. Docker Hub

Docker Hub is a public registry for Docker images.

It contains images such as:

```text
nginx
ubuntu
debian
redis
mysql
postgres
python
node
```

Example:

```bash
docker pull ubuntu
```

The image is downloaded from Docker Hub to your local Docker host.

---

# 12. What is a Docker Image?

A Docker image is a **read-only template used to create containers**.

An image contains things such as:

- Application
- Libraries
- Dependencies
- Files
- Configuration
- Metadata

Example:

```text
Ubuntu Image
│
├── Ubuntu filesystem
├── Basic utilities
└── Configuration
```

When you run the image:

```bash
docker run ubuntu
```

Docker creates a container from that image.

---

# 13. Image vs Container

This is one of the most important Docker concepts.

```text
IMAGE
  ↓
Template
  ↓
Used to create
  ↓
CONTAINER
  ↓
Running instance
```

### Simple analogy

Think of a Docker image as a **class** in programming.

A container is an **object/instance** created from that class.

Another analogy:

```text
Image = Recipe
Container = Prepared meal
```

The image is the blueprint/template.

The container is the running instance.

---

# 14. Docker Images are Immutable

Docker images are designed to be immutable.

This means the original image is not normally modified when a container runs.

Instead:

```text
Image
  ↓
Container
  ↓
Container gets its own writable layer
```

If you modify files inside a running container, the original image remains unchanged.

---

# 15. Image Layers

Docker images are built using layers.

For example:

```text
Application Layer
-----------------
Python Dependencies
-----------------
Python
-----------------
Ubuntu Base
-----------------
Base Layer
```

Each Dockerfile instruction can create a layer.

Example:

```dockerfile
FROM ubuntu
RUN apt update
RUN apt install python3
COPY app.py /app/
```

Conceptually:

```text
Layer 4 → app.py
Layer 3 → Python
Layer 2 → Ubuntu packages
Layer 1 → Ubuntu base image
```

### Why layers are useful

Layers allow Docker to:

- Reuse existing data
- Reduce build time
- Reduce storage usage
- Cache unchanged steps

---

# 16. Docker Container

A container is a **running or stopped instance of a Docker image**.

Example:

```bash
docker run -d --name web nginx
```

This creates a container from the Nginx image.

Conceptually:

```text
Nginx Image
     ↓
Docker Engine
     ↓
Nginx Container
```

---

# 17. What Happens When You Run a Container?

Suppose you execute:

```bash
docker run -d --name web nginx
```

Docker roughly performs these steps:

```text
1. Check whether nginx image exists locally
          ↓
2. If not, pull image from registry
          ↓
3. Create container
          ↓
4. Add writable container layer
          ↓
5. Configure networking
          ↓
6. Configure storage
          ↓
7. Start container process
```

---

# 18. Container Lifecycle

A container can have different states.

```text
Created
   ↓
Running
   ↓
Stopped
   ↓
Removed
```

Example:

```bash
docker create nginx
docker start <container>
docker stop <container>
docker rm <container>
```

Using `docker run` combines creation and starting:

```bash
docker run nginx
```

---

# 19. Container Isolation

Containers provide process and resource isolation.

For example:

```text
Host Operating System
│
├── Container A
│   └── Web Server
│
├── Container B
│   └── Database
│
└── Container C
    └── Application
```

Applications can be isolated from each other while sharing the host kernel.

Linux technologies involved include:

- Namespaces
- cgroups
- Capabilities
- Seccomp
- Linux filesystem isolation

You do not need to master these immediately, but they become important for Docker security.

---

# 20. Containers Share the Host Kernel

This is one of the biggest differences between containers and virtual machines.

```text
CONTAINERS

Host OS
└── Linux Kernel
    ├── Container A
    ├── Container B
    └── Container C
```

Containers generally do **not** contain their own complete operating-system kernel.

They share the host kernel.

---

# 21. Virtual Machine Architecture

A virtual machine normally includes a complete guest operating system.

```text
Physical Hardware
       ↓
Hypervisor
       ↓
┌──────────────┬──────────────┐
│ VM 1         │ VM 2         │
│ Guest OS     │ Guest OS     │
│ Application  │ Application  │
└──────────────┴──────────────┘
```

Each VM has its own operating system/kernel.

---

# 22. Container vs Virtual Machine

## Virtual Machine

```text
Hardware
   ↓
Hypervisor
   ↓
Guest OS
   ↓
Libraries
   ↓
Application
```

## Container

```text
Hardware
   ↓
Host OS
   ↓
Docker Engine
   ↓
Container
   ↓
Application
```

---

# 23. VM vs Container — Main Differences

| Feature | Virtual Machine | Container |
|---|---|---|
| Virtualization | Hardware-level | OS-level |
| Guest OS | Yes | Usually no separate guest kernel |
| Kernel | Separate kernel per VM | Shared host kernel |
| Size | Usually GBs | Usually MBs to GBs |
| Startup | Usually slower | Usually very fast |
| Resource usage | Higher | Lower |
| Isolation | Strong | Process-level isolation |
| Portability | Good | Excellent |
| Density | Lower | Higher |
| Performance overhead | Higher | Lower |
| Best for | Full OS isolation | Applications/services |

---

# 24. Example: Running Three Applications

### Using Virtual Machines

```text
Physical Server
│
├── VM 1
│   ├── Linux OS
│   └── Web Server
│
├── VM 2
│   ├── Linux OS
│   └── Database
│
└── VM 3
    ├── Linux OS
    └── Application
```

Three guest operating systems are running.

### Using Containers

```text
Physical Server
│
├── Host OS
│
└── Docker
    ├── Web Container
    ├── Database Container
    └── Application Container
```

The containers share the host kernel.

---

# 25. Why Containers are Lightweight

A VM needs a complete guest OS.

A container generally only needs:

```text
Application
+
Dependencies
+
Container filesystem
```

Therefore, multiple containers can run efficiently on the same host.

---

# 26. Containers Are Not Lightweight VMs

This is an important concept.

A container is **not simply a small virtual machine**.

A VM virtualizes a machine/OS environment.

A container isolates processes while sharing the host kernel.

```text
VM:
Virtual Machine
    ↓
Complete Guest OS
    ↓
Application

Container:
Isolated Process
    ↓
Shared Host Kernel
    ↓
Application
```

---

# 27. Can Containers Replace VMs?

Not always.

Containers are excellent for:

- Microservices
- Web applications
- APIs
- Development environments
- CI/CD
- Security labs
- Application deployment
- Scalable services

VMs are useful when you need:

- A complete operating system
- Stronger isolation
- Different operating systems
- Kernel-level customization
- Traditional server workloads

In real infrastructure, **VMs and containers are often used together**.

Example:

```text
Physical Server
      ↓
VM
      ↓
Linux
      ↓
Docker
      ↓
Containers
```

---

# 28. Dockerfile

A Dockerfile is a text file containing instructions for building a Docker image.

Example:

```dockerfile
FROM ubuntu:24.04

RUN apt-get update

RUN apt-get install -y nginx

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

Build:

```bash
docker build -t mynginx .
```

Run:

```bash
docker run -d -p 8080:80 mynginx
```

---

# 29. Image → Container Workflow

The basic Docker workflow is:

```text
Dockerfile
     ↓
docker build
     ↓
Docker Image
     ↓
docker run
     ↓
Docker Container
```

For an existing public image:

```text
Docker Registry
     ↓
docker pull
     ↓
Docker Image
     ↓
docker run
     ↓
Container
```

---

# 30. Docker's Core Building Blocks

Remember these five concepts:

### 1. Image

The blueprint/template.

### 2. Container

The running instance of an image.

### 3. Dockerfile

Instructions for building an image.

### 4. Registry

Place where images are stored and distributed.

### 5. Docker Engine

The platform that builds and runs containers.

Simple relationship:

```text
Dockerfile
    ↓
  Image
    ↓
Container
    ↑
Docker Engine
    ↕
Registry
```

---

# 31. Important Terms to Remember

| Term | Meaning |
|---|---|
| Docker | Container platform |
| Docker Engine | Core Docker technology |
| Docker CLI | Command-line interface |
| Docker Daemon | Background service managing Docker |
| Image | Read-only template |
| Container | Instance of an image |
| Dockerfile | Instructions for building an image |
| Registry | Image storage/distribution system |
| Docker Hub | Public Docker registry |
| Volume | Persistent container storage |
| Network | Container communication system |
| Compose | Tool for managing multi-container applications |
| Namespace | Provides process/resource isolation |
| cgroups | Controls resource usage |
| containerd | Container runtime component |
| runc | Low-level container runtime |

---

# 32. The Most Important Mental Model

Remember this:

```text
                    DOCKER
                       │
              ┌────────┴────────┐
              │                 │
          Dockerfile          Registry
              │                 │
              ↓                 ↓
           Build             Pull
              │                 │
              └───────┬─────────┘
                      ↓
                    IMAGE
                      │
                   docker run
                      ↓
                 CONTAINER
                      │
          ┌───────────┼───────────┐
          ↓           ↓           ↓
       Network      Storage     Process
```

And remember:

```text
Image = Blueprint
Container = Running instance
Dockerfile = Image recipe
Registry = Image warehouse
Docker Engine = Runs everything
```

---

# 33. What You Should Learn Next

After understanding this introduction, follow this order:

```text
1. Docker Commands
        ↓
2. Docker Images
        ↓
3. Docker Containers
        ↓
4. Dockerfile
        ↓
5. Docker Networking
        ↓
6. Docker Volumes
        ↓
7. Docker Compose
        ↓
8. Docker Troubleshooting
        ↓
9. Docker Security
        ↓
10. Docker + Cloud
        ↓
11. Kubernetes
```

For a networking and cybersecurity-focused career, pay special attention to:

```text
Containers
Networking
Image Security
Container Isolation
Docker Security
Logging
Monitoring
Docker + Cloud
Kubernetes
```
