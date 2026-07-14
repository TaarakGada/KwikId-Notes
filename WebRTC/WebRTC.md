# WebRTC — Web Real-Time Communication

## What is it?
- WebRTC is a browser technology that enables **real-time audio, video, and data** sharing **directly between browsers** (peer-to-peer)
- No need for a central server to relay media — browsers talk directly to each other
- Used in: Google Meet, Zoom web, Discord (partially), video calls in apps

## Key Concepts

### Peer-to-Peer (P2P)
- After connection is established, data flows **directly** between two browsers
- The server is only needed to help them *find* each other (signaling)

### Signaling
- Before two peers can connect, they need to exchange connection info
- This is done through a **signaling server** (your backend — WebSocket or HTTP)
- WebRTC itself doesn't define how signaling works — you implement it

### SDP (Session Description Protocol)
- A text-based description of the media session (codecs, formats, etc.)
- Peers exchange **offer** and **answer** SDP via signaling

### ICE (Interactive Connectivity Establishment)
- The process of finding the best network path between two peers
- Tries local IPs → public IPs → TURN server
- See: ICE Candidates note

### TURN / STUN Servers
- Help peers discover their public IP (STUN) or relay traffic (TURN)
- Required when peers are behind firewalls/NAT
- See: TURN & STUN note

## Connection Flow
```
Peer A                Signaling Server              Peer B
  |                        |                          |
  |-- Create Offer ------->|                          |
  |                        |-- Forward Offer -------->|
  |                        |<-- Send Answer ----------|
  |<-- Forward Answer -----|                          |
  |                        |                          |
  |<========= ICE Candidates Exchanged =============>|
  |                                                   |
  |<============ Direct P2P Connection =============>|
```

## Basic Code Example (Browser)
```javascript
// Create a peer connection
const pc = new RTCPeerConnection({
  iceServers: [
    { urls: 'stun:stun.l.google.com:19302' }  // Google's free STUN server
  ]
});

// Get user's camera/mic
const stream = await navigator.mediaDevices.getUserMedia({ video: true, audio: true });

// Add tracks to the peer connection
stream.getTracks().forEach(track => pc.addTrack(track, stream));

// Create an offer (Caller side)
const offer = await pc.createOffer();
await pc.setLocalDescription(offer);

// Send offer to the other peer via your signaling server
signalingSocket.emit('offer', offer);

// Handle ICE candidates
pc.onicecandidate = (event) => {
  if (event.candidate) {
    signalingSocket.emit('ice-candidate', event.candidate);
  }
};
```

## What You Can Send
- **Video/Audio streams**: Camera, microphone, screen share
- **Data channels**: Any arbitrary data (text, files, game state)

## Good to Know
- WebRTC is built into all modern browsers — no plugins needed
- Works best on same network; TURN server needed for cross-network connections
- For production, use a library like **LiveKit**, **mediasoup**, or **Janus** — raw WebRTC is complex
- HTTPS is required to access the user's camera/mic
