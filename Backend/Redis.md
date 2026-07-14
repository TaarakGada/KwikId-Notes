# Redis

## What is it?
- Redis is an **in-memory data store** — it stores data in RAM, making it extremely fast
- Used as a **cache**, **session store**, **message broker**, or **real-time data store**
- Think of it like a super-fast scratchpad next to your database
- Default port: **6379**

## Why Use Redis?
- **Speed**: Data in RAM means sub-millisecond reads/writes
- **Cache**: Store results of expensive DB queries — serve from Redis instead
- **Sessions**: Store user session data that expires automatically
- **Rate Limiting**: Track request counts per user/IP efficiently
- **Pub/Sub**: Simple messaging between services

## Data Types
| Type | Example Use |
|------|-------------|
| String | Cache a value, counter |
| List | Task queues, timelines |
| Set | Unique visitors, tags |
| Hash | Store object fields (user profile) |
| Sorted Set | Leaderboards, ranked data |
| TTL | Any key with expiry |

## Basic Commands
```bash
# Start Redis CLI
redis-cli

# Set a key
SET name "Alice"

# Get a key
GET name

# Set with expiry (TTL) - expires in 60 seconds
SETEX session:123 60 "user-data"

# Check time to live
TTL session:123

# Delete a key
DEL name

# Check if key exists
EXISTS name

# Increment a counter
INCR page_views

# List operations
LPUSH tasks "task1"   # Add to left
RPUSH tasks "task2"   # Add to right
LPOP tasks            # Remove from left
LRANGE tasks 0 -1     # Get all
```

## Python Example (redis-py)
```python
import redis

r = redis.Redis(host='localhost', port=6379, db=0)

# Cache pattern
def get_user(user_id):
    cached = r.get(f'user:{user_id}')
    if cached:
        return json.loads(cached)  # Return from cache
    
    # If not in cache, fetch from DB
    user = db.query_user(user_id)
    
    # Store in cache for 5 minutes
    r.setex(f'user:{user_id}', 300, json.dumps(user))
    return user
```

## Good to Know
- Data is lost on restart by default — enable persistence (RDB/AOF) if needed
- Redis is single-threaded but incredibly fast
- **Redis Cluster** for horizontal scaling
- Use **connection pooling** in production
- Common pattern: Redis as L1 cache in front of a slower DB
