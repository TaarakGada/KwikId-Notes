# Docker Compose

## What is it?
- Docker Compose lets you **define and run multi-container apps** using a single YAML file
- Instead of running multiple `docker run` commands, you write one `docker-compose.yml` and start everything with one command
- Think of it as a **conductor** for an orchestra of containers

## When Do You Need It?
- Your app has multiple services: backend + database + cache + worker
- You want all of them to start together with proper networking

## Basic `docker-compose.yml` Example
```yaml
version: '3.8'

services:
  web:
    build: .          # Build from local Dockerfile
    ports:
      - "5000:5000"   # host:container
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/mydb
    depends_on:
      - db             # Start after db is ready

  db:
    image: postgres:15 # Use official image
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: mydb
    volumes:
      - postgres_data:/var/lib/postgresql/data  # Persist DB data

volumes:
  postgres_data:       # Named volume
```

## Key Commands
```bash
# Start all services (in background)
docker compose up -d

# Stop all services
docker compose down

# View logs for all services
docker compose logs -f

# View logs for a specific service
docker compose logs -f web

# Rebuild images and restart
docker compose up -d --build

# List running services
docker compose ps

# Run a command inside a service container
docker compose exec web bash
```

## Key Concepts
- **Services**: Each container is a service (web, db, redis, worker)
- **Networks**: All services in a compose file can talk to each other by service name (`db`, `redis`, etc.)
- **Volumes**: Persist data across container restarts
- **depends_on**: Control startup order
- **environment**: Pass environment variables to containers

## Good to Know
- Services refer to each other by **service name** (not IP) — e.g., your app connects to `db:5432` not `localhost:5432`
- Use `.env` file to store sensitive values, reference in compose with `${VAR_NAME}`
- `docker compose down -v` removes volumes too (data deleted!)
