# Docker Volume Mounts

## The Problem
- By default, data inside a container is **lost when the container stops**
- Volumes and mounts solve this by connecting container paths to persistent storage

## Types of Storage

### 1. Named Volumes (Recommended)
- Docker manages the storage location
- Data persists even if the container is deleted
- Best for databases, persistent app data
```bash
# Create a volume
docker volume create my-data

# Use it in a container
docker run -v my-data:/app/data my-app
# /app/data inside container is backed by the named volume

# List volumes
docker volume ls

# Inspect a volume
docker volume inspect my-data
```

### 2. Bind Mounts
- Mount a specific **folder from your host machine** into the container
- Changes on host are immediately reflected in container (great for development)
```bash
# Syntax: -v /host/path:/container/path
docker run -v /home/user/myapp:/app my-app

# Using $(pwd) for current directory
docker run -v $(pwd):/app my-app
```

### 3. tmpfs Mounts
- Stored only in memory — never written to disk
- Good for sensitive temporary data
```bash
docker run --tmpfs /tmp my-app
```

## In Docker Compose
```yaml
services:
  db:
    image: postgres:15
    volumes:
      - postgres_data:/var/lib/postgresql/data  # Named volume

  app:
    build: .
    volumes:
      - ./src:/app/src  # Bind mount (hot reload for dev)

volumes:
  postgres_data:  # Declare named volume here
```

## Quick Reference
| Type | Command | Use Case |
|------|---------|----------|
| Named Volume | `-v my-vol:/path` | DB data, persistent storage |
| Bind Mount | `-v /host/path:/path` | Dev hot reload, config files |
| tmpfs | `--tmpfs /path` | Temp/sensitive data |

## Good to Know
- `docker compose down` keeps volumes — use `docker compose down -v` to delete them too
- You can **backup a volume** by running a container that archives its contents
- Bind mounts are great for development but named volumes are better for production
