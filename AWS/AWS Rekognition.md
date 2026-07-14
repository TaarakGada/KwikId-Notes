# AWS Rekognition

## What is it?
- A cloud-based **computer vision** service
- Analyzes images and videos to detect faces, objects, text, and more
- No ML knowledge needed — just call the API

## What Can It Do?
- **Face Detection**: Find faces in images, estimate age, emotions
- **Face Recognition**: Match a face against a database of known faces
- **Object Detection**: Identify objects (car, dog, tree) in images
- **Text in Images (OCR)**: Read text from photos
- **Content Moderation**: Detect inappropriate content
- **Celebrity Recognition**: Identify famous people
- **Activity Detection**: In videos (e.g., person falling, running)

## Common Use Cases
- Identity verification (compare selfie vs. ID photo)
- Security camera monitoring
- Automatic photo tagging
- Detecting unsafe content in user uploads

## Python Example (boto3)
```python
import boto3

client = boto3.client('rekognition', region_name='us-east-1')

# Detect faces in an image stored in S3
response = client.detect_faces(
    Image={
        'S3Object': {
            'Bucket': 'my-bucket',
            'Name': 'photo.jpg'
        }
    },
    Attributes=['ALL']
)

for face in response['FaceDetails']:
    print(f"Age range: {face['AgeRange']}")
    print(f"Emotions: {face['Emotions']}")
```

## Face Comparison Example
```python
response = client.compare_faces(
    SourceImage={'S3Object': {'Bucket': 'bucket', 'Name': 'selfie.jpg'}},
    TargetImage={'S3Object': {'Bucket': 'bucket', 'Name': 'id-photo.jpg'}},
    SimilarityThreshold=90  # Only match if 90%+ similar
)

if response['FaceMatches']:
    print("Match found!")
```

## Good to Know
- Pricing is per image/minute of video analyzed
- Works best with well-lit, forward-facing images
- Store known faces in a **Collection** (Rekognition's face database)
- Not 100% accurate — always have human review for high-stakes decisions
