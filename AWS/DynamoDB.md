# DynamoDB — AWS NoSQL Database

## What is it?
- DynamoDB is a **fully managed NoSQL database** by AWS
- Stores data as **key-value pairs** or **documents** (JSON-like)
- No servers to manage — AWS handles scaling, backups, and availability
- Think of it like a **giant dictionary**: you look things up by a key and get back a value instantly
- Designed for **millisecond response times** at any scale

## When to Use DynamoDB
- You need **very fast reads/writes** at scale
- Your data doesn't need complex SQL joins
- You want **zero maintenance** — no DB servers to manage
- Use cases: user sessions, shopping carts, real-time leaderboards, IoT data, metadata storage

## DynamoDB vs Traditional SQL (e.g., PostgreSQL)
| Feature | DynamoDB | PostgreSQL |
|---------|----------|------------|
| Type | NoSQL (key-value/document) | Relational (SQL) |
| Schema | Flexible (no fixed columns) | Fixed schema |
| Joins | ❌ Not supported | ✅ Yes |
| Scaling | Automatic (horizontal) | Manual (vertical) |
| Queries | By primary key (fast) | Any column (flexible) |
| Best for | High-speed, simple lookups | Complex queries, relations |

---

## Key Concepts

### Table
- Where your data lives — like a spreadsheet, but schema-less
- Each table has items (rows) and attributes (columns)

### Item
- A single record in a table (like a row in SQL)
- Each item is a JSON-like object

### Primary Key — Required for every table
Two types:
- **Partition Key only**: A single unique field (e.g., `user_id`)
- **Partition Key + Sort Key**: Two fields combined (e.g., `user_id` + `timestamp`) — lets you store multiple items per user

### Attributes
- The fields/columns of an item
- DynamoDB is **schema-less** — different items can have different attributes

### Index
- **GSI (Global Secondary Index)**: Query by a non-primary-key attribute
- **LSI (Local Secondary Index)**: Alternate sort key on the same partition

---

## Data Model Example

```
Table: Orders

Item 1:
{
  "user_id": "user-123",        ← Partition Key
  "order_id": "ord-001",        ← Sort Key
  "status": "shipped",
  "total": 49.99,
  "items": ["shoe", "hat"]
}

Item 2:
{
  "user_id": "user-123",        ← Same user, different order
  "order_id": "ord-002",
  "status": "pending",
  "total": 15.00
}
```

---

## Basic Operations

### Python (boto3)

```python
import boto3

dynamodb = boto3.resource('dynamodb', region_name='us-east-1')
table = dynamodb.Table('Orders')

# ✅ PUT — Create or replace an item
table.put_item(Item={
    'user_id': 'user-123',
    'order_id': 'ord-001',
    'status': 'pending',
    'total': 49.99
})

# ✅ GET — Fetch a specific item by primary key
response = table.get_item(Key={
    'user_id': 'user-123',
    'order_id': 'ord-001'
})
item = response.get('Item')
print(item)

# ✅ UPDATE — Change specific attributes
table.update_item(
    Key={'user_id': 'user-123', 'order_id': 'ord-001'},
    UpdateExpression='SET #s = :new_status',
    ExpressionAttributeNames={'#s': 'status'},
    ExpressionAttributeValues={':new_status': 'shipped'}
)

# ✅ DELETE — Remove an item
table.delete_item(Key={
    'user_id': 'user-123',
    'order_id': 'ord-001'
})

# ✅ QUERY — Get all orders for a user (uses partition key)
response = table.query(
    KeyConditionExpression='user_id = :uid',
    ExpressionAttributeValues={':uid': 'user-123'}
)
orders = response['Items']

# ✅ SCAN — Goes through entire table (slow, avoid on large tables)
response = table.scan(
    FilterExpression='status = :s',
    ExpressionAttributeValues={':s': 'pending'}
)
```

---

## Read & Write Capacity

Two billing modes:

### On-Demand (recommended to start)
- Pay per request — no planning needed
- AWS auto-scales instantly
- More expensive per request but zero waste

### Provisioned
- You set a fixed number of read/write units per second
- Cheaper if traffic is predictable
- Can cause throttling if you go over the limit

---

## DynamoDB Streams
- A log of all changes (inserts, updates, deletes) in real time
- Use with **Lambda** to trigger actions on data changes

```
Data changes in DynamoDB
        ↓
DynamoDB Stream (event log)
        ↓
Lambda function triggered
        ↓
Send notification / update cache / sync elsewhere
```

---

## TTL — Time to Live
- Set an expiry timestamp on items — DynamoDB auto-deletes them
- Great for sessions, caches, temporary data

```python
import time

table.put_item(Item={
    'session_id': 'sess-abc',
    'user_id': 'user-123',
    'ttl': int(time.time()) + 3600  # Expires in 1 hour
})
# DynamoDB will auto-delete this item after 1 hour
```

---

## Good to Know
- **Query** is fast (uses index) — always prefer over Scan
- **Scan** reads the whole table — slow and expensive on large tables
- Items can be max **400 KB** in size
- DynamoDB is **eventually consistent** by default (can request strong consistency)
- Use **DynamoDB Local** for testing without AWS charges
- Free tier: 25 GB storage + 25 read/write capacity units forever
- Works seamlessly with **Lambda**, **API Gateway**, **AppSync** for serverless apps
