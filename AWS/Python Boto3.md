# Python Boto3 — AWS SDK for Python

## What is it?
- **Boto3** is the official Python library to interact with AWS services
- Lets you control EC2, S3, SQS, Lambda, and 200+ services from Python code
- Think of it as a remote control for AWS, but in Python

## Installation
```bash
pip install boto3
```

## Setup / Authentication
```bash
# Configure AWS credentials
aws configure
# Enter: Access Key ID, Secret Access Key, Region, Output format
```

Or set environment variables:
```bash
export AWS_ACCESS_KEY_ID=your_key
export AWS_SECRET_ACCESS_KEY=your_secret
export AWS_DEFAULT_REGION=us-east-1
```

## Two Main Ways to Use Boto3

### 1. Client — Low-level API
```python
import boto3

# Direct API calls, returns raw JSON
s3_client = boto3.client('s3')
response = s3_client.list_buckets()
print(response['Buckets'])
```

### 2. Resource — High-level OOP interface
```python
# More Pythonic, object-oriented
s3 = boto3.resource('s3')
bucket = s3.Bucket('my-bucket')
for obj in bucket.objects.all():
    print(obj.key)
```

## Common Examples

### Upload a file to S3
```python
s3 = boto3.client('s3')
s3.upload_file('local_file.txt', 'my-bucket', 's3_file.txt')
```

### Start/Stop EC2 instance
```python
ec2 = boto3.client('ec2')
ec2.start_instances(InstanceIds=['i-1234567890abcdef0'])
ec2.stop_instances(InstanceIds=['i-1234567890abcdef0'])
```

### Send SQS message
```python
sqs = boto3.client('sqs')
sqs.send_message(
    QueueUrl='https://sqs.us-east-1.amazonaws.com/123/my-queue',
    MessageBody='Hello!'
)
```

## Good to Know
- Always specify a **region** when creating clients
- Use **IAM roles** in production — avoid hardcoding credentials
- Use **Paginator** for large result sets (e.g., listing 1000s of S3 objects)
- Errors are raised as exceptions (`botocore.exceptions.ClientError`)

```python
try:
    s3.download_file('bucket', 'key', 'local_file')
except Exception as e:
    print(f"Error: {e}")
```
