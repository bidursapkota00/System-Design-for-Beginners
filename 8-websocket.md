# 08 - WebSocket and Polling

## Overview

Real-time communication is a key requirement in many modern applications, such as chat apps, multiplayer games, live updates, and collaborative tools. Two primary techniques for enabling this are **WebSockets** and **Polling**.

---

## 1. Polling

### What is Polling?

Polling is a technique where the client repeatedly asks the server for new data at regular intervals.

### Example:

Client sends a request every 5 seconds:

Server responds with latest data (even if there's no update).

### Types of Polling

#### a. **Short Polling**

- Client sends requests at fixed intervals.
- Easy to implement but inefficient.
- Can lead to server overload and wasted bandwidth.

```js
setInterval(() => {
  fetch("/new-messages")
    .then((res) => res.json())
    .then((data) => console.log(data));
}, 3000); // every 3 seconds
```

#### b. **Long Polling**

- Client sends request and waits.
- Server holds the request open until new data is available.
- Once data is sent, the client immediately sends a new request.
- More efficient than short polling.

### Pros

- Simple to implement.
- Works over standard HTTP/HTTPS.

### Cons

- Higher latency than WebSocket.
- More overhead due to repeated HTTP connections.
- Not truly real-time.

---

## 2. WebSocket

### What is WebSocket?

**WebSocket** is a protocol that enables full-duplex (two-way) communication between client and server over a single, long-lived connection.

- Starts as HTTP → Upgrades to WebSocket via `Upgrade` header.
- Uses `ws://` or `wss://` (secure).
- Ideal for real-time data transfer.

### WebSocket Lifecycle

1. **Handshake**: Client sends HTTP request with `Upgrade: websocket`.
2. **Upgrade**: Server responds with `101 Switching Protocols`.
3. **Open Connection**: Persistent TCP connection is established.
4. **Data Exchange**: Both client and server can send data anytime.
5. **Close Connection**: Either party can close the connection.

### Example Use Cases

- Chat apps (WhatsApp, Slack)
- Real-time collaboration (Google Docs)
- Online gaming
- Live dashboards/stock tickers
- IoT device communication

```js
// Client
const socket = new WebSocket("wss://example.com/chat");

socket.onopen = () => socket.send("Hello!");
socket.onmessage = (event) => console.log("Server:", event.data);

// Server
const WebSocket = require("ws");
const wss = new WebSocket.Server({ port: 8080 });

wss.on("connection", (ws) => {
  ws.on("message", (message) => console.log("Client:", message));
  ws.send("Welcome!");
});
```

### Pros

- Real-time, bidirectional communication.
- Lower latency than polling.
- Less network overhead after connection is established.

### Cons

- Requires more complex server-side setup.
- Not ideal for simple applications or infrequent data changes.

---

## 3. Polling vs WebSocket

| Feature    | Polling                   | WebSocket                          |
| ---------- | ------------------------- | ---------------------------------- |
| Protocol   | HTTP                      | TCP (after HTTP upgrade)           |
| Direction  | Client → Server           | Bidirectional                      |
| Latency    | Higher                    | Very low                           |
| Overhead   | High (many HTTP requests) | Low (single persistent connection) |
| Complexity | Simple                    | Moderate to complex                |
| Use Case   | Simple updates            | Real-time apps                     |

---
