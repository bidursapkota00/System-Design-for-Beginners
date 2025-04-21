# 07 - HTTP, RPC, Methods, Status Codes, SSL/TLS, HTTPS

## 1. What is HTTP?

**HTTP (HyperText Transfer Protocol)** is the foundational protocol for data communication on the web. It defines how clients (usually browsers) communicate with servers.

- **Stateless**: Each request is independent.
- **Text-based**: Uses plain text for communication.
- **Request/Response Model**: The client sends a request, and the server responds.

---

## 2. What is RPC?

**RPC (Remote Procedure Call)** is a protocol that allows a program to execute a function or procedure on a remote server as if it were local.

- RPC abstracts the network layer, making remote calls look like local function calls.
- Used in microservices and backend systems.
- Protocols: gRPC (Google), JSON-RPC, XML-RPC.

### HTTP vs RPC:

| Feature     | HTTP                           | RPC                          |
| ----------- | ------------------------------ | ---------------------------- |
| Format      | Text-based (usually JSON/HTML) | Binary or JSON               |
| Abstraction | Resource-based (`/users`)      | Function-based (`getUser()`) |
| Use Case    | Web apps, REST APIs            | Backend microservice comms   |

---

## 3. Common HTTP Methods

| Method  | Purpose                             |
| ------- | ----------------------------------- |
| GET     | Retrieve data (no side effects)     |
| POST    | Submit data (e.g., form submission) |
| PUT     | Replace existing resource           |
| PATCH   | Partially update a resource         |
| DELETE  | Remove a resource                   |
| HEAD    | Same as GET, but no response body   |
| OPTIONS | Discover supported methods          |

---

## 4. Common HTTP Status Codes

### 1xx: Informational

- `100 Continue`: Request received, continue sending body.

### 2xx: Success

- `200 OK`: Request successful.
- `201 Created`: Resource created.
- `204 No Content`: Success, no body returned.

### 3xx: Redirection

- `301 Moved Permanently`: Resource moved to a new URL.
- `302 Found`: Temporary redirect.
- `304 Not Modified`: Use cached version.

### 4xx: Client Errors

- `400 Bad Request`: Invalid input.
- `401 Unauthorized`: Auth required.
- `403 Forbidden`: Authenticated but not allowed.
- `404 Not Found`: Resource not found.

### 5xx: Server Errors

- `500 Internal Server Error`: Something broke.
- `502 Bad Gateway`: Invalid response from upstream.
- `503 Service Unavailable`: Server is overloaded or down.

---

## 5. SSL/TLS and HTTPS

### What is SSL/TLS?

- **SSL (Secure Sockets Layer)** and its successor **TLS (Transport Layer Security)** encrypt communication between client and server.
- Protects against:
  - Eavesdropping
  - Data tampering
  - Man-in-the-middle (MITM) attacks

### How SSL/TLS Works (Simplified):

1. **Handshake**: Client and server agree on encryption methods and exchange keys.
2. **Authentication**: Server provides a digital certificate (usually issued by a trusted CA).
3. **Session Keys**: Both sides derive session keys for encryption.
4. **Encrypted Communication**: All subsequent data is encrypted using these keys.

---

## 6. What is HTTPS?

- **HTTPS = HTTP + TLS**
- Ensures encrypted and secure communication over the web.
- Common on websites handling sensitive data (login, payments).

---
