# Docker Commands — Complete Practical Reference

A practical Docker CLI reference covering the commands most useful for learning Docker, networking, troubleshooting, and security.

---

## Table of Contents

- [1. Docker Help & Version](#1-docker-help--version)
- [2. Images](#2-images)
- [3. Running Containers](#3-running-containers)
- [4. Container Listing](#4-container-listing)
- [5. Container Lifecycle](#5-container-lifecycle)
- [6. Removing Containers](#6-removing-containers)
- [7. Container Information](#7-container-information)
- [8. Execute Commands Inside Containers](#8-execute-commands-inside-containers)
- [9. Copy Files](#9-copy-files)
- [10. Port & Networking](#10-port--networking)
- [11. Volumes & Storage](#11-volumes--storage)
- [12. Bind Mounts](#12-bind-mounts)
- [13. Named Volumes](#13-named-volumes)
- [14. Building Images](#14-building-images)
- [15. Dockerfile-Related Commands](#15-dockerfile-related-commands)
- [16. Tagging Images](#16-tagging-images)
- [17. Push & Pull from Registries](#17-push--pull-from-registries)
- [18. Export & Import](#18-export--import)
- [19. Docker Compose](#19-docker-compose)
- [20. System Cleanup](#20-system-cleanup)
- [21. Container Resource Limits](#21-container-resource-limits)
- [22. Environment Variables](#22-environment-variables)
- [23. Container Restart Policies](#23-container-restart-policies)
- [24. Health Checks](#24-health-checks)
- [25. Security-Related Commands](#25-security-related-commands)
- [26. Useful Troubleshooting Commands](#26-useful-troubleshooting-commands)
- [27. Useful Command Patterns](#27-useful-command-patterns)
- [28. Most Important Commands to Memorize First](#28-most-important-commands-to-memorize-first)
- [29. Recommended Learning Order](#29-recommended-learning-order)

---

## 1. Docker Help & Version

### `docker --version`
Shows the installed Docker CLI version.

```bash
docker --version
```

### `docker version`
Shows client and server/Engine version information.

```bash
docker version
```

### `docker info`
Displays detailed information about the Docker Engine, containers, images, storage driver, runtimes, and more.

```bash
docker info
```

### `docker help`
Shows Docker command help.

```bash
docker help
docker run --help
docker network --help
```

---

# 2. Images

## `docker pull`

Downloads an image from a registry such as Docker Hub.

```bash
docker pull ubuntu
docker pull ubuntu:24.04
```

Format:

```bash
docker pull IMAGE[:TAG]
```

---

## `docker images`

Lists locally available images.

```bash
docker images
```

Useful:

```bash
docker images -a
```

`-a` shows intermediate/dangling images as well.

---

## `docker image ls`

Modern equivalent of `docker images`.

```bash
docker image ls
```

---

## `docker image inspect`

Shows detailed image metadata.

```bash
docker image inspect ubuntu
```

Useful for checking image configuration, layers, architecture, environment variables, and entrypoint information.

---

## `docker image history`

Shows the layers used to build an image.

```bash
docker image history ubuntu
```

Useful for understanding Docker image layers.

---

## `docker image rm`

Removes an image.

```bash
docker image rm ubuntu
```

Force removal:

```bash
docker image rm -f ubuntu
```

---

## `docker rmi`

Shortcut for removing images.

```bash
docker rmi ubuntu
```

---

## `docker image prune`

Removes unused/dangling images.

```bash
docker image prune
```

Remove all unused images:

```bash
docker image prune -a
```

Be careful with `-a`.

---

# 3. Running Containers

## `docker run`

Creates and starts a new container from an image.

```bash
docker run ubuntu
```

Common example:

```bash
docker run -it ubuntu bash
```

Important options:

- `-i` — keep STDIN open
- `-t` — allocate a terminal
- `-d` — detached/background mode
- `--name` — assign container name
- `-p` — publish a port
- `-P` — publish exposed ports automatically
- `-e` — set environment variable
- `-v` — mount volume/bind mount
- `--network` — connect to a network
- `--restart` — restart policy
- `--rm` — automatically remove container when it stops
- `--privileged` — give extended privileges; avoid unless required
- `--read-only` — make container filesystem read-only
- `--user` — run as a specific user
- `--cpus` — limit CPU
- `--memory` — limit memory

Example:

```bash
docker run -d --name web -p 8080:80 nginx
```

This:

- runs Nginx in background
- names it `web`
- maps host port `8080` to container port `80`

---

# 4. Container Listing

## `docker ps`

Shows currently running containers.

```bash
docker ps
```

Show all containers:

```bash
docker ps -a
```

Useful options:

```bash
docker ps -q
docker ps --no-trunc
```

`-q` prints only container IDs.

---

## `docker container ls`

Modern equivalent of `docker ps`.

```bash
docker container ls
docker container ls -a
```

---

# 5. Container Lifecycle

## `docker start`

Starts an existing stopped container.

```bash
docker start CONTAINER
```

Example:

```bash
docker start web
```

---

## `docker stop`

Gracefully stops a running container.

```bash
docker stop web
```

Force stop:

```bash
docker stop -t 0 web
```

---

## `docker restart`

Restarts a container.

```bash
docker restart web
```

---

## `docker kill`

Immediately sends a kill signal to a container.

```bash
docker kill web
```

Use this when normal stopping is not working or an immediate stop is required.

---

## `docker pause`

Pauses processes inside a container.

```bash
docker pause web
```

Resume:

```bash
docker unpause web
```

---

# 6. Removing Containers

## `docker rm`

Removes a stopped container.

```bash
docker rm web
```

Force removal:

```bash
docker rm -f web
```

Remove multiple:

```bash
docker rm container1 container2
```

---

## `docker container prune`

Removes stopped containers.

```bash
docker container prune
```

---

# 7. Container Information

## `docker inspect`

Displays detailed low-level information about containers.

```bash
docker inspect web
```

Very useful for:

- IP address
- Network configuration
- Mounts
- Environment variables
- Container configuration
- Restart policy
- Resource configuration

Example:

```bash
docker inspect -f '{{.NetworkSettings.IPAddress}}' web
```

---

## `docker logs`

Displays container logs.

```bash
docker logs web
```

Follow logs:

```bash
docker logs -f web
```

Show timestamps:

```bash
docker logs -t web
```

Show recent logs:

```bash
docker logs --tail 100 web
```

Follow only recent lines:

```bash
docker logs --tail 50 -f web
```

---

## `docker stats`

Shows live resource usage.

```bash
docker stats
```

For one container:

```bash
docker stats web
```

Shows CPU, memory, network I/O, and block I/O.

---

## `docker top`

Shows processes running inside a container.

```bash
docker top web
```

---

# 8. Execute Commands Inside Containers

## `docker exec`

Runs a command inside a running container.

```bash
docker exec web ls
```

Open an interactive shell:

```bash
docker exec -it web bash
```

If Bash isn't available:

```bash
docker exec -it web sh
```

Run as a specific user:

```bash
docker exec -u root -it web bash
```

Run an environment command:

```bash
docker exec web env
```

---

## `docker attach`

Attaches your terminal to the main process of a running container.

```bash
docker attach web
```

`docker attach` connects to the container's main process, while `docker exec` starts a separate process.

---

# 9. Copy Files

## `docker cp`

Copies files between the host and a container.

Host → container:

```bash
docker cp file.txt web:/tmp/file.txt
```

Container → host:

```bash
docker cp web:/var/log/app.log .
```

Directory:

```bash
docker cp ./config web:/app/
```

---

# 10. Port & Networking

## `docker port`

Shows published port mappings.

```bash
docker port web
```

Example output may look like:

```text
80/tcp -> 0.0.0.0:8080
```

---

## `docker network ls`

Lists Docker networks.

```bash
docker network ls
```

---

## `docker network create`

Creates a custom network.

```bash
docker network create mynetwork
```

Specify driver:

```bash
docker network create --driver bridge mynetwork
```

---

## `docker network inspect`

Shows detailed network information.

```bash
docker network inspect mynetwork
```

Useful for checking:

- Connected containers
- Container IP addresses
- Gateway
- Subnet
- Network driver

---

## `docker network connect`

Connects a container to a network.

```bash
docker network connect mynetwork web
```

---

## `docker network disconnect`

Disconnects a container from a network.

```bash
docker network disconnect mynetwork web
```

---

## `docker network rm`

Removes a network.

```bash
docker network rm mynetwork
```

The network must not have active containers attached to it.

---

## `docker network prune`

Removes unused Docker networks.

```bash
docker network prune
```

---

# 11. Volumes & Storage

## `docker volume ls`

Lists Docker volumes.

```bash
docker volume ls
```

---

## `docker volume create`

Creates a named volume.

```bash
docker volume create mydata
```

---

## `docker volume inspect`

Displays volume information.

```bash
docker volume inspect mydata
```

---

## `docker volume rm`

Removes a volume.

```bash
docker volume rm mydata
```

---

## `docker volume prune`

Removes unused volumes.

```bash
docker volume prune
```

Be careful: volumes may contain important persistent data.

---

# 12. Bind Mounts

Mount a host directory into a container:

```bash
docker run -v /host/path:/container/path nginx
```

Example:

```bash
docker run -d \
  --name web \
  -v $(pwd)/html:/usr/share/nginx/html \
  nginx
```

Modern `--mount` syntax:

```bash
docker run -d \
  --mount type=bind,source=$(pwd)/html,target=/usr/share/nginx/html \
  nginx
```

---

# 13. Named Volumes

Run a container with a named volume:

```bash
docker run -d \
  --name database \
  -v dbdata:/var/lib/mysql \
  mysql
```

Using `--mount`:

```bash
docker run -d \
  --mount source=dbdata,target=/var/lib/mysql \
  mysql
```

---

# 14. Building Images

## `docker build`

Builds an image from a Dockerfile.

```bash
docker build -t myapp .
```

`-t` assigns a name/tag.

Example:

```bash
docker build -t myapp:v1 .
```

Build using a different Dockerfile:

```bash
docker build -f Dockerfile.dev -t myapp:dev .
```

---

## `docker buildx build`

Modern extended build command.

```bash
docker buildx build -t myapp:v1 .
```

Build for a specific platform:

```bash
docker buildx build --platform linux/amd64 -t myapp:v1 .
```

---

# 15. Dockerfile-Related Commands

Typical Dockerfile instructions are:

```dockerfile
FROM
RUN
COPY
ADD
WORKDIR
ENV
ARG
EXPOSE
CMD
ENTRYPOINT
USER
VOLUME
LABEL
HEALTHCHECK
```

Build:

```bash
docker build -t myimage .
```

---

# 16. Tagging Images

## `docker tag`

Creates another tag for an image.

```bash
docker tag myapp:v1 myapp:latest
```

For a registry:

```bash
docker tag myapp:v1 username/myapp:v1
```

---

# 17. Push & Pull from Registries

## `docker login`

Logs into a container registry.

```bash
docker login
```

Registry-specific:

```bash
docker login registry.example.com
```

---

## `docker logout`

Logs out.

```bash
docker logout
```

---

## `docker push`

Uploads an image to a registry.

```bash
docker push username/myapp:v1
```

---

## `docker pull`

Downloads an image.

```bash
docker pull username/myapp:v1
```

---

## `docker search`

Searches Docker Hub.

```bash
docker search nginx
```

---

# 18. Export & Import

## `docker save`

Saves an image to a tar archive.

```bash
docker save -o myimage.tar myapp:v1
```

Load it later:

```bash
docker load -i myimage.tar
```

Useful for transferring images between systems without a registry.

---

## `docker export`

Exports a container's filesystem.

```bash
docker export web -o web.tar
```

Import:

```bash
docker import web.tar myimage:imported
```

Important difference:

- `docker save` → image, preserves image layers/metadata
- `docker export` → container filesystem, does not preserve image history/layers

---

# 19. Docker Compose

## `docker compose up`

Creates and starts services defined in `compose.yaml`.

```bash
docker compose up
```

Background:

```bash
docker compose up -d
```

Rebuild images:

```bash
docker compose up -d --build
```

---

## `docker compose down`

Stops and removes Compose-created containers and networks.

```bash
docker compose down
```

Remove volumes too:

```bash
docker compose down -v
```

Be careful with `-v` because it can delete persistent data.

---

## `docker compose ps`

Lists Compose services.

```bash
docker compose ps
```

---

## `docker compose logs`

Shows service logs.

```bash
docker compose logs
```

Specific service:

```bash
docker compose logs web
```

Follow:

```bash
docker compose logs -f
```

---

## `docker compose exec`

Executes a command inside a Compose service.

```bash
docker compose exec web bash
```

---

## `docker compose build`

Builds Compose images.

```bash
docker compose build
```

---

## `docker compose pull`

Pulls service images.

```bash
docker compose pull
```

---

## `docker compose start`

Starts existing Compose containers.

```bash
docker compose start
```

---

## `docker compose stop`

Stops Compose containers without removing them.

```bash
docker compose stop
```

---

## `docker compose restart`

Restarts Compose services.

```bash
docker compose restart
```

---

## `docker compose config`

Validates and displays the resolved Compose configuration.

```bash
docker compose config
```

Very useful for troubleshooting Compose files.

---

# 20. System Cleanup

## `docker system df`

Shows Docker disk usage.

```bash
docker system df
```

---

## `docker system prune`

Removes unused Docker resources.

```bash
docker system prune
```

Remove unused images too:

```bash
docker system prune -a
```

Remove unused volumes too:

```bash
docker system prune -a --volumes
```

⚠️ Use carefully. Cleanup commands can remove resources you intended to keep.

---

# 21. Container Resource Limits

## CPU Limit

```bash
docker run --cpus="1.0" nginx
```

Limits the container to approximately one CPU.

---

## Memory Limit

```bash
docker run --memory="512m" nginx
```

---

## CPU + Memory

```bash
docker run \
  --cpus="1.0" \
  --memory="512m" \
  nginx
```

---

# 22. Environment Variables

Set a variable:

```bash
docker run -e APP_ENV=production myapp
```

Multiple variables:

```bash
docker run \
  -e APP_ENV=production \
  -e PORT=8080 \
  myapp
```

Using an environment file:

```bash
docker run --env-file .env myapp
```

---

# 23. Container Restart Policies

Restart automatically:

```bash
docker run --restart unless-stopped nginx
```

Common policies:

```text
no
on-failure
always
unless-stopped
```

Example:

```bash
docker run -d --restart unless-stopped nginx
```

---

# 24. Health Checks

Inspect health status:

```bash
docker inspect --format='{{.State.Health.Status}}' web
```

Dockerfile example:

```dockerfile
HEALTHCHECK CMD curl -f http://localhost/ || exit 1
```

Compose can also define health checks.

---

# 25. Security-Related Commands

## Run as Non-Root User

```bash
docker run --user 1000:1000 nginx
```

---

## Read-Only Filesystem

```bash
docker run --read-only nginx
```

---

## Drop Linux Capabilities

Example:

```bash
docker run --cap-drop ALL nginx
```

Add only required capability:

```bash
docker run --cap-drop ALL --cap-add NET_BIND_SERVICE nginx
```

---

## Privileged Container

```bash
docker run --privileged image
```

⚠️ Avoid using `--privileged` unless you specifically understand why it is required. It significantly reduces container isolation.

---

# 26. Useful Troubleshooting Commands

### Check running containers

```bash
docker ps
```

### Check all containers

```bash
docker ps -a
```

### Check logs

```bash
docker logs CONTAINER
```

### Follow logs

```bash
docker logs -f CONTAINER
```

### Inspect container

```bash
docker inspect CONTAINER
```

### Enter container

```bash
docker exec -it CONTAINER sh
```

### Check processes

```bash
docker top CONTAINER
```

### Check resource usage

```bash
docker stats
```

### Check ports

```bash
docker port CONTAINER
```

### Check network

```bash
docker network inspect NETWORK
```

### Check disk usage

```bash
docker system df
```

---

# 27. Useful Command Patterns

## Run a temporary container

```bash
docker run --rm -it ubuntu bash
```

The container is automatically removed after it exits.

---

## Run in background

```bash
docker run -d nginx
```

---

## Give container a name

```bash
docker run --name myweb nginx
```

---

## Map a port

```bash
docker run -p 8080:80 nginx
```

Format:

```text
HOST_PORT:CONTAINER_PORT
```

---

## Map to a specific host interface

```bash
docker run -p 127.0.0.1:8080:80 nginx
```

---

## Connect container to custom network

```bash
docker run -d --network mynetwork --name web nginx
```

---

## Combine common options

```bash
docker run -d \
  --name web \
  --network mynetwork \
  -p 8080:80 \
  --restart unless-stopped \
  nginx
```

---

# 28. Most Important Commands to Memorize First

If you're just starting Docker, focus on these first:

```text
docker pull
docker images
docker run
docker ps
docker ps -a
docker start
docker stop
docker restart
docker rm
docker exec
docker logs
docker inspect
docker build
docker rmi
docker tag
docker push
docker network ls
docker network create
docker network inspect
docker network connect
docker volume ls
docker volume create
docker volume inspect
docker compose up
docker compose down
docker compose ps
docker compose logs
docker compose exec
docker stats
docker system df
```

---

# 29. Recommended Learning Order

```text
Docker Fundamentals
        ↓
Images
        ↓
Containers
        ↓
docker run / ps / exec / logs / inspect
        ↓
Dockerfile
        ↓
Build & Tag Images
        ↓
Docker Networking
        ↓
Docker Volumes
        ↓
Docker Compose
        ↓
Troubleshooting
        ↓
Docker Security
        ↓
Registries
        ↓
CI/CD
        ↓
Kubernetes
```

## Quick Rule

Don't try to memorize every command.

Understand:

**Image → Container → Network → Storage → Compose → Security**

Then use `docker --help` and `docker <command> --help` whenever you forget syntax.
