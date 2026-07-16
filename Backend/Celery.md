# Celery

## What is it?
- Celery is a **distributed task queue** for Python
- Lets you run tasks **in the background** (asynchronously) — so your web server doesn't block waiting
- Think of it as a restaurant: the waiter (web server) takes your order and passes it to the kitchen (Celery worker) — you don't wait at the counter

## Why Use Celery?
- Send emails without making users wait
- Process large files/images in background
- Run periodic tasks (like a cron job)
- Distribute heavy work across multiple workers

## Components
- **Celery App**: The task definition and configuration
- **Broker**: Carries messages between app and workers (Redis or RabbitMQ)
- **Worker**: Process that picks up and executes tasks
- **Result Backend**: Where task results are stored (Redis, DB)

## Architecture
```
Web Request
    ↓
Django/FastAPI  → sends task → [Broker: Redis/RabbitMQ]
                                        ↓
                               [Celery Worker]
                                        ↓  stores result
                               [Result Backend: Redis]
```

## Installation
```bash
pip install celery redis
```

## Basic Example
```python
# tasks.py
from celery import Celery

# Connect to Redis as broker
app = Celery('myapp', broker='redis://localhost:6379/0')

@app.task
def send_email(recipient, subject, body):
    # This runs in a background worker
    email_service.send(recipient, subject, body)
    return 'Email sent!'

@app.task
def process_image(image_path):
    # Heavy processing without blocking web request
    result = run_ml_model(image_path)
    return result
```

```python
# In your web handler
from tasks import send_email

def register_user(request):
    user = create_user(request.data)
    
    # Fire and forget — doesn't block
    send_email.delay(user.email, 'Welcome!', 'Thanks for joining!')
    
    return {'message': 'User created!'}
```

## Starting a Worker
```bash
# Start a Celery worker
celery -A tasks worker --loglevel=info

# Start multiple workers
celery -A tasks worker --concurrency=4
```

## Periodic Tasks (Beat Scheduler)
```python
from celery.schedules import crontab

app.conf.beat_schedule = {
    'cleanup-every-midnight': {
        'task': 'tasks.cleanup_old_records',
        'schedule': crontab(hour=0, minute=0),  # Every day at midnight
    },
    'heartbeat-every-minute': {
        'task': 'tasks.heartbeat',
        'schedule': 60.0,  # Every 60 seconds
    }
}
```

```bash
# Start the beat scheduler alongside workers
celery -A tasks beat --loglevel=info
```

## Good to Know
- Celery needs a **broker** (Redis or RabbitMQ) to work
- Tasks should be **idempotent** — safe to run multiple times in case of failure
- Use `apply_async` for more control (delay, retries)
- **Flower** is a web UI to monitor Celery workers
- In production, run workers with **Supervisor** or **systemd**

---

## Celery in KwikID

Inside the KwikID APIs (`kwikid-admin-api` and `kwikid-agent-api`), Celery runs asynchronous background tasks:
*   **Video KYC Video Encoder**: In `kwikid-agent-api`, Celery handles heavy encoding jobs (e.g. `celery_video_maker.py` and `run_video_encode.sh`) to join or format video feeds.
*   **Celery Config & App**: Initialized in `celery_app.py` in the root of the api directories.
*   **Docker Integration**: Separated into specific containers during development and production (`Dockerfile.celery` and `docker-compose-celery.yml`).
