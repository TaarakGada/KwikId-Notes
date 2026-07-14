# LiveKit

## What is it?
- LiveKit is an **open-source real-time communication platform** built on top of WebRTC
- It handles all the complex parts of WebRTC (signaling, TURN, SFU, scaling) for you
- Think of it as: WebRTC made easy for developers

## Why Use LiveKit Instead of Raw WebRTC?
| Raw WebRTC | LiveKit |
|-----------|--------|
| You build signaling server | Built-in |
| You manage TURN/STUN | Managed |
| Complex multi-party video | Easy room API |
| Hard to scale | Built for scale |

## Key Concepts

### Room
- A virtual space where participants connect
- Each room has a unique name
- Participants can publish (send) and subscribe (receive) media

### Participant
- A user connected to a room
- Can be a regular user or a server-side participant

### Track
- A single stream of media (video, audio, or data)
- Participants publish tracks (their camera/mic) and subscribe to others' tracks

### SFU (Selective Forwarding Unit)
- LiveKit's server receives all streams and selectively forwards them to participants
- Much more efficient than pure P2P for group calls

### Token
- JWT-based auth — every participant needs a token to join a room
- Server generates tokens, never expose your API secret to clients

## Architecture
```
[Your Backend] → generates token → [Client]
[Client] → joins room with token → [LiveKit Server]
[LiveKit Server] ↔ manages all participants and media ↔ [Other Clients]
```

## Quick Start (Python SDK)
```python
from livekit import api
import asyncio

# Generate a token for a participant
token = api.AccessToken('API_KEY', 'API_SECRET')
token.with_identity('user-123')
token.with_name('Alice')
token.with_grants(api.VideoGrants(room_join=True, room='my-room'))

print(token.to_jwt())
```

## Quick Start (JavaScript Client)
```javascript
import { Room } from 'livekit-client';

const room = new Room();

await room.connect('wss://your-livekit-server.com', token);

// Publish camera/mic
const { localParticipant } = room;
await localParticipant.setCameraEnabled(true);
await localParticipant.setMicrophoneEnabled(true);

// Handle other participants
room.on('trackSubscribed', (track, publication, participant) => {
  if (track.kind === 'video') {
    const videoElement = track.attach();
    document.body.appendChild(videoElement);
  }
});
```

## Good to Know
- LiveKit Cloud offers a hosted version — no server management needed
- Self-hostable (Docker image available)
- Supports **screen sharing**, **data channels**, **recording**
- Has SDKs for: JavaScript, Python, Go, iOS, Android, Flutter, Unity
- Uses WebRTC under the hood — compatible with all WebRTC-capable browsers
- Great for: video conferencing, live streaming, gaming voice chat
