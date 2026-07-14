# TURN & STUN Servers

## The Problem: NAT
- Most devices are behind a **NAT (Network Address Translation)** — they have a private IP (like `192.168.x.x`) and share a public IP
- Two peers trying to connect may not be able to reach each other directly
- STUN and TURN servers solve this

## STUN — Session Traversal Utilities for NAT

### What it does
- Tells your device: **"Your public IP address is X"**
- Free to run, very lightweight
- Doesn't relay traffic — just helps with discovery

### How it works
```
Peer → asks STUN server → "Your public IP is 203.0.113.5:50000"
Peer shares this IP with the other peer via signaling
Peers attempt direct connection using these public IPs
```

### Free STUN Servers
- `stun:stun.l.google.com:19302` (Google's free STUN)
- `stun:stun1.l.google.com:19302`

### Limitation
- Fails if either peer is behind a **symmetric NAT** (common in corporate networks)

## TURN — Traversal Using Relays around NAT

### What it does
- Acts as a **relay server** — all media traffic goes through it
- Works in 100% of network conditions
- More expensive (you host it, bandwidth is used)

### How it works
```
Peer A → TURN Server → Peer B
(Traffic relayed, not direct P2P)
```

### When is TURN needed?
- Both peers on different networks with strict NAT
- Corporate firewalls blocking direct connections
- When STUN fails

## Using Both in WebRTC
```javascript
const pc = new RTCPeerConnection({
  iceServers: [
    // STUN — try direct connection first
    { urls: 'stun:stun.l.google.com:19302' },
    
    // TURN — fallback if direct fails
    {
      urls: 'turn:your-turn-server.com:3478',
      username: 'your-username',
      credential: 'your-password'
    }
  ]
});
```

## STUN vs TURN Comparison
| Feature | STUN | TURN |
|---------|------|------|
| What it does | Discovers public IP | Relays all traffic |
| Hosts traffic | No | Yes |
| Cost | Free/cheap | Higher (bandwidth) |
| Reliability | May fail | Always works |
| Latency | Low (direct P2P) | Higher (relayed) |

## Setting Up Your Own TURN Server
- **Coturn** is the most popular open-source TURN server
```bash
# Install coturn on Ubuntu
sudo apt install coturn

# Basic config /etc/turnserver.conf
listening-port=3478
external-ip=YOUR_PUBLIC_IP
username=user
password=pass
realm=yourdomain.com
```

## Good to Know
- Always configure both STUN and TURN — let ICE choose
- TURN server must be publicly accessible
- **LiveKit and similar services** provide managed TURN servers so you don't have to host your own
- TURNS (TURN over TLS) is more secure, uses port 443 which is rarely blocked
