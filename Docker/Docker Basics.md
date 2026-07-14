# Docker — The Basics

## What is Docker?
- Docker lets you **package your app + everything it needs** (code, libraries, settings) into a single unit called a **container**
- The container runs the same way on any machine — no more "it works on my machine" problems
- Think of a container like a **lunchbox**: it has everything the meal needs, sealed and ready to open anywhere

## Key Concepts

### Image
- A **blueprint/template** for a container
- Read-only — you don't modify an image, you create containers from it
- Like a recipe: the recipe doesn't change, but you can cook many meals from it

### Container
- A **running instance** of an image
- Isolated from your system — has its own filesystem, network, processes
- Lightweight (shares your OS kernel, unlike VMs)

### Dockerfile
- A text file with instructions to build a Docker image
- Step-by-step: start from a base image, copy files, install deps, run commands

### Registry
- A place to store and share Docker images
- **Docker Hub** is the public registry (like GitHub but for images)

## Basic Commands
```bash
# Pull an image from Docker Hub
docker pull nginx

# Run a container
docker run -d -p 8080:80 nginx
# -d = run in background (detached)
# -p 8080:80 = map port 8080 on host to port 80 in container

# List running containers
docker ps

# Stop a container
docker stop <container_id>

# Remove a container
docker rm <container_id>

# List images
docker images

# Remove an image
docker rmi nginx

# View logs
docker logs <container_id>

# Enter a running container's shell
docker exec -it <container_id> bash
```

## Simple Dockerfile Example
```dockerfile
# Start from Python base image
FROM python:3.11-slim

# Set working directory inside container
WORKDIR /app

# Copy requirements first (for layer caching)
COPY requirements.txt .

# Install dependencies
RUN pip install -r requirements.txt

# Copy the rest of the app
COPY . .

# Run the app
CMD ["python", "app.py"]
```

```bash
# Build the image
docker build -t my-app:latest .

# Run it
docker run -p 5000:5000 my-app:latest
```

## Good to Know
- Containers are **ephemeral** — data is lost when they stop (unless you use volumes)
- Each `RUN` command in a Dockerfile creates a new **layer** — keep layers minimal
- Use `.dockerignore` to exclude files from the build (like `.git`, `node_modules`)
