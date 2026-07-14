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
