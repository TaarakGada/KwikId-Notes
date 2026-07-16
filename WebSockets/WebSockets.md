# WebSockets

## What is it?
- WebSocket is a protocol that provides a **persistent, two-way connection** between client and server
- Unlike regular HTTP (which is one request → one response), WebSocket stays open — both sides can send messages anytime
- Think of HTTP as writing letters (one at a time), and WebSocket as a **phone call** (both can talk freely)

## HTTP vs WebSocket
| Feature | HTTP | WebSocket |
|---------|------|-----------|
| Connection | Opens and closes per request | Stays open |
| Communication | Client asks, server answers | Both can send anytime |
| Overhead | Headers on every request | Low — just send data |
| Use case | Page loads, REST APIs | Chat, live data, games |

## Common Use Cases
- Real-time chat applications
- Live sports scores / stock prices
- Multiplayer games
- Collaborative editing (Google Docs-like)
- Live notifications
- WebRTC signaling

## How It Works
1. Client sends an HTTP request with `Upgrade: websocket` header
2. Server responds with `101 Switching Protocols`
3. Connection is now a WebSocket — both sides can send frames freely

## Browser JavaScript Example
```javascript
// Connect to WebSocket server
const ws = new WebSocket('ws://localhost:8080');

// Connection opened
ws.onopen = () => {
  console.log('Connected!');
  ws.send('Hello, server!');  // Send a message
};

// Receive a message
ws.onmessage = (event) => {
  console.log('Received:', event.data);
};

// Connection closed
ws.onclose = () => console.log('Disconnected');

// Error
ws.onerror = (err) => console.error('Error:', err);

// Send a JSON message
ws.send(JSON.stringify({ type: 'chat', message: 'Hi there!' }));
```

## Python Server Example (using `websockets` library)
```python
import asyncio
import websockets
import json

async def handler(websocket):
    print("Client connected")
    async for message in websocket:
        data = json.loads(message)
        print(f"Received: {data}")
        # Echo back
        await websocket.send(json.dumps({'status': 'ok', 'received': data}))

async def main():
    async with websockets.serve(handler, 'localhost', 8080):
        print("Server running on ws://localhost:8080")
        await asyncio.Future()  # Run forever

asyncio.run(main())
```

## WebSocket with Socket.IO (More features)
```javascript
// Socket.IO adds rooms, events, auto-reconnect on top of WebSocket
const io = require('socket.io')(server);

io.on('connection', (socket) => {
  socket.on('message', (data) => {
    io.emit('message', data);  // Broadcast to all clients
  });
});
```

## Good to Know
- Use `wss://` (WebSocket Secure) in production — like HTTPS for WebSockets
- WebSocket connections can drop — implement reconnection logic
- **Socket.IO** is a popular library that wraps WebSockets with extra features
- Not ideal for one-off requests — use regular HTTP/REST for those
- Nginx can proxy WebSocket connections (requires specific config)

## Nginx WebSocket Proxy Config
```nginx
location /ws {
    proxy_pass http://localhost:8080;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
}
```

---

## WebSockets & Signaling in KwikID

In the **KwikID Call system**, we run two parallel networks: **Media** (video/audio via LiveKit) and **Signaling** (Socket.io). 

### Socket.io Roles
The socket channel (`socket.io-client`) is responsible for:
*   Synchronizing UI steps between User and Agent portals.
*   Triggering remote photo capture events.
*   Exchanging real-time chat and network speed telemetry.

### Call Health & Connection Monitor
Since the socket channel and LiveKit channels can fail independently, KwikID implements a **hybrid signaling health monitor**:
1.  **Ping-Pong Check**: Once a call starts, the Agent Portal runs a loop sending `ping_to_peer` to the User Portal every **12 seconds**, expecting a response within an **8-second** timeout.
2.  **Signaling Lost (Amber Warning)**: If a ping times out or telemetry goes silent for **18 seconds**, the connection state is marked as degraded, updating the header to **"Signaling reconnecting"** (amber). The video stream may still be running.
3.  **Status Escalation Check**: Rather than showing "User Disconnected" (red) immediately on socket disconnect, the system:
    *   Polls the API backend (`isUserDroppedCall`) to see if the user deliberately exited.
    *   Waits up to **30 seconds** (`SIGNALING_LOST_ESCALATE_MS`) before confirming disconnection.
    *   If signaling recovers (e.g. via a new socket registration or track update), the timers are canceled and the call returns to "ongoing call" status.
