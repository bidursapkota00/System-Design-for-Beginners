# 08 - API Paradigms

## Overview

APIs (Application Programming Interfaces) define how different software components communicate. Choosing the right API paradigm impacts performance, scalability, and developer experience. This lesson covers major API paradigms:

- REST
- GraphQL
- gRPC
- Others (WebSockets, RPC)

---

## 1. REST (Representational State Transfer)

### Characteristics

- Stateless client-server architecture.
- Uses HTTP methods: GET, POST, PUT, DELETE.
- Resource-oriented (URLs represent resources).

### Sessions & Cookies

- Stateless by default; sessions are maintained using cookies or tokens (e.g., JWT).
- Cookies store session ID on client, sent with every request.
- Server validates session ID to associate with a user.

### Example

```http
GET /users/123
```

Returns user data for user with ID 123.

### Pros

- Simple and widely adopted.
- Good for CRUD-based applications.
- Caches well.

### Cons

- Overfetching or underfetching data.
- Multiple round trips for complex queries.

---

## 2. GraphQL

### Characteristics

- Declarative data fetching query language.
- One endpoint (usually `/graphql`) for all interactions.

### Query and Mutation

- **Query**: Fetch data.
- **Mutation**: Modify data (create/update/delete).

### Overfetching & Underfetching

- Solves overfetching by allowing clients to specify exactly what they need.
- Also prevents underfetching that leads to multiple requests.

### Example Query

```graphql
{
  user(id: "123") {
    name
    email
  }
}
```

### Pros

- Reduces number of requests.
- Fine-grained data fetching.
- Strong typing (schema-driven).

### Cons

- Complex caching.
- Harder to debug and monitor.
- Learning curve for newcomers.

---

## 3. gRPC

### Characteristics

- High-performance RPC framework developed by Google.
- Uses HTTP/2 and Protocol Buffers (Protobuf) for compact communication.

### Protocol Buffers

- Efficient binary serialization format.
- Smaller payload size compared to JSON.

### Streaming Support

- Supports:
  - Unary (single request/response)
  - Server-side streaming
  - Client-side streaming
  - Bi-directional streaming

### Pros

- Great for internal microservice communication.
- Strong typing and fast performance.
- Built-in code generation for multiple languages.

### Cons

- Not human-readable (binary format).
- Less browser support (typically used in backend systems).

---

## 4. Other Paradigms

### WebSockets

- Full-duplex communication over a single TCP connection.
- Suitable for real-time apps (chat, notifications).

### Traditional RPC (Remote Procedure Call)

- Function-call semantics over the network.
- gRPC is a modern take on RPC.

---
