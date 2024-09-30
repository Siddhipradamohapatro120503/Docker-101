# Docker: From Basic to Advanced

## Table of Contents
1. [Introduction to Docker](#introduction-to-docker)
2. [Docker Architecture](#docker-architecture)
3. [Installation](#installation)
4. [Basic Docker Commands](#basic-docker-commands)
5. [Working with Docker Images](#working-with-docker-images)
6. [Managing Docker Containers](#managing-docker-containers)
7. [Docker Networking](#docker-networking)
8. [Docker Volumes](#docker-volumes)
9. [Dockerfile](#dockerfile)
10. [Docker Compose](#docker-compose)
11. [Docker Swarm](#docker-swarm)
12. [Advanced Docker Concepts](#advanced-docker-concepts)

## 1. Introduction to Docker

Docker is a platform for developing, shipping, and running applications in containers. Containers are lightweight, portable, and consistent environments that package an application and its dependencies.

Key benefits of Docker:
- Consistency across development, testing, and production environments
- Isolation of applications and their dependencies
- Efficient resource utilization
- Rapid deployment and scaling

## 2. Docker Architecture

Docker uses a client-server architecture:

- Docker Client: The command-line interface (CLI) that allows users to interact with Docker.
- Docker Host: The machine running the Docker daemon (dockerd).
- Docker Registry: A repository for Docker images (e.g., Docker Hub).

Components:
- Images: Read-only templates for creating containers.
- Containers: Runnable instances of images.
- Volumes: Persistent data storage.
- Networks: Communication between containers and the outside world.

## 3. Installation

Visit the official Docker website (https://www.docker.com) and follow the installation instructions for your operating system.

Verify installation:
```bash
docker --version
docker-compose --version
```

## 4. Basic Docker Commands

```bash
# List Docker CLI commands
docker

# Get help on a specific command
docker <command> --help

# Display Docker version and info
docker --version
docker version
docker info

# Log in to a Docker registry
docker login

# Log out from a Docker registry
docker logout
```

## 5. Working with Docker Images

```bash
# List images
docker images

# Pull an image from a registry
docker pull <image_name>:<tag>

# Build an image from a Dockerfile
docker build -t <image_name>:<tag> <path_to_dockerfile>

# Push an image to a registry
docker push <image_name>:<tag>

# Remove an image
docker rmi <image_name>:<tag>

# Search for images on Docker Hub
docker search <keyword>
```

## 6. Managing Docker Containers

```bash
# Run a container
docker run <image_name>:<tag>

# Run a container in detached mode
docker run -d <image_name>:<tag>

# Run a container with port mapping
docker run -p <host_port>:<container_port> <image_name>:<tag>

# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# Stop a running container
docker stop <container_id>

# Start a stopped container
docker start <container_id>

# Remove a container
docker rm <container_id>

# Remove all stopped containers
docker container prune

# Execute a command in a running container
docker exec -it <container_id> <command>

# View container logs
docker logs <container_id>

# Inspect container details
docker inspect <container_id>
```

## 7. Docker Networking

```bash
# List networks
docker network ls

# Create a network
docker network create <network_name>

# Connect a container to a network
docker network connect <network_name> <container_id>

# Disconnect a container from a network
docker network disconnect <network_name> <container_id>

# Remove a network
docker network rm <network_name>

# Inspect a network
docker network inspect <network_name>
```

## 8. Docker Volumes

```bash
# List volumes
docker volume ls

# Create a volume
docker volume create <volume_name>

# Remove a volume
docker volume rm <volume_name>

# Inspect a volume
docker volume inspect <volume_name>

# Mount a volume to a container
docker run -v <volume_name>:<container_path> <image_name>:<tag>
```

## 9. Dockerfile

A Dockerfile is a text file containing instructions to build a Docker image.

Example Dockerfile:

```dockerfile
FROM ubuntu:20.04
RUN apt-get update && apt-get install -y nginx
COPY ./app /var/www/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

Common Dockerfile instructions:
- FROM: Specifies the base image
- RUN: Executes commands in the container
- COPY: Copies files from host to container
- ADD: Copies files and can also download from URLs and extract archives
- EXPOSE: Informs Docker that the container listens on specified ports
- ENV: Sets environment variables
- WORKDIR: Sets the working directory for subsequent instructions
- CMD: Specifies the command to run when the container starts
- ENTRYPOINT: Configures the container to run as an executable

## 10. Docker Compose

Docker Compose is a tool for defining and running multi-container Docker applications.

Example docker-compose.yml:

```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
  redis:
    image: "redis:alpine"
```

Docker Compose commands:

```bash
# Start services
docker-compose up

# Start services in detached mode
docker-compose up -d

# Stop services
docker-compose down

# View logs
docker-compose logs

# List containers
docker-compose ps

# Execute a command in a service
docker-compose exec <service_name> <command>
```

## 11. Docker Swarm

Docker Swarm is Docker's native clustering and orchestration solution.

```bash
# Initialize a swarm
docker swarm init

# Join a swarm as a worker
docker swarm join --token <worker_token> <manager_ip>:<port>

# List nodes in the swarm
docker node ls

# Create a service
docker service create --name <service_name> <image_name>:<tag>

# List services
docker service ls

# Scale a service
docker service scale <service_name>=<number_of_replicas>

# Remove a service
docker service rm <service_name>
```

## 12. Advanced Docker Concepts

### Multi-stage builds
Use multiple FROM statements in your Dockerfile to create smaller, more efficient images.

```dockerfile
FROM node:14 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### Health checks
Add health checks to your containers to ensure they're running correctly.

```dockerfile
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
  CMD curl -f http://localhost/ || exit 1
```

### Docker security best practices
- Use official base images
- Scan images for vulnerabilities
- Run containers with least privileges
- Use secrets management
- Implement network segmentation
- Regularly update and patch containers

### Monitoring and logging
Use tools like Prometheus, Grafana, and ELK stack for monitoring and logging Docker environments.

```bash
# View container resource usage
docker stats

# Enable logging drivers
docker run --log-driver=fluentd <image_name>:<tag>
```

This guide covers the basics to advanced concepts of Docker. As you progress, you'll discover more advanced topics and best practices for containerization and orchestration.
