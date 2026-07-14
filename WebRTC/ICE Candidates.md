# ICE Candidates

## What Are They?
- ICE stands for **Interactive Connectivity Establishment**
- An ICE candidate is a **possible network address** that a peer can use to receive data
- WebRTC collects all possible candidates and tries each one to find the best path
- Think of it like suggesting multiple roads to a destination — whichever is fastest wins

## Types of ICE Candidates

### 1. Host Candidates
- The device's own local IP addresses (e.g., `192.168.1.5`)
- Works only if both peers are on the **same local network**

### 2. Server Reflexive Candidates (STUN)
- The public IP of the device as seen by the internet (obtained via STUN server)
- Works when peers are on **different networks** but not behind strict firewalls

### 3. Relay Candidates (TURN)
- An IP address from a **TURN server** that relays traffic
- Used when direct connection fails (strict firewalls, symmetric NAT)
- Slowest but most reliable — always works

## ICE Candidate Priority
```
Host (fastest, private network)
    ↓ if fails
Server Reflexive / STUN (public network)
    ↓ if fails  
Relay / TURN (always works, slowest)
```

## What a Candidate Looks Like
```
candidate:1 1 UDP 2122252543 192.168.1.5 50000 typ host
```
- `192.168.1.5` — the IP
- `50000` — the port
- `typ host` — type is host (local)

## ICE Gathering Process
```javascript
const pc = new RTCPeerConnection({
  iceServers: [
    { urls: 'stun:stun.l.google.com:19302' },
    {
      urls: 'turn:your-turn-server.com',
      username: 'user',
      credential: 'password'
    }
  ]
});

// ICE candidates are generated asynchronously
pc.onicecandidate = (event) => {
  if (event.candidate) {
    // Send this candidate to the remote peer via signaling
    sendToRemotePeer(event.candidate);
  } else {
    // null candidate = ICE gathering is complete
    console.log('All ICE candidates gathered');
  }
};

// Adding a candidate received from remote peer
pc.addIceCandidate(new RTCIceCandidate(candidateFromRemote));
```

## ICE Connection States
- `checking` — Trying candidate pairs
- `connected` — Found a working path
- `completed` — Best path found
- `failed` — No path worked (usually need TURN server)
- `disconnected` — Connection was lost

## Good to Know
- ICE gathering happens automatically — you just need to forward candidates via signaling
- **Trickle ICE**: Send candidates to peer as they're discovered (faster than waiting for all)
- If ICE fails, check if you have a TURN server configured
- Symmetric NAT (common in corporate networks) blocks STUN — TURN required
