# SNS — Simple Notification Service

## What is it?
- SNS is a **pub/sub (publish-subscribe) messaging service**
- One publisher sends a message to a **topic**, and all subscribers get it
- Think of it like a **radio station**: one broadcast, many listeners

## SNS vs SQS
| Feature | SNS | SQS |
|---------|-----|-----|
| Pattern | Pub/Sub (push) | Queue (pull) |
| Receivers | Many (fan-out) | One worker at a time |
| Persistence | No (fire and forget) | Yes (message stored) |

## Common Use Cases
- Send email/SMS alerts when something happens (server crash, payment failed)
- Fan-out: one event triggers multiple SQS queues
- Mobile push notifications

## Key Concepts
- **Topic**: The channel publishers send to
- **Subscription**: An endpoint that wants to receive messages from a topic
- **Subscriber types**: Email, SMS, HTTP endpoint, Lambda, SQS

## Basic Flow
```
Publisher → [SNS Topic] → Subscriber 1 (Email)
                        → Subscriber 2 (SQS Queue)
                        → Subscriber 3 (Lambda)
```

## Python Example (boto3)
```python
import boto3

sns = boto3.client('sns', region_name='us-east-1')

# Publish a message
sns.publish(
    TopicArn='arn:aws:sns:us-east-1:123456789:my-topic',
    Message='Server is down!',
    Subject='Alert'
)
```

## Good to Know
- SNS does NOT store messages — if no subscribers, message is lost
- Pair SNS + SQS for reliable fan-out delivery
- Supports **message filtering** — subscribers can choose which messages they receive
