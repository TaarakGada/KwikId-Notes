# AWS Lambda

## What is it?
- Lambda lets you run **code without managing servers**
- You upload a function, AWS runs it when triggered, charges only for what runs
- Think of it like a **vending machine**: put in a coin (trigger), get a snack (function runs), machine turns off

## Key Concepts
- **Function**: Your code (Python, Node.js, Go, Java, etc.)
- **Trigger**: What starts the function (HTTP request, SQS message, S3 upload, schedule, etc.)
- **Event**: The input data passed to your function
- **Cold Start**: First run takes a bit longer (Lambda needs to spin up)
- **Timeout**: Max time your function can run (default 3s, max 15 min)
- **Memory**: You choose RAM (128MB–10GB) — more RAM = more CPU too

## Common Triggers
- API Gateway (HTTP requests)
- SQS (process queue messages)
- S3 (file uploaded)
- CloudWatch Events (scheduled like a cron job)
- SNS (notification received)

## Basic Python Lambda Function
```python
def lambda_handler(event, context):
    print("Event received:", event)
    
    name = event.get('name', 'World')
    return {
        'statusCode': 200,
        'body': f'Hello, {name}!'
    }
```

## Pricing
- You pay per **number of requests** + **duration** (GB-seconds)
- First 1 million requests/month are FREE

## Good to Know
- Lambda functions are **stateless** — no memory between runs
- Use **environment variables** for config (API keys, DB URLs)
- Package dependencies in a **Lambda Layer** or include them in your zip
- Max deployment size: 50MB (zipped), 250MB (unzipped)
- Great for event-driven architectures
