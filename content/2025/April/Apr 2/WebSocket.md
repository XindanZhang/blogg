# WebSocket

WebSocket is a communication protocol that provides full-duplex communication channels over a single TCP connection. It enables real-time, bidirectional communication between a client (typically a web browser) and a server.

## Key Features

- **Persistent Connection**: Unlike HTTP, WebSocket maintains a persistent connection after the initial handshake
- **Low Latency**: Reduced overhead compared to HTTP polling methods
- **Bidirectional**: Both server and client can send data at any time
- **Cross-Origin Communication**: Supports communication across different domains with proper configuration

## Common Use Cases

- Real-time chat applications
- Live dashboards and monitoring systems
- Online gaming
- Collaborative editing tools
- Financial trading platforms
- Live sports updates

## Basic Implementation

```javascript
// Client-side example
const socket = new WebSocket('ws://example.com/socketserver');

socket.onopen = function(event) {
  console.log('Connection established');
  socket.send('Hello Server!');
};

socket.onmessage = function(event) {
  console.log('Message from server:', event.data);
};

socket.onclose = function(event) {
  console.log('Connection closed');
};
```
