# Docker Networking

## What is it?
- Docker has its own internal networking system so containers can **talk to each other**
- By default, containers are isolated — networking must be explicitly configured

## Network Types

### Bridge (Default)
- Containers on the same bridge network can talk to each other
- Default network for standalone containers
```bash
# Containers on same bridge can ping each other by name
docker run --network my-bridge my-app
```

### Host
- Container shares the host machine's network directly
- No port mapping needed — container uses host ports directly
- Less isolation, but better performance
```bash
docker run --network host nginx
```

### None
- Container has no network access at all
- Useful for tasks that should be fully isolated

## Creating Custom Networks
```bash
# Create a network
docker network create my-network

# Run containers on that network
docker run -d --network my-network --name backend my-backend-app
docker run -d --network my-network --name frontend my-frontend-app

# Now frontend can reach backend using hostname "backend"
# e.g., http://backend:8000

# List all networks
docker network ls

# Inspect a network
docker network inspect my-network
```

## Networking in Docker Compose
- All services in a `docker-compose.yml` are automatically on the same network
- They can communicate by **service name**
```yaml
services:
  app:
    image: my-app
  redis:
    image: redis
# app can connect to redis at redis:6379
```

## Port Mapping
- Expose container ports to the outside world
```bash
# -p <host_port>:<container_port>
docker run -p 8080:80 nginx
# Visit localhost:8080 → hits nginx on port 80 inside container
```

## Good to Know
- Containers on **different networks cannot talk** to each other by default
- Use multiple networks for better isolation (frontend network, backend network)
- Container names act as DNS hostnames within a network
