# Dockerfile vs Image vs Container

## The Analogy
| Concept | Real World Analogy |
|---------|-------------------|
| Dockerfile | Recipe / Blueprint |
| Docker Image | Baked cake (ready, but not served) |
| Docker Container | Slice of cake on a plate (running, in use) |

## Dockerfile
- A **text file** with build instructions
- Not runnable by itself
- Example:
```dockerfile
FROM node:18
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
CMD ["node", "server.js"]
```

## Docker Image
- Built FROM a Dockerfile using `docker build`
- **Immutable** — once built, it doesn't change
- Stored on disk (or pushed to a registry like Docker Hub)
- Can be shared/downloaded

```bash
# Build image from Dockerfile
docker build -t my-node-app:v1 .

# See all images
docker images
```

## Docker Container
- A **running instance** of a Docker Image
- You can run many containers from the same image
- Has its own isolated environment
- Can be started, stopped, paused, deleted

```bash
# Create and run a container from the image
docker run -d -p 3000:3000 my-node-app:v1

# Run multiple containers from same image
docker run -d -p 3001:3000 my-node-app:v1
docker run -d -p 3002:3000 my-node-app:v1
```

## Lifecycle Flow
```
Dockerfile
    ↓  docker build
Docker Image
    ↓  docker run
Docker Container (running)
    ↓  docker stop
Docker Container (stopped)
    ↓  docker rm
(deleted)
```

## Key Difference Summary
- **Dockerfile** = Instructions (static file)
- **Image** = Built artifact (static, shareable)
- **Container** = Live running process (dynamic)
