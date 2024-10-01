# Docker Cheatsheet
## Essential Docker Commands and Best Practices for Efficient Container Management

Docker has revolutionized the way developers build, ship, and run applications by offering a consistent environment across various stages of development. It enables applications to run in isolated containers, ensuring that the software behaves the same, regardless of where it is deployed. This efficiency and reliability make Docker an essential tool for developers, DevOps engineers, and system administrators.

This cheatsheet is designed to provide you with a quick reference to the most commonly used Docker commands and configurations. Whether you're just getting started with Docker or need a refresher on specific commands, this guide will help you efficiently navigate your Docker workflow.

## Understanding Docker

Docker is an open-source platform that automates the deployment of applications inside lightweight, portable containers. Unlike traditional virtual machines, Docker containers share the host system's kernel but run in isolated environments, making them much more efficient. Docker consists of several key components:

- **Docker Engine**: The core service that manages containers.
- **Docker Images**: Read-only templates used to create containers.
- **Docker Containers**: The running instances of Docker images.
- **Docker Hub**: A cloud-based registry where Docker images can be stored and shared.

Docker's architecture and ease of use make it a preferred choice for modern application development, especially in microservices architectures.

## Basic Docker Commands

Understanding the basic Docker commands is essential for effective container management. Below are some fundamental commands you'll use frequently.

### Docker Setup and Version Check

Check Docker version:
```
docker --version
```

Get Docker system information:
```
docker info
```

### Working with Docker Images

Pull an image from Docker Hub:
```
docker pull <image_name>
```
Example:
```
docker pull nginx
```

List all Docker images:
```
docker images
```

Remove an image:
```
docker rmi <image_name>
```

### Managing Docker Containers

Run a Docker container:
```
docker run <image_name>
```
Example:
```
docker run nginx
```

List running containers:
```
docker ps
```

Stop a running container:
```
docker stop <container-id>
```

Remove a container:
```
docker rm <container-id>
```

### Building Docker Images

Build an image from a Dockerfile:
```
docker build -t <image_name> .
```

Tag an image:
```
docker tag <image_id> <repository>/<image_name>:<tag>
```

## Advanced Docker Commands

Once you're comfortable with the basics, it's time to dive into more advanced Docker commands that help manage networking, data, and orchestrating multiple containers.

### Networking in Docker

List Docker networks:
```
docker network ls
```

Create a new network:
```
docker network create <network_name>
```

Connect a container to a network:
```
docker network connect <network_name> <container_name>
```

### Volumes and Data Management

Create a Docker volume:
```
docker volume create <volume_name>
```

Mount a volume to a container:
```
docker run -v <volume_name>:/path/in/container <image_name>
```
Example:
```
docker run -v my_volume:/var/www/html nginx
```

### Docker Compose

Sample docker-compose.yml file:
```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "80:80"
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
```

Start services with Docker Compose:
```
docker-compose up
```

Stop and remove services:
```
docker-compose down
```

## Docker Configuration Files

Understanding Docker configuration files, such as Dockerfiles and docker-compose.yml, is crucial for building and deploying Docker containers efficiently.

### Dockerfile

A Dockerfile is a script containing instructions to build a Docker image. Here's an example of a basic Dockerfile:

```dockerfile
# Use an official Python runtime as a parent image
FROM python:3.8-slim-buster

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Run app.py when the container launches
CMD ["python", "app.py"]
```

Key Commands in the Dockerfile:

- `FROM`: Specifies the base image.
- `WORKDIR`: Sets the working directory inside the container.
- `COPY`: Copies files from the host system into the container.
- `RUN`: Executes commands in the container, such as installing dependencies.
- `EXPOSE`: Specifies the port that the container will use.
- `CMD`: Provides the command to run the application.

### Docker-Compose File

The docker-compose.yml file is used to define and run multi-container Docker applications. Here's a basic example:

```yaml
version: '3'
services:
  app:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/code
    networks:
      - app-network
  redis:
    image: "redis:alpine"
    networks:
      - app-network

networks:
  app-network:
    driver: bridge
```

This configuration defines two services, app and redis, that share a network and exposes the app's port to the host system.

## Tips and Best Practices

To make the most out of Docker, it's important to follow certain best practices and tips that can enhance security, optimize performance, and ensure smoother integration in development workflows.

### Security Best Practices

1. Run containers as a non-root user:
   ```dockerfile
   RUN useradd -m appuser
   USER appuser
   ```

2. Use trusted images: Always use official or verified images from Docker Hub.

3. Limit container capabilities: Use Docker's built-in security options to limit the capabilities of your containers, such as using --cap-drop to drop unnecessary privileges.

### Optimizing Docker Images

1. Minimize the number of layers:
   ```dockerfile
   RUN apt-get update && apt-get install -y \
       package1 \
       package2
   ```

2. Use multi-stage builds:
   ```dockerfile
   FROM golang:1.16 AS builder
   WORKDIR /app
   COPY . .
   RUN go build -o myapp

   FROM alpine:latest
   WORKDIR /app
   COPY --from=builder /app/myapp .
   CMD ["./myapp"]
   ```

### Using Docker in CI/CD Pipelines

1. Integrate Docker with CI/CD tools: Docker can be easily integrated into CI/CD pipelines using tools like Jenkins, GitLab CI, or GitHub Actions.

2. Use Docker cache: Enable Docker caching in your CI/CD pipelines to speed up build times by reusing layers from previous builds.
