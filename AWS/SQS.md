# SQS — Simple Queue Service

## What is it?
- SQS is a **message queue** — a holding area where one service drops messages and another picks them up
- Think of it like a **to-do list**: one person adds tasks, another picks them up one by one
- Helps decouple services — the sender doesn't wait for the receiver

## Why Use a Queue?
- If your backend gets 10,000 requests at once, you don't want to crash
- SQS holds all those requests, and workers process them at their own pace

## Types of Queues
- **Standard Queue**: Messages delivered at least once, order not guaranteed (high throughput)
- **FIFO Queue**: Messages delivered exactly once, in order (slower, strict ordering)

## Key Concepts
- **Message**: The data sent (JSON, string, etc.) — max 256 KB
- **Visibility Timeout**: After a consumer picks up a message, it's hidden from others for X seconds (prevents double processing)
- **Dead Letter Queue (DLQ)**: Where failed messages go after too many retries
- **Polling**: Consumers must *ask* the queue for messages (not pushed to them)

## Basic Flow
```
Producer → [SQS Queue] → Consumer (Worker)
```

## Python Example (boto3)
```python
import boto3

sqs = boto3.client('sqs', region_name='us-east-1')

# Send a message
sqs.send_message(
    QueueUrl='https://sqs.us-east-1.amazonaws.com/123/my-queue',
    MessageBody='Hello from producer!'
)

# Receive a message
response = sqs.receive_message(QueueUrl='https://sqs...')
messages = response.get('Messages', [])
for msg in messages:
    print(msg['Body'])
    # Delete after processing
    sqs.delete_message(QueueUrl='...', ReceiptHandle=msg['ReceiptHandle'])
```

## Good to Know
- SQS is pull-based (consumers poll for messages)
- Combine with **SNS** for fan-out (one message → many queues)
- Use **Lambda** to automatically trigger processing when a message arrives
