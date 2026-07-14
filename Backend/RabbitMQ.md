# RabbitMQ

## What is it?
- RabbitMQ is a **message broker** — software that receives, stores, and forwards messages between services
- Think of it like a **post office**: producers put messages in, consumers pick them up
- Great for connecting services that don't need to talk directly

## RabbitMQ vs Redis (as message broker)
| | RabbitMQ | Redis (Pub/Sub) |
|--|----------|-----------------|
| Message persistence | Yes | Limited |
| Routing flexibility | High | Low |
| Acknowledgments | Yes | No |
| Complex routing | Yes (exchanges) | No |
| Learning curve | Steeper | Simple |

## Key Concepts

### Producer
- Sends messages to RabbitMQ

### Queue
- Where messages are stored until consumed
- Consumers subscribe to a queue

### Consumer
- Reads and processes messages from a queue

### Exchange
- Routes messages from producer to the correct queue(s)
- Types: **Direct** (exact match), **Fanout** (all queues), **Topic** (pattern match)

## Basic Flow
```
Producer → Exchange → Queue → Consumer
```

## Python Example (pika library)
```python
import pika

# Connect
connection = pika.BlockingConnection(
    pika.ConnectionParameters('localhost')
)
channel = connection.channel()

# Producer — Send a message
channel.queue_declare(queue='task_queue', durable=True)
channel.basic_publish(
    exchange='',
    routing_key='task_queue',
    body='Process this image!',
    properties=pika.BasicProperties(delivery_mode=2)  # Make message persistent
)
print("Message sent!")
```

```python
# Consumer — Receive and process messages
def callback(ch, method, properties, body):
    print(f"Processing: {body.decode()}")
    # Acknowledge message after processing
    ch.basic_ack(delivery_tag=method.delivery_tag)

channel.basic_consume(queue='task_queue', on_message_callback=callback)
channel.start_consuming()
```

## Fanout (Broadcast to all queues)
```python
# Send to ALL consumers (like broadcasting an event)
channel.exchange_declare(exchange='logs', exchange_type='fanout')
channel.basic_publish(exchange='logs', routing_key='', body='System event!')
```

## Good to Know
- Messages can be made **persistent** (survive RabbitMQ restart)
- **Acknowledgments (ACK)**: Worker confirms message processed — prevents message loss
- Dead Letter Queue: Messages that fail go here for inspection
- Use **Celery** with RabbitMQ as the broker for easier task management
- RabbitMQ has a built-in web management UI on port `15672`
- Default credentials: `guest`/`guest` (change in production!)
