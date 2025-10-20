# System Design Course

## Table of Contents

1. [Computer Architecture](#01---computer-architecture)
2. [Application Architecture](#02---application-architecture)
3. [Design Requirements](#03---design-requirements)
4. [Networking Basics](#04---networking-basics)
5. [TCP and UDP](#05---tcp-and-udp)
6. [DNS, Dynamic DNS, and the DNS Lookup Process](#06---dns-dynamic-dns-and-the-dns-lookup-process)
7. [HTTP, RPC, Methods, Status Codes, SSL/TLS, HTTPS](#07---http-rpc-methods-status-codes-ssltls-https)
8. [WebSocket and Polling](#08---websocket-and-polling)
9. [API Paradigms](#09---api-paradigms)
10. [API Design](#10---api-design)
11. [Caching](#11---caching)
12. [Content Delivery Networks (CDNs) & Edge Computing](#12---content-delivery-networks-cdns--edge-computing)
13. [Proxies and Load Balancers](#13---proxies-and-load-balancers)
14. [Consistent Hashing](#14---consistent-hashing)
15. [SQL](#15---sql)
16. [NoSQL](#16---nosql)
17. [Replication and Sharding](#17---replication-and-sharding)
18. [CAP Theorem](#18---cap-theorem)
19. [Object Storage](#19---object-storage)
20. [Message Queues](#20---message-queues)

---

## 01 - Computer Architecture

### Key Concepts

#### 1. CPU (Central Processing Unit)

- The "brain" of the computer.
- Responsible for executing instructions from applications and the operating system.
- Performance measured in clock cycles (GHz), cores, and instruction set architecture (ISA).

#### 2. Memory Hierarchy

##### a. Registers

- Fastest memory, inside the CPU.
- Very limited in size, used for immediate instruction execution.

##### b. CPU Cache

- **L1, L2, and L3 caches**: Small, fast memory located close to or inside the CPU.
- Stores frequently accessed data to reduce the need to access slower RAM.
- L1 is fastest but smallest; L3 is slower but larger.

##### c. RAM (Random Access Memory)

- Main memory used to store active processes and data.
- Volatile memory — data is lost when power is off.
- Slower than CPU cache but much larger.

##### d. Storage (Disk)

- Persistent memory (HDD or SSD).
- Used to store applications, files, and data long-term.
- SSDs are much faster than HDDs but slower than RAM.
- Storage affects I/O performance, latency, and throughput.

---

#### 4. Network Interface

- Facilitates communication with other machines via the Internet.
- Bandwidth and latency are key factors.

#### 5. I/O Devices

- Keyboards, displays, etc., but more relevant for system design is how servers interact with disks and networks.

#### 6. Bus

- Internal communication system that transfers data between components (CPU, memory, I/O devices).

### Why It Matters in System Design

- Performance bottlenecks often arise from CPU limitations, memory constraints, or slow disk I/O.
- A system designer must understand the hardware to make informed architectural choices (e.g., caching strategies, load balancing, parallelism).

### Common Bottlenecks

- CPU saturation under heavy computation.
- RAM limitations leading to swapping (slow performance).
- Disk I/O constraints in data-intensive applications.
- Network delays impacting distributed systems.

### Caching: The Data Journey from Storage to CPU

#### Process Flow:

#### Explanation:

1. **Storage → RAM**:

   - When a program or data is needed, it is loaded from the disk into RAM.
   - This is relatively slow (milliseconds), especially from HDDs.

2. **RAM → CPU Cache**:

   - The CPU requests specific data. If it's not in the cache (a cache miss), it fetches it from RAM.
   - Caches use algorithms (like LRU - Least Recently Used) to decide which data to keep.

3. **CPU Cache → CPU Registers**:
   - The most frequently accessed values are moved to registers for immediate use by the CPU.

#### Cache Efficiency:

- Caches drastically reduce the time it takes for the CPU to access memory.
- A cache hit (data already in cache) is much faster than fetching from RAM or disk.

---

---

---

## 02 - Application Architecture

### Key Architectures

#### 1. Monolithic Architecture

- A single unified codebase.
- Easier to develop and deploy initially.
- Becomes harder to scale and maintain as the application grows.

#### 2. Microservices Architecture

- Breaks the application into independent services (e.g., auth, payments, catalog).
- Each service has its own codebase and can be deployed, scaled, and updated independently.
- Requires robust inter-service communication (e.g., REST, gRPC) and coordination.

#### 3. Client-Server Model

- Client (browser, mobile app) interacts with server via APIs.
- Server processes logic and connects to databases or other services.

#### 4. N-Tier Architecture

- **Presentation Layer**: UI and frontend logic.
- **Application Layer**: Business logic and orchestration.
- **Data Layer**: Storage systems like SQL/NoSQL databases.

#### 5. Service-Oriented Architecture (SOA)

- Similar to microservices but more tightly integrated with enterprise-level systems.
- Commonly uses middleware like an Enterprise Service Bus (ESB).

---

### Scaling Applications

#### Vertical Scaling (Scaling Up)

- Add more CPU, RAM, or storage to a single server.
- Easier but limited by hardware capacity.
- Downtime might be required during scaling.

#### Horizontal Scaling (Scaling Out)

- Add more servers to distribute the load.
- Enables near-infinite scalability.
- Often requires stateless services and a load balancer.

---

### Load Balancer

- Sits between the client and backend servers.
- Distributes incoming traffic to multiple backend instances.
- Types:
  - **Layer 4 (Transport-level)**: Operates at TCP/UDP level.
  - **Layer 7 (Application-level)**: Makes routing decisions based on content (e.g., URL, headers).
- Ensures:
  - High availability
  - Fault tolerance
  - Even resource usage

---

### Observability: Logging, Metrics, and Alerts

#### Logging

- Records system events, errors, and app-specific information.
- Helps in debugging and post-mortem analysis.
- Tools: ELK stack (Elasticsearch, Logstash, Kibana), Fluentd, Loki.

#### Metrics

- Quantitative data like response times, memory usage, request rates.
- Used to monitor system health.
- Tools: Prometheus, Grafana, Datadog.

#### Alerts

- Notifications triggered by metrics/log patterns crossing thresholds.
- Critical for timely response to failures or performance degradation.
- Can be routed to emails, Slack, PagerDuty, etc.

---

---

---

## 03 - Design Requirements

### 1. Core System Actions

#### Move Data

- Transferring data between systems, services, or users.
- Examples: sending data from client to server, syncing between services, or replicating between data centers.

#### Store Data

- Persisting data for future use.
- Involves choosing between SQL, NoSQL, file systems, or object storage.
- Considerations: durability, scalability, read/write speed, cost.

#### Transform Data

- Processing data for various purposes like analytics, formatting, validation, or machine learning.
- Examples: ETL (Extract, Transform, Load) pipelines, data cleaning, real-time processing.

---

### 2. Non-Functional Requirements

#### Availability

- The percentage of time the system is operational.
- Measured as uptime (e.g., 99.9%).
- High availability systems minimize downtime using failovers and redundancy.

#### Reliability

- The system performs as expected under specific conditions.
- Often involves retry mechanisms, error handling, and graceful degradation.

#### Fault Tolerance

- Ability to continue functioning despite component failures.
- Achieved through redundancy, backups, and fallback strategies.

#### Redundancy

- Duplication of critical components (data, servers, etc.) to ensure continued operation if one fails.
- Common in high-availability systems and disaster recovery planning.

---

### 3. Performance Metrics

#### Throughput

- Amount of work the system can handle over time (e.g., requests/sec).
- Higher throughput = more efficient system.
- Influenced by concurrency, resource allocation, and load balancing.

#### Latency

- Time taken to respond to a request.
- Measured in milliseconds.
- Lower latency = faster user experience.
- Influenced by network delays, processing speed, and system load.

---

### 4. Security and Scalability

#### DDoS (Distributed Denial of Service)

- Attack where many systems overwhelm a target with traffic.
- Defenses: rate limiting, firewalls, Web Application Firewalls (WAF), and CDNs.

#### CDN (Content Delivery Network)

- Distributes content across geographically located servers.
- Reduces latency by serving content closer to the user.
- Also helps absorb traffic spikes (including DDoS attacks).

---

---

---

## 04 - Networking Basics

### Key Concepts

#### 1. IP Addressing

- **IP Address**: Unique identifier for a device on a network (e.g., `192.168.1.1`).
- Two major versions: IPv4 (32-bit) and IPv6 (128-bit).

#### 2. Public vs Private IP

- **Public IP**:
  - Assigned by ISPs.
  - Routable over the internet.
  - Example: A server hosting a website typically has a public IP.
- **Private IP**:
  - Used within local networks (e.g., homes, offices).
  - Not routable over the public internet.
  - Common ranges: `192.168.x.x`, `10.x.x.x`, `172.16.x.x - 172.31.x.x`.

#### 3. Static vs Dynamic IP

- **Static IP**:
  - Manually assigned and remains constant.
  - Useful for servers or devices that need consistent addressing.
- **Dynamic IP**:
  - Automatically assigned by DHCP (Dynamic Host Configuration Protocol).
  - Changes over time or when the device reconnects.

---

### 4. Ports

- Represent different services on a machine.
- Example: `80` for HTTP, `443` for HTTPS, `22` for SSH.
- Enables multiple services to run on the same IP address.

#### 5. DNS (Domain Name System)

- Translates domain names like `example.com` to IP addresses.
- DNS Lookup Steps:
  1. Client checks local DNS cache.
  2. If not found, queries a recursive resolver.
  3. Resolver asks the root server → TLD server → Authoritative server.
  4. IP address is returned to the client.

#### 6. NAT (Network Address Translation)

- Allows multiple devices on a private network to access the internet using one public IP.
- Commonly used in routers.

#### 7. MAC Address

- Hardware identifier for a network interface.
- Used within local area networks (LANs).

#### 8. Firewalls

- Control network access based on rules (e.g., block all traffic except on port 443).
- Used to secure systems from unauthorized access.

---

### Communication Models

#### Client-Server Model

- Client sends request → Server processes it → Server sends response.
- Example: Web browser (client) requests a web page from a server.

#### Peer-to-Peer Model

- Each device can act as both client and server.
- Example: File sharing in torrents.

---

### The Request-Response Lifecycle (Web Example)

#### 1. User Action

- A user types `example.com` in the browser.

#### 2. DNS Resolution

- Browser checks cache or asks a DNS server to resolve `example.com` to an IP (e.g., `93.184.216.34`).

#### 3. TCP Connection

- A TCP connection is established between client and server (usually on port 80 or 443).

#### 4. HTTP Request Sent

- The browser sends an HTTP GET request for a web page.

#### 5. Server Processing

- Server receives the request, processes it, and prepares a response (e.g., HTML content).

#### 6. HTTP Response Sent

- Server sends back the response via TCP to the client.

#### 7. Rendering the Page

- Browser receives the HTML, renders the page, and may make additional requests for CSS, JS, images, etc.

---

---

---

## 05 - TCP and UDP

### TCP (Transmission Control Protocol)

#### Characteristics:

- **Connection-oriented**: A connection must be established before data can be sent.
- **Reliable**: Ensures delivery of packets in the correct order without loss or duplication.
- **Error Checking**: Includes mechanisms for retransmission, flow control, and congestion control.
- **Slower** than UDP due to overhead of maintaining state and reliability.

---

#### TCP 3-Way Handshake

1. **SYN (Synchronize)**: Client sends a TCP packet with SYN flag to server.
2. **SYN-ACK (Synchronize-Acknowledge)**: Server responds with SYN and ACK.
3. **ACK (Acknowledge)**: Client sends final ACK, and the connection is established.

Once the handshake completes, data transfer begins.

---

#### TCP Connection Termination

- Connection ends with a **4-step teardown** using FIN and ACK flags.
  - One side sends FIN → other responds with ACK.
  - Then reverse happens for the other direction.

---

#### Use Cases for TCP

- Web browsing (HTTP/HTTPS)
- Email (SMTP, IMAP)
- File transfers (FTP)
- Remote login (SSH)

---

### UDP (User Datagram Protocol)

#### Characteristics:

- **Connectionless**: No handshake or setup.
- **Unreliable**: No guarantees of delivery, ordering, or duplication protection.
- **Faster** than TCP due to minimal overhead.
- Suitable for applications where speed is more critical than reliability.

---

#### How UDP Works

- Each UDP packet (datagram) is sent independently.
- The receiver does not send acknowledgements.
- No guarantee that packets arrive in order—or at all.

---

#### Use Cases for UDP

- Real-time applications (voice/video calls, gaming, streaming).
- DNS lookups.
- IoT sensors sending frequent small bursts of data.

---

---

---

## 06 - DNS, Dynamic DNS, and the DNS Lookup Process

### What is DNS?

**DNS (Domain Name System)** is the internet’s phonebook. It translates human-readable domain names (like `openai.com`) into machine-readable IP addresses (like `104.18.12.123`), enabling devices to locate and communicate with each other.

---

### Key Components of DNS

- **Domain Name**: A readable address like `example.com`.
- **IP Address**: The actual network address (IPv4 or IPv6).
- **DNS Resolver**: A client-side service (usually your ISP) that performs DNS queries.
- **Root DNS Servers**: Top of the DNS hierarchy. Direct queries to TLD servers.
- **TLD Servers**: Handle top-level domains like `.com`, `.org`, `.net`.
- **Authoritative DNS Servers**: Contain the actual IP address for a domain.

---

### What is Dynamic DNS (DDNS)?

**Dynamic DNS** automatically updates DNS records whenever the device's IP changes (especially useful for devices on dynamic IPs, like most home internet connections).

#### Use Cases:

- Hosting a server at home (e.g., security camera or web server).
- IoT devices.
- Remote access when IP changes frequently.

#### How It Works:

- A DDNS service monitors your IP.
- When it changes, it sends an update to the DNS server.
- Your domain now points to the new IP without manual changes.

---

### DNS Lookup Process (Step-by-Step)

Let’s say a user visits `www.example.com`. Here’s what happens:

#### 1. **Browser Cache**

- Browser checks if it has recently resolved `www.example.com`.
- If yes, it uses that cached IP.

#### 2. **Operating System Cache**

- If not found in the browser, the OS checks its DNS cache.

#### 3. **DNS Resolver (Usually ISP)**

- If not found locally, the query is sent to the configured DNS resolver (e.g., Google’s 8.8.8.8).

#### 4. **Root Server Query**

- Resolver asks the **root server** for `.com` TLD info.

#### 5. **TLD Server Query**

- Root server responds with TLD server for `.com`.
- Resolver queries this TLD server.

#### 6. **Authoritative Server Query**

- TLD responds with the authoritative name server for `example.com`.
- Resolver queries this server.

#### 7. **IP Address Returned**

- Authoritative server responds with the IP address of `www.example.com`.

#### 8. **Client Connects to Server**

- The resolver sends the IP to the client.
- The browser makes an HTTP/S request to that IP.

#### 9. **Caching for Speed**

- All intermediate steps are cached by the resolver and the client for future speed.

---

### Example Timeline:

User types www.example.com ↓
Local browser/OS cache → miss ↓
Query to ISP DNS resolver (or 8.8.8.8) ↓ → Root server (where's .com?) → TLD server (where's example.com?) → Authoritative server (what's www.example.com?) ↓ IP returned (e.g., 93.184.216.34) ↓ Browser loads the website.

---

---

---

## 07 - HTTP, RPC, Methods, Status Codes, SSL/TLS, HTTPS

### 1. What is HTTP?

**HTTP (HyperText Transfer Protocol)** is the foundational protocol for data communication on the web. It defines how clients (usually browsers) communicate with servers.

- **Stateless**: Each request is independent.
- **Text-based**: Uses plain text for communication.
- **Request/Response Model**: The client sends a request, and the server responds.

---

### 2. What is RPC?

**RPC (Remote Procedure Call)** is a protocol that allows a program to execute a function or procedure on a remote server as if it were local.

- RPC abstracts the network layer, making remote calls look like local function calls.
- Used in microservices and backend systems.
- Protocols: gRPC (Google), JSON-RPC, XML-RPC.

#### HTTP vs RPC:

| Feature     | HTTP                           | RPC                          |
| ----------- | ------------------------------ | ---------------------------- |
| Format      | Text-based (usually JSON/HTML) | Binary or JSON               |
| Abstraction | Resource-based (`/users`)      | Function-based (`getUser()`) |
| Use Case    | Web apps, REST APIs            | Backend microservice comms   |

---

### 3. Common HTTP Methods

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

### 4. Common HTTP Status Codes

#### 1xx: Informational

- `100 Continue`: Request received, continue sending body.

#### 2xx: Success

- `200 OK`: Request successful.
- `201 Created`: Resource created.
- `204 No Content`: Success, no body returned.

#### 3xx: Redirection

- `301 Moved Permanently`: Resource moved to a new URL.
- `302 Found`: Temporary redirect.
- `304 Not Modified`: Use cached version.

#### 4xx: Client Errors

- `400 Bad Request`: Invalid input.
- `401 Unauthorized`: Auth required.
- `403 Forbidden`: Authenticated but not allowed.
- `404 Not Found`: Resource not found.

#### 5xx: Server Errors

- `500 Internal Server Error`: Something broke.
- `502 Bad Gateway`: Invalid response from upstream.
- `503 Service Unavailable`: Server is overloaded or down.

---

### 5. SSL/TLS and HTTPS

#### What is SSL/TLS?

- **SSL (Secure Sockets Layer)** and its successor **TLS (Transport Layer Security)** encrypt communication between client and server.
- Protects against:
  - Eavesdropping
  - Data tampering
  - Man-in-the-middle (MITM) attacks

#### How SSL/TLS Works (Simplified):

1. **Handshake**: Client and server agree on encryption methods and exchange keys.
2. **Authentication**: Server provides a digital certificate (usually issued by a trusted CA).
3. **Session Keys**: Both sides derive session keys for encryption.
4. **Encrypted Communication**: All subsequent data is encrypted using these keys.

---

### 6. What is HTTPS?

- **HTTPS = HTTP + TLS**
- Ensures encrypted and secure communication over the web.
- Common on websites handling sensitive data (login, payments).

---

---

---

## 08 - WebSocket and Polling

### Overview

Real-time communication is a key requirement in many modern applications, such as chat apps, multiplayer games, live updates, and collaborative tools. Two primary techniques for enabling this are **WebSockets** and **Polling**.

---

### 1. Polling

#### What is Polling?

Polling is a technique where the client repeatedly asks the server for new data at regular intervals.

#### Example:

Client sends a request every 5 seconds:

Server responds with latest data (even if there's no update).

#### Types of Polling

##### a. **Short Polling**

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

##### b. **Long Polling**

- Client sends request and waits.
- Server holds the request open until new data is available.
- Once data is sent, the client immediately sends a new request.
- More efficient than short polling.

#### Pros

- Simple to implement.
- Works over standard HTTP/HTTPS.

#### Cons

- Higher latency than WebSocket.
- More overhead due to repeated HTTP connections.
- Not truly real-time.

---

### 2. WebSocket

#### What is WebSocket?

**WebSocket** is a protocol that enables full-duplex (two-way) communication between client and server over a single, long-lived connection.

- Starts as HTTP → Upgrades to WebSocket via `Upgrade` header.
- Uses `ws://` or `wss://` (secure).
- Ideal for real-time data transfer.

#### WebSocket Lifecycle

1. **Handshake**: Client sends HTTP request with `Upgrade: websocket`.
2. **Upgrade**: Server responds with `101 Switching Protocols`.
3. **Open Connection**: Persistent TCP connection is established.
4. **Data Exchange**: Both client and server can send data anytime.
5. **Close Connection**: Either party can close the connection.

#### Example Use Cases

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

#### Pros

- Real-time, bidirectional communication.
- Lower latency than polling.
- Less network overhead after connection is established.

#### Cons

- Requires more complex server-side setup.
- Not ideal for simple applications or infrequent data changes.

---

### 3. Polling vs WebSocket

| Feature    | Polling                   | WebSocket                          |
| ---------- | ------------------------- | ---------------------------------- |
| Protocol   | HTTP                      | TCP (after HTTP upgrade)           |
| Direction  | Client → Server           | Bidirectional                      |
| Latency    | Higher                    | Very low                           |
| Overhead   | High (many HTTP requests) | Low (single persistent connection) |
| Complexity | Simple                    | Moderate to complex                |
| Use Case   | Simple updates            | Real-time apps                     |

---

---

---

## 09 - API Paradigms

### Overview

APIs (Application Programming Interfaces) define how different software components communicate. Choosing the right API paradigm impacts performance, scalability, and developer experience. This lesson covers major API paradigms:

- REST
- GraphQL
- gRPC
- Others (WebSockets, RPC)

---

### 1. REST (Representational State Transfer)

#### Characteristics

- Stateless client-server architecture.
- Uses HTTP methods: GET, POST, PUT, DELETE.
- Resource-oriented (URLs represent resources).

#### Sessions & Cookies

- Stateless by default; sessions are maintained using cookies or tokens (e.g., JWT).
- Cookies store session ID on client, sent with every request.
- Server validates session ID to associate with a user.

#### Example

```http
GET /users/123
```

Returns user data for user with ID 123.

#### Pros

- Simple and widely adopted.
- Good for CRUD-based applications.
- Caches well.

#### Cons

- Overfetching or underfetching data.
- Multiple round trips for complex queries.

---

### 2. GraphQL

#### Characteristics

- Declarative data fetching query language.
- One endpoint (usually `/graphql`) for all interactions.

#### Query and Mutation

- **Query**: Fetch data.
- **Mutation**: Modify data (create/update/delete).

#### Overfetching & Underfetching

- Solves overfetching by allowing clients to specify exactly what they need.
- Also prevents underfetching that leads to multiple requests.

#### Example Query

```graphql
{
  user(id: "123") {
    name
    email
  }
}
```

#### Pros

- Reduces number of requests.
- Fine-grained data fetching.
- Strong typing (schema-driven).

#### Cons

- Complex caching.
- Harder to debug and monitor.
- Learning curve for newcomers.

---

### 3. gRPC

#### Characteristics

- High-performance RPC framework developed by Google.
- Uses HTTP/2 and Protocol Buffers (Protobuf) for compact communication.

#### Protocol Buffers

- Efficient binary serialization format.
- Smaller payload size compared to JSON.

#### Streaming Support

- Supports:
  - Unary (single request/response)
  - Server-side streaming
  - Client-side streaming
  - Bi-directional streaming

#### Pros

- Great for internal microservice communication.
- Strong typing and fast performance.
- Built-in code generation for multiple languages.

#### Cons

- Not human-readable (binary format).
- Less browser support (typically used in backend systems).

---

### 4. Other Paradigms

#### WebSockets

- Full-duplex communication over a single TCP connection.
- Suitable for real-time apps (chat, notifications).

#### Traditional RPC (Remote Procedure Call)

- Function-call semantics over the network.
- gRPC is a modern take on RPC.

---

---

---

## 10 - API Design

### Introduction to API Design

API (Application Programming Interface) design is the process of developing APIs that effectively expose data and application functionality for consumption by developers and applications. Good API design is crucial for developer experience, system scalability, and long-term maintenance.

### Core API Design Paradigms

#### REST (Representational State Transfer)

REST is an architectural style for distributed systems, particularly web services. RESTful APIs use HTTP methods explicitly and have the following characteristics:

- **Stateless**: Server doesn't store client state between requests
- **Resource-based**: Everything is a resource identified by URIs
- **Representation-oriented**: Resources can have multiple representations (JSON, XML, etc.)
- **Uniform interface**: Consistent approach to resource manipulation
- **Client-server architecture**: Separation of concerns between client and server
- **Layered system**: Components cannot "see" beyond their immediate layer

##### REST Maturity Levels (Richardson Maturity Model)

1. **Level 0**: Single URI, single HTTP method (typically POST)
2. **Level 1**: Multiple URIs for different resources, but still a single HTTP method
3. **Level 2**: HTTP verbs used as intended (GET, POST, PUT, DELETE)
4. **Level 3**: HATEOAS (Hypermedia as the Engine of Application State) - APIs provide hyperlinks to guide clients

#### RPC (Remote Procedure Call)

RPC focuses on actions and functions rather than resources:

- Typically uses POST for most operations
- Emphasizes operations over resources
- Examples include gRPC, XML-RPC, and JSON-RPC
- Often preferred for internal service-to-service communication

##### gRPC

- Uses Protocol Buffers for serialization
- Supports bidirectional streaming
- Provides auto-generated client libraries

#### GraphQL

A query language and runtime for APIs:

- Client specifies exactly what data it needs
- Single endpoint for all operations
- Strongly typed schema
- Reduces over-fetching and under-fetching of data
- Supports queries (read), mutations (write), and subscriptions (real-time updates)

### CRUD Operations

CRUD represents the four basic operations for persistent storage:

| Operation | Description               | HTTP Method (REST) | SQL    |
| --------- | ------------------------- | ------------------ | ------ |
| Create    | Add new resources         | POST               | INSERT |
| Read      | Retrieve resources        | GET                | SELECT |
| Update    | Modify existing resources | PUT/PATCH          | UPDATE |
| Delete    | Remove resources          | DELETE             | DELETE |

#### REST CRUD Mapping

```http
POST /users             # Create a user
GET /users              # Read all users
GET /users/{id}         # Read specific user
PUT /users/{id}         # Update user (full replacement)
PATCH /users/{id}       # Update user (partial modification)
DELETE /users/{id}      # Delete user
```

### URL Design Best Practices

#### Resource Naming

- Use nouns, not verbs (e.g., `/products` not `/getProducts`)
- Use plural nouns for collections (`/users` not `/user`)
- Use concrete names over abstract concepts
- Use lowercase letters and hyphens for multi-word resources

#### Hierarchy and Relationships

- Express parent-child relationships in the URL path:
  ```http
  /departments/{id}/employees
  /users/{id}/posts
  ```
- Consider the depth of nesting (avoid too many levels)

#### Query Parameters vs Path Parameters

- **Path parameters**: Identify specific resources
  ```http
  /users/123
  ```
- **Query parameters**: Filter, sort, paginate, or control response format
  ```http
  /users?role=admin&status=active
  /products?sort=price&direction=asc
  ```

### Pagination Strategies

#### Limit-Offset Pagination

- **Limit**: Maximum number of items to return
- **Offset**: Number of items to skip

```http
GET /products?limit=10&offset=20
```

Pros:

- Simple to implement
- Stateless
- Works well with SQL databases (`LIMIT` and `OFFSET` clauses)

Cons:

- Performance issues with large offsets
- Potential inconsistencies with concurrent updates

#### Cursor-Based Pagination

Uses a pointer (cursor) to a specific item:

```http
GET /products?limit=10&after=eyJpZCI6MTAwfQ==
```

Pros:

- Performance remains consistent regardless of page depth
- Handles insertions/deletions gracefully
- Better for real-time data

Cons:

- More complex to implement
- Usually requires an indexed field for sorting

#### Page-Based Pagination

Uses page numbers:

```http
GET /products?page=2&per_page=10
```

Pros:

- Intuitive for users
- Simple to implement

Cons:

- Same issues as offset pagination
- Doesn't handle data changes well

### Response Formats

#### JSON Structure Best Practices

```json
{
  "data": {
    "id": 123,
    "name": "Example Product",
    "price": 99.99
  },
  "meta": {
    "timestamp": "2023-04-10T15:00:00Z",
    "version": "1.0"
  }
}
```

#### Collection Responses with Pagination

```json
{
  "data": [
    { "id": 1, "name": "Product A" },
    { "id": 2, "name": "Product B" }
  ],
  "pagination": {
    "total_items": 50,
    "total_pages": 5,
    "current_page": 1,
    "per_page": 10,
    "next": "/products?page=2&per_page=10",
    "prev": null
  }
}
```

### Error Handling

#### HTTP Status Codes

- **2xx**: Success (200 OK, 201 Created, 204 No Content)
- **4xx**: Client errors (400 Bad Request, 401 Unauthorized, 404 Not Found)
- **5xx**: Server errors (500 Internal Server Error, 503 Service Unavailable)

#### Error Response Format

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input parameters",
    "details": [
      { "field": "email", "message": "Must be a valid email address" }
    ]
  }
}
```

### Versioning Strategies

#### URI Path Versioning

```http
/api/v1/products
/api/v2/products
```

#### Query Parameter Versioning

```http
/api/products?version=1
```

#### Header Versioning

```http
Accept: application/vnd.myapi.v2+json
```

#### Content Negotiation

```http
Accept: application/vnd.myapi+json; version=2.0
```

### Authentication & Authorization

#### Authentication Methods

- **API Keys**: Simple key passed in header or query parameter
- **OAuth 2.0**: Token-based authorization framework
- **JWT (JSON Web Tokens)**: Self-contained tokens with encoded claims
- **Basic Auth**: Base64 encoded username:password

#### Authorization Patterns

- **Role-Based Access Control (RBAC)**: Permissions assigned to roles
- **Attribute-Based Access Control (ABAC)**: Dynamic permissions based on attributes
- **Scopes**: Permissions attached to tokens

### API Documentation

#### Documentation Formats

- **OpenAPI/Swagger**: Industry standard for REST APIs
- **API Blueprint**: Markdown-based documentation
- **RAML**: YAML-based API modeling language

#### Documentation Best Practices

- Keep documentation in sync with implementation
- Include examples for each endpoint
- Document error responses and edge cases
- Provide SDKs or code snippets when possible

### Performance Considerations

- **Caching**: Use HTTP caching headers (ETag, Last-Modified)
- **Compression**: Enable GZIP/Brotli compression
- **Rate Limiting**: Protect against abuse and ensure fair usage
- **Batch Operations**: Allow multiple operations in a single request

### Security Best Practices

- Always use HTTPS
- Implement proper authentication and authorization
- Validate and sanitize all inputs
- Apply rate limiting and throttling
- Use security headers (CORS, CSP, etc.)
- Regular security audits and testing

### Hypermedia and HATEOAS

```json
{
  "id": 123,
  "name": "Example Product",
  "price": 99.99,
  "_links": {
    "self": { "href": "/products/123" },
    "manufacturer": { "href": "/manufacturers/456" },
    "related": { "href": "/products/123/related" }
  }
}
```

### API Design Tools

- **Swagger/OpenAPI Editor**: For designing and documenting APIs
- **Postman**: API testing and documentation
- **Insomnia**: REST and GraphQL client
- **Apiary**: API design, prototyping, and documentation

---

---

---

## 11 - Caching

### Introduction to Caching

Caching is a technique that stores copies of frequently accessed data in a high-speed storage layer (the cache), allowing future requests for that data to be served faster. Caching plays a crucial role in improving application performance, reducing latency, decreasing network traffic, and minimizing load on backend servers.

### Why Use Caching?

- **Improved Performance**: Reduced data retrieval time
- **Enhanced Throughput**: Handle more requests with the same infrastructure
- **Reduced Bandwidth Usage**: Less data transferred over networks
- **Decreased Server Load**: Fewer requests to origin servers
- **Higher Availability**: Services remain available even when backend experiences issues
- **Cost Savings**: Lower compute and network resource requirements

### Key Caching Metrics

#### Throughput

Throughput in caching refers to the number of requests a cache can process per unit of time (typically measured in requests per second).

- **Factors affecting throughput**:

  - Cache hit ratio
  - Cache access latency
  - Network bandwidth and latency
  - Backend service performance
  - Cache size and eviction policy
  - Hardware resources (CPU, memory, network)

- **Optimizing for throughput**:

  - Increase cache memory allocation
  - Use efficient data structures
  - Implement appropriate eviction policies
  - Distribute cache load across multiple nodes
  - Minimize network round trips
  - Pre-warm cache with frequently accessed data

- **Measuring throughput**:
  ```text
  Throughput = (Total Requests × Hit Ratio) / Time Period
  ```
  - Higher hit ratio = higher effective throughput
  - Monitor throughput during peak loads to identify bottlenecks

#### Cache Hit Ratio

The percentage of requests that are served from the cache:

```text
Hit Ratio = (Cache Hits / Total Requests) × 100%
```

- **Good hit ratios** typically range from 80-95% depending on the application
- **Low hit ratio** may indicate:
  - Cache size too small
  - TTL values too short
  - Suboptimal eviction policy
  - Cacheable content not being cached
  - Highly random access patterns

### Types of Caches

#### Client-Side Caching

- **Browser Cache**: Stores web assets (HTML, CSS, JS, images) locally in the user's browser
- **Application Cache**: Data stored within client-side applications (mobile apps, SPAs)
- **Service Workers**: Enable offline functionality for web applications

#### Server-Side Caching

- **Application/In-Memory Cache**: Data stored in the application's memory space (e.g., using Redis, Memcached)
- **Page Cache**: Fully rendered HTML pages cached on the server
- **Fragment Cache**: Portions of pages or components cached separately
- **Database Query Cache**: Results of database queries stored for reuse
- **Opcode Cache**: Compiled code cached to avoid repetitive processing (e.g., PHP opcache)

#### Network Caching

- **Reverse Proxy Cache**: Caches content close to the server (e.g., Nginx, Varnish)
- **CDN Cache**: Distributes cached content geographically closer to users
- **Gateway Cache**: Caches content at network boundaries

### Caching Strategies

#### Cache-Aside (Lazy Loading)

1. Application checks the cache for data
2. If found (cache hit), data is returned
3. If not found (cache miss), data is fetched from the database
4. Application stores fetched data in cache for future use

```js
function getData(key) {
  data = cache.get(key);
  if (data == null) {
    data = database.get(key);
    cache.put(key, data);
  }
  return data;
}
```

**Pros:**

- Simple to implement
- Only caches what's actually requested
- Resilient to cache failures

**Cons:**

- Cache misses incur additional latency (database query + cache update)
- Potential for stale data
- Cold cache problem after cache restart

#### Write-Through

1. Application updates the database
2. Immediately after, application updates the cache

```js
function saveData(key, value) {
  database.put(key, value);
  cache.put(key, value);
  return success;
}
```

**Pros:**

- Data consistency between cache and database
- No cache miss penalty for newly written data

**Cons:**

- Write operations have higher latency (must update both DB and cache)
- Cache may contain unread data, wasting resources
- Additional write operation even if data isn't read later

#### Write-Behind (Write-Back)

1. Application updates the cache
2. Cache asynchronously updates the database

```js
function saveData(key, value) {
  cache.put(key, value);
  cacheQueue.queueForPersistence(key, value);
  return success;
}

// Separate process
function persistCacheUpdates() {
  while ((item = cacheQueue.dequeue())) {
    database.put(item.key, item.value);
  }
}
```

**Pros:**

- Reduced write latency
- Ability to batch multiple writes together
- Buffer against write-heavy workloads
- Improved throughput for write operations

**Cons:**

- Risk of data loss if cache fails before writing to database
- More complex implementation
- Potential data consistency issues

#### Write-Around

1. Application writes directly to the database, bypassing cache
2. Cache is only populated when data is read

```js
function saveData(key, value) {
  database.put(key, value);
  return success;
}

function getData(key) {
  data = cache.get(key);
  if (data == null) {
    data = database.get(key);
    cache.put(key, data);
  }
  return data;
}
```

**Pros:**

- Prevents cache churn from write-heavy operations
- Good for data that won't be read immediately or frequently

**Cons:**

- Cache misses for recently written data
- Higher read latency after writes

#### Refresh-Ahead

Proactively refreshes frequently accessed items before they expire

```js
function setUpRefreshAhead(key, refreshThreshold) {
  item = cache.get(key);
  if (
    item.accessCount > threshold &&
    item.expiryTime - now() < refreshThreshold
  ) {
    asyncRefresh(key);
  }
}

function asyncRefresh(key) {
  data = database.get(key);
  cache.put(key, data);
}
```

**Pros:**

- Reduces latency for frequently accessed items
- Smooths out database load spikes
- Maintains high throughput during peak times

**Cons:**

- More complex to implement
- May waste resources refreshing items that won't be accessed again
- Difficult to predict which items to refresh

### Cache Eviction Policies

When a cache reaches its capacity limit, eviction policies determine which items to remove to make space for new data.

#### Least Recently Used (LRU)

Removes the items that haven't been accessed for the longest time.

#### Least Frequently Used (LFU)

Removes items that are accessed least often. Tracks access frequency (hit count) for each item.

#### Other Notable Eviction Policies

- **First In, First Out (FIFO)**: Removes the oldest entries first
- **Random Replacement**: Randomly selects items for eviction
- **Most Recently Used (MRU)**: Removes the most recently used items (useful for certain scanning patterns)

### Redis as a Caching Solution

[Redis](https://redis.io/) (Remote Dictionary Server) is an open-source, in-memory data structure store that can be used as a database, cache, message broker, and streaming engine.

#### Key Redis Features

- **In-memory storage**: Extremely fast operations
- **Rich data structures**: Strings, Lists, Sets, Sorted Sets, Hashes, Streams, HyperLogLogs, Bitmaps
- **Persistence options**: RDB snapshots and AOF logs
- **Replication**: Master-slave architecture
- **High availability**: Redis Sentinel and Redis Cluster
- **Lua scripting**: Custom atomic operations
- **Pub/Sub messaging**: Real-time communication
- **Transactions**: Atomic operation sequences
- **Geospatial indexes**: Location-based features
- **Time series data support**: Efficient time-series storage

Redis provides several eviction policies configured via the `maxmemory-policy` setting:

### Throughput Optimization in Caching Systems

Throughput is a critical metric measuring a cache's request processing capacity. Several factors influence and can be optimized to improve throughput.

#### Hardware Considerations

- **Memory bandwidth**: Higher bandwidth = faster data retrieval
- **CPU speed and cores**: More processing power for complex cache operations
- **Network interface**: Higher bandwidth for distributed caches
- **Storage I/O**: For cache persistence operations

#### Software Optimizations

- **Serialization format**: Choose efficient formats (Protocol Buffers, MessagePack)

#### Caching Architecture for Throughput

- **Read replicas**: Scale read capacity horizontally
- **Sharding**: Distribute load across multiple cache instances
- **Local caching**: Reduce network round-trips with process-local caches

#### Measuring and Monitoring Throughput

- **Requests per second (RPS)**: Primary throughput metric
- **Response time percentiles**: P50, P95, P99 latency
- **Cache hit/miss ratio**: Impact on effective throughput
- **Network bandwidth utilization**: Potential bottleneck
- **CPU and memory utilization**: Resource constraints

### Cache Consistency Patterns

#### TTL-Based Invalidation

- Set reasonable expiration times based on data volatility
- Simple but can lead to stale data within the TTL window

#### Explicit Invalidation

- Services explicitly invalidate cache entries when underlying data changes
- More complex but provides better consistency

```js
function updateData(key, value) {
  database.put(key, value);
  cache.invalidate(key); // OR cache.put(key, value)
}
```

#### Event-Based Invalidation

- Use publish-subscribe mechanisms to notify cache of data changes
- Good for distributed systems where multiple services might update data

```js
function updateData(key, value) {
  database.put(key, value);
  messageBus.publish("data-change", { key: key });
}

// Cache subscriber
messageBus.subscribe("data-change", function (message) {
  cache.invalidate(message.key);
});
```

#### Version Stamping / ETag

- Associate a version identifier with cached data
- Check if version matches before using cached data

### HTTP Caching

#### Cache-Control Headers

- **max-age**: Seconds the response remains fresh
- **s-maxage**: Like max-age but for shared caches
- **no-cache**: Must revalidate with server before using cached version
- **no-store**: Don't cache at all
- **public**: Response can be stored by any cache
- **private**: Response is for a single user only

```http
Cache-Control: max-age=3600, public
```

### Caching Challenges

#### Cache Invalidation

- Determining when and how to invalidate cached items
- Ensuring all relevant caches are invalidated
- Balancing freshness vs. performance

```text
"There are only two hard things in Computer Science: cache invalidation and naming things."
- Phil Karlton
```

#### Cache Stampede/Thundering Herd

When many requests simultaneously try to refresh an expired cache item

**Solutions:**

- **Lock/Mutex**: Only one request regenerates the cache
- **Stale-While-Revalidate**: Serve stale content while refreshing asynchronously
- **Probabilistic Early Expiration**: Randomly refresh before expiration
- **Sliding Expiration Window**: Reset TTL on access

---

---

---

## 12 - Content Delivery Networks (CDNs) & Edge Computing

### Introduction to CDNs

A Content Delivery Network (CDN) is a distributed network of servers deployed in multiple data centers across different geographic locations. CDNs are designed to deliver content to end-users with high availability and performance by serving content from edge servers that are physically closer to users than origin servers.

### Core CDN Concepts

#### Purpose and Benefits

- **Improved Performance**: Reduced latency through geographic proximity
- **Increased Reliability**: Distributed architecture minimizes single points of failure
- **Scalability**: Better handling of traffic spikes and large audience sizes
- **Cost Efficiency**: Reduced origin server load and bandwidth costs
- **Security**: Protection against DDoS attacks and other threats

#### Key CDN Components

- **Edge Locations/Points of Presence (PoPs)**: Distributed servers in various geographic locations
- **Origin Server**: The original source of content (your web server or cloud storage)
- **Distribution Network**: The infrastructure connecting edge locations and origin
- **Cache Servers**: Specialized servers optimized for content delivery
- **Control Plane**: Management and configuration systems

#### How CDNs Work

1. User requests content from a website/application
2. DNS routes the request to the nearest CDN edge server
3. The edge server checks its cache for requested content
4. If content is in cache (cache hit), it's delivered directly to the user
5. If content is not cached (cache miss), the edge server requests it from the origin
6. The edge server caches the content and delivers it to the user
7. Subsequent requests for the same content are served from the edge cache

### CDN Architecture Types

#### Traditional Pull CDNs

In a pull CDN, content is "pulled" from the origin server when first requested by a user and not found in the edge cache.

**Characteristics:**

- Content is cached on-demand (lazy loading)
- Origin server remains the source of truth
- Automatic cache population based on user requests
- Better suited for frequently changing content

**Examples:** Cloudflare, Amazon CloudFront (in pull mode), Akamai

#### Push CDNs

In a push CDN, content is proactively "pushed" to the edge servers before users request it.

**Characteristics:**

- Content is uploaded to CDN in advance
- Better for static content that doesn't change frequently
- Provides more control over what's cached and when
- Requires explicit cache invalidation when content changes
- Often used for large media files, software downloads, etc.

**Examples:** Amazon CloudFront (in push mode), Azure CDN

### CDN Content Types

#### Static Content Delivery

Static content doesn't change between user requests and is ideal for CDN caching:

- **Images and Graphics**: JPG, PNG, GIF, SVG, WebP
- **CSS and JavaScript Files**: Style sheets and client-side scripts
- **Downloadable Files**: PDFs, documents, software installation files
- **Audio and Video Files**: MP3, MP4, WebM
- **Web Fonts**: WOFF, WOFF2, TTF
- **HTML**: Static web pages

**Optimization techniques:**

- File compression (Gzip, Brotli)
- Image optimization (WebP, responsive images)
- Minification of CSS/JavaScript
- Bundling resources
- Setting appropriate cache TTLs

#### Dynamic Content Acceleration

While traditionally challenging to cache, modern CDNs offer ways to optimize dynamic content:

- **Edge Computing**: Running logic at edge locations
- **Dynamic Page Caching**: Caching portions of dynamic pages
- **API Response Caching**: Caching API results with appropriate invalidation
- **Route Optimization**: Optimizing network paths between origin and edge
- **Connection Reuse**: Maintaining persistent connections to origin

### Edge Computing

Edge computing extends the CDN concept by not just caching content but also running code at edge locations closer to users.

#### Edge Computing vs. Traditional Cloud

| Traditional Cloud                  | Edge Computing                 |
| ---------------------------------- | ------------------------------ |
| Centralized processing             | Distributed processing         |
| Higher latency                     | Lower latency                  |
| Potentially higher bandwidth costs | Reduced bandwidth requirements |
| Consistent resources               | Variable resources by location |
| Easier management                  | More complex orchestration     |

#### Edge Computing Use Cases

- **Real-time Data Processing**: IoT sensors, streaming analytics
- **Content Personalization**: User-specific content generation
- **Authentication and Authorization**: Security checks closer to users
- **Image and Video Manipulation**: On-the-fly resizing, format conversion
- **A/B Testing**: Different content variations served from the edge
- **Geofencing**: Location-based content restrictions
- **Edge SEO**: Dynamic metadata generation
- **Bot Detection**: Identifying and blocking malicious traffic

#### Edge Functions/Workers

Many CDNs now offer serverless functions that run at the edge:

#### CDN Edge Computing Platforms

- **Cloudflare Workers**: JavaScript execution at the edge
- **AWS Lambda@Edge**: Lambda functions at CloudFront edge locations
- **Akamai EdgeWorkers**: JavaScript runtime at Akamai edge servers
- **Fastly Compute@Edge**: WebAssembly-based edge computing
- **Vercel Edge Functions**: Serverless edge functions integrated with Vercel deployments

### Proxy Servers and CDNs

CDNs function as a specialized form of reverse proxy, but understanding the broader proxy server concepts helps clarify their role.

#### Types of Proxies

##### Forward Proxy

Acts on behalf of clients, forwarding their requests to servers.

**Use cases:**

- Access control (corporate firewalls)
- Privacy and anonymity
- Bypassing geographical restrictions
- Content filtering

##### Reverse Proxy

Acts on behalf of servers, receiving client requests and forwarding them to appropriate backend servers.

**Use cases:**

- Load balancing
- SSL termination
- Caching
- Compression
- Security (WAF)

##### CDN as a Specialized Reverse Proxy

CDNs extend the reverse proxy concept with:

- Geographically distributed edge locations
- Advanced caching mechanisms
- Built-in security features
- Traffic routing optimizations
- Edge computing capabilities

### Popular CDN Providers

#### Global CDN Providers

- **Cloudflare**: Robust security features and Workers edge computing
- **Amazon CloudFront**: Integrated with AWS services
- **Akamai**: Enterprise-focused with extensive global network
- **Fastly**: Developer-friendly with powerful edge computing
- **Google Cloud CDN**: Integrated with Google Cloud
- **Microsoft Azure CDN**: Integrated with Azure services
- **StackPath**: Security-focused CDN

---

---

---

## 13 - Proxies and Load Balancers

#### Forward Proxy vs. Reverse Proxy

**Forward Proxy:**

- Sits between clients and the internet
- Represents clients to external servers
- Key uses:
  - Anonymize client requests
  - Enforce access policies
  - Cache content to improve performance
  - Bypass geo-restrictions

**Reverse Proxy:**

- Sits between clients and backend servers
- Represents servers to clients
- Key uses:
  - Load balancing
  - SSL termination
  - Caching
  - Security (WAF, DDoS protection)

### Load Balancing Fundamentals

Load balancing distributes incoming traffic across multiple servers to ensure:

- High availability
- Scalability
- Reliability

#### Essential Load Balancing Algorithms

1. **Round Robin**

   - Routes requests sequentially across servers
   - Simple but doesn't account for server load
   - Example: Request 1 → Server A, Request 2 → Server B, etc.

2. **Least Connections**

   - Routes to server with fewest active connections
   - Better for varying request durations
   - Prevents overloading busy servers

3. **IP Hash**

   - Uses client IP to determine server
   - Ensures client always reaches same server
   - Critical for session persistence

4. **Weighted Algorithms**
   - Assigns capacity values to servers
   - More powerful servers receive proportionally more traffic
   - Example: Server A (weight 3) gets 3x traffic of Server B (weight 1)

### Layer 4 vs. Layer 7 Load Balancing

**Layer 4 (Transport):**

- Routes based on IP address and TCP/UDP ports
- Faster, less resource-intensive
- Limited insight into application data
- Example: HAProxy in TCP mode, AWS NLB

**Layer 7 (Application):**

- Routes based on HTTP headers, cookies, application data
- More intelligent routing decisions
- Higher resource requirements
- Example: NGINX, HAProxy in HTTP mode, AWS ALB

### Health Checks

**Active Monitoring:**

- Load balancer periodically checks backend servers
- Removes unhealthy servers from rotation
- Basic check: TCP connection
- Advanced check: Application-specific response

**Passive Monitoring:**

- Monitors actual client connections
- Marks server as failed after consecutive errors

### Session Persistence

**Problem:** Stateful applications need consistent routing to same server

**Solutions:**

- **Cookie-based:** Inserts cookie to identify server
- **IP-based:** Uses client IP (less reliable with mobile clients)
- **Application-controlled:** Application manages session data externally (Redis, DB)

### Common Load Balancing Patterns

1. **High Availability Pairs**

   - Two load balancers (active/passive)
   - Heartbeat connection between them
   - Failover if primary goes down

2. **Global Server Load Balancing (GSLB)**

   - Distributes traffic across multiple data centers
   - Uses DNS or anycast routing
   - Provides geographic redundancy

3. **Direct Server Return (DSR)**
   - Response traffic bypasses load balancer
   - Reduces load balancer bandwidth requirements
   - More complex to implement

### Popular Proxy & Load Balancing Solutions

**Software:**

- **NGINX:** Web server, reverse proxy, load balancer
- **HAProxy:** High-performance TCP/HTTP load balancer
- **Envoy:** Modern L7 proxy designed for microservices
- **Traefik:** Auto-discovering, cloud-native load balancer

**Hardware:**

- F5 BIG-IP
- Citrix ADC (formerly NetScaler)

**Cloud Services:**

- AWS: Elastic Load Balancing (ALB, NLB, CLB)
- GCP: Cloud Load Balancing
- Azure: Application Gateway, Load Balancer

### SSL Termination

- Load balancer handles SSL/TLS encryption/decryption
- Backend servers receive unencrypted traffic
- Reduces computational load on application servers
- Centralizes certificate management

### Common Challenges & Solutions

1. **Single Point of Failure**

   - Solution: Load balancer redundancy (active/passive pair)

2. **Uneven Load Distribution**

   - Solution: Dynamic weighting, least connections algorithm

3. **Session Management**

   - Solution: Sticky sessions, shared session storage

4. **SSL Performance**
   - Solution: SSL acceleration hardware, session caching

### Monitoring Metrics That Matter

- **Throughput:** Requests/second
- **Latency:** Response time
- **Connection counts:** Active and idle
- **Error rates:** 4xx, 5xx responses
- **Health check status:** Up/down state of backend servers

### When to Scale

- **Vertical scaling:** More powerful load balancer hardware/instances
- **Horizontal scaling:** Multiple load balancers with DNS round-robin or GSLB
- **Consider scaling when:**
  - CPU/memory utilization consistently above 70%
  - Connection queues building up
  - Increased latency under load

---

---

---

## 14 - Consistent Hashing

### The Challenge of Distributed Data Placement

Imagine you have a large collection of data items that need to be distributed across multiple servers. How do you decide which server should handle which data item? This decision becomes particularly challenging when servers are added or removed from the system.

### Traditional Approach and Its Limitations

**Traditional Method:** Using modulo arithmetic (`hash(key) % number_of_servers`)

**Example:**

- With 4 servers (S0, S1, S2, S3) and keys hashed between 0-999
- Key "user_profile_123" hashes to 857
- 857 % 4 = 1, so it goes to server S1

**Problem:**
When you add a 5th server, almost all keys need to be remapped:

- Now 857 % 5 = 2, so it moves to server S2
- In fact, approximately 80% of all keys would need to be moved!

### Consistent Hashing

#### Conceptual Model: The Hash Ring

Imagine a circular ring numbered from 0 to 359 degrees (like a compass). Both servers and data keys are placed on this ring using a hash function.

**How it works:**

1. Place each server at positions on the ring based on their hash values
2. For each data key, find its position on the ring
3. Move clockwise from the key's position and assign it to the first server encountered

**Concrete Example:**

- Server A hashes to position 80 on the ring
- Server B hashes to position 160
- Server C hashes to position 240
- Server D hashes to position 320

If we have data keys that hash to positions 30, 110, 200, and 290:

- Key at position 30 goes to Server A (next server clockwise)
- Key at position 110 goes to Server B
- Key at position 200 goes to Server C
- Key at position 290 goes to Server D

**Server Addition Example:**

- Add Server E at position 140
- Now the key at position 110 will move to Server E
- All other key assignments remain the same!

**Server Removal Example:**

- Remove Server B (at position 160)
- Only the keys between positions 80-160 are affected; they now go to Server C
- All other key assignments remain unchanged

#### Virtual Nodes

**Problem:** With few servers, distribution can be unbalanced

**Solution:** Each physical server is represented by multiple points on the ring

**Example:**

- Instead of just "Server A," we have "Server A-1," "Server A-2," ... "Server A-100"
- Each gets placed at different positions on the ring
- This spreads the load more evenly

#### Real-World Analogy

Think of consistent hashing like postal delivery zones. Each server is responsible for a "zone" on the ring. When a new post office opens, it only takes over a portion of one existing zone, rather than changing all zone boundaries.

### Rendezvous Hashing (Highest Random Weight)

#### Conceptual Model: Scoring Contest

Imagine that for each data item, all servers compete in a "contest" to determine who will store that item. The contest is deterministic but unique for each key-server pair.

**How it works:**

1. For a given data key, each server generates a score based on hash(server_id + key)
2. The server with the highest score "wins" the key

**Concrete Example:**
With servers A, B, C, D and data key "user_123":

| Server | Score for "user_123" |
| ------ | -------------------- |
| A      | 82                   |
| B      | 45                   |
| C      | 96                   |
| D      | 31                   |

- Server C has the highest score, so it gets assigned "user_123"

For a different key "product_456":

| Server | Score for "product_456" |
| ------ | ----------------------- |
| A      | 74                      |
| B      | 91                      |
| C      | 23                      |
| D      | 57                      |

- Server B wins this "contest" and gets assigned "product_456"

**Server Addition Example:**

- Add Server E which scores 88 for "user_123" and 42 for "product_456"
- Since E's score (88) for "user_123" is less than C's score (96), "user_123" stays on server C
- Since E's score (42) for "product_456" is less than B's score (91), "product_456" stays on server B
- Only keys where the new server gets the highest score will move

**Server Removal Example:**

- Remove Server C
- For "user_123", we need to find the next highest score, which is Server A (82)
- Only keys previously assigned to Server C need to be reassigned

#### Real-World Analogy

Think of rendezvous hashing like a specialized job assignment. Each data item is a job with unique requirements, and each server has different skills for different jobs. The server that's the best match for a particular job gets assigned that job.

### Practical Applications

#### Consistent Hashing Applications:

- **CDN:** Akamai uses consistent hashing to determine which edge server should cache specific content
- **Distributed Databases:** Cassandra and DynamoDB use consistent hashing for data partitioning
- **Caching Systems:** Memcached clients use consistent hashing to distribute cache entries

#### Rendezvous Hashing Applications:

- **Content-addressable Storage:** Systems like Ceph use rendezvous hashing for object placement
- **Load Balancers:** Some advanced load balancers use rendezvous hashing for request distribution
- **Peer-to-peer Networks:** Used to determine which peer should store particular content

### When to Choose Which

- **Choose Consistent Hashing when:**

  - You need a well-established algorithm with extensive literature and tooling
  - The ring structure provides useful properties for your application
  - Memory overhead isn't a significant concern

- **Choose Rendezvous Hashing when:**
  - You want a simpler implementation
  - Memory efficiency is important
  - You need natural load balancing without virtual nodes

---

---

---

## 15 - SQL

### SQL (Structured Query Language)

SQL is a domain-specific language used for managing and manipulating relational databases. It serves as the standard language for relational database management systems (RDBMS).

#### Core Components of SQL

##### 1. Data Definition Language (DDL)

Commands that define and modify database structure:

- `CREATE`: Create databases, tables, indexes, views
- `ALTER`: Modify existing database objects
- `DROP`: Remove database objects
- `TRUNCATE`: Remove all records from a table

##### 2. Data Manipulation Language (DML)

Commands that manipulate data within tables:

- `SELECT`: Retrieve data from one or more tables
- `INSERT`: Add new records
- `UPDATE`: Modify existing records
- `DELETE`: Remove records

##### 3. Data Control Language (DCL)

Commands that control access to data:

- `GRANT`: Give privileges to users
- `REVOKE`: Remove privileges from users

##### 4. Transaction Control Language (TCL)

Commands that manage transactions:

- `COMMIT`: Save changes permanently
- `ROLLBACK`: Restore to the last commit point
- `SAVEPOINT`: Create points to roll back to

#### Key SQL Concepts

##### Joins

Connect rows from multiple tables based on related columns:

- **INNER JOIN**: Returns records with matching values in both tables
- **LEFT JOIN**: Returns all records from the left table and matching records from the right
- **RIGHT JOIN**: Returns all records from the right table and matching records from the left
- **FULL JOIN**: Returns all records when there is a match in either table

##### Indexes

Special data structures that improve the speed of data retrieval operations:

- Make queries faster but can slow down write operations
- Primary keys are automatically indexed

##### Constraints

Rules enforced on data columns:

- **PRIMARY KEY**: Uniquely identifies each record
- **FOREIGN KEY**: Ensures referential integrity
- **UNIQUE**: Ensures all values in a column are different
- **CHECK**: Ensures values meet specified conditions
- **NOT NULL**: Ensures a column cannot have NULL value

##### Aggregate Functions

Operations that perform calculations on sets of values:

- `COUNT()`: Counts rows
- `SUM()`: Calculates total
- `AVG()`: Calculates average
- `MIN()`: Finds minimum value
- `MAX()`: Finds maximum value

### B+ Trees

B+ Trees are self-balancing tree data structures that maintain sorted data and allow for efficient insertion, deletion, and search operations. They are the most common implementation for indexes in database systems.

#### Structure of B+ Trees

##### 1. Nodes

- **Root Node**: The top node of the tree
- **Internal Nodes**: Contain keys and pointers to child nodes
- **Leaf Nodes**: Store actual data or pointers to data

##### 2. Properties

- All leaf nodes are at the same level (balanced tree)
- Each node contains between m/2 and m children (where m is the order of the tree)
- Leaf nodes are linked together in a linked list (crucial for range queries)
- All keys are present in leaf nodes

#### B+ Tree Operations

##### Search Operation

1. Start at the root node
2. For each level, find the appropriate subtree based on key comparison
3. Continue until reaching a leaf node
4. Scan the leaf node for the target key

##### Range Queries

1. Search for the lower bound key
2. Once found in a leaf node, traverse the linked list of leaf nodes
3. Continue until reaching the upper bound
4. Time complexity: O(log n + k) where k is the number of elements in the range

#### Advantages in Database Systems

- **Shallow depth**: Most databases can find any row with 3-4 disk reads
- **Sequential access**: Leaf nodes link enables efficient range scans
- **Space utilization**: High branching factor minimizes wasted space
- **Self-balancing**: Maintains performance even with frequent changes

### ACID Properties

ACID is an acronym that represents a set of properties ensuring reliable processing of database transactions.

#### The Four ACID Properties

##### 1. Atomicity

- Transactions are "all or nothing"
- If any part fails, the entire transaction fails (rollback)
- The database state remains unchanged if a transaction fails

**Example**: A bank transfer must either complete fully (debit one account and credit another) or not happen at all. Partial completion is not acceptable.

##### 2. Consistency

- Transactions only transition the database from one valid state to another
- All constraints, triggers, and rules must be satisfied
- Data integrity is preserved

**Example**: If a table has a constraint that account balances cannot be negative, any transaction resulting in a negative balance will be rejected.

##### 3. Isolation

- Concurrent transactions do not interfere with each other
- Results of a transaction are invisible to other transactions until completed
- Prevents "dirty reads," "non-repeatable reads," and "phantom reads"

**Example**: When two users update the same data simultaneously, isolation ensures one user's changes don't overwrite or interfere with the other's.

##### 4. Durability

- Once a transaction is committed, it remains so
- Changes survive system failures (power outages, crashes)
- Typically implemented using transaction logs

**Example**: After confirming a payment, the data is permanently stored even if the database crashes immediately afterward.

#### ACID vs. BASE

Modern distributed systems sometimes use BASE (Basically Available, Soft state, Eventually consistent) as an alternative to ACID:

| ACID                 | BASE                        |
| -------------------- | --------------------------- |
| Strong consistency   | Eventual consistency        |
| High isolation       | Lower isolation             |
| Focus on reliability | Focus on availability       |
| Traditional RDBMS    | Often used in NoSQL systems |

---

---

---

## 16 - NoSQL

NoSQL ("Not Only SQL") databases are non-relational database systems designed to handle various data models, provide horizontal scalability, and deliver high performance for specific use cases.

#### Key Characteristics

- **Schema Flexibility**: Dynamic or no schema requirements
- **Horizontal Scalability**: Ability to scale across multiple servers
- **High Availability**: Designed for distributed environments
- **Eventual Consistency**: Often prioritize availability over consistency
- **Specialized Workloads**: Optimized for specific data patterns

#### NoSQL Data Models

##### 1. Key-Value Stores

The simplest NoSQL model, storing data as key-value pairs:

- **Structure**: Each item is stored as an attribute name (key) with its value
- **Performance**: Extremely fast lookups by key
- **Use Cases**: Caching, session storage, user preferences

##### 2. Document Stores

Store semi-structured data in document format (typically JSON or BSON):

- **Structure**: Collections of documents, each with unique structure
- **Flexibility**: Schema-free with nested data structures
- **Querying**: Rich query capabilities on document contents
- **Use Cases**: Content management, user profiles, real-time analytics

##### 3. Column-Family Stores

Store data in column families - groups of related data:

- **Structure**: Rows with dynamic columns grouped into families
- **Performance**: Optimized for queries over large datasets
- **Sparsity**: Efficiently handles sparse data
- **Use Cases**: Time-series data, logging systems, heavy write workloads

##### 4. Graph Databases

Specialized for highly connected data:

- **Structure**: Nodes (entities), edges (relationships), and properties
- **Traversal**: Optimized for relationship queries and traversals
- **Use Cases**: Social networks, recommendation engines, fraud detection

### Popular NoSQL Database Examples

#### Key-Value Stores

- **Redis**: In-memory store with data structures (strings, lists, sets)
- **Amazon DynamoDB**: Fully managed, multi-region, multi-active database
- **etcd**: Distributed key-value store for configuration data

#### Document Stores

- **MongoDB**: General-purpose document database
- **Couchbase**: Distributed multi-model database
- **Amazon DocumentDB**: MongoDB-compatible document database
- **Firestore**: Google's document database with real-time capabilities

#### Column-Family Stores

- **Apache Cassandra**: Distributed wide-column store
- **HBase**: Hadoop's distributed column-family database
- **ScyllaDB**: High-performance Cassandra-compatible database

#### Graph Databases

- **Neo4j**: Native graph database with Cypher query language
- **Amazon Neptune**: Fully managed graph database service
- **JanusGraph**: Distributed graph database with multi-model support
- **TigerGraph**: Scalable graph database for deep link analytics

### Graph Databases in Depth

Graph databases excel at managing highly connected data by focusing on the relationships between entities.

#### Core Components

- **Nodes**: Represent entities (people, products, accounts)
- **Edges**: Represent relationships between nodes
- **Properties**: Attributes on both nodes and edges

#### Advantages of Graph Databases

- **Relationship Performance**: Traversing connections is efficient
- **Schema Flexibility**: Easily adapt to changing business requirements
- **Intuitive Modeling**: Natural representation of real-world relationships
- **Query Simplicity**: Complex relationship queries are concise

#### Common Use Cases

- **Social Networks**: Friend connections, influence mapping
- **Recommendation Engines**: Product, content, and friend suggestions
- **Knowledge Graphs**: Connecting concepts and information
- **Fraud Detection**: Identifying suspicious relationship patterns
- **Network/IT Operations**: Infrastructure dependency mapping

### BASE Properties

BASE is an alternative to ACID for distributed systems, emphasizing availability over consistency:

- **Basically Available**: System guarantees availability
- **Soft state**: State may change over time, even without input
- **Eventually consistent**: System will become consistent over time

#### ACID vs. BASE Comparison

| ACID                            | BASE                            |
| ------------------------------- | ------------------------------- |
| Strong consistency              | Eventual consistency            |
| Isolation                       | Availability first              |
| Focus on reliability            | Focus on scalability            |
| Pessimistic approach            | Optimistic approach             |
| Difficulty scaling horizontally | Designed for horizontal scaling |

### Scaling Strategies for Databases

#### Vertical Scaling (Scale Up)

- Add more resources (CPU, RAM, storage) to existing servers
- **Pros**: Simpler to implement, no distribution complexity
- **Cons**: Hardware limits, single point of failure, cost

#### Horizontal Scaling (Scale Out)

- Add more servers to distribute the load
- **Pros**: Virtually unlimited scaling potential, fault tolerance
- **Cons**: Increased complexity, data distribution challenges

#### Read Replicas

- Create copies of data optimized for read operations
- **Pros**: Scales read operations, improved read performance
- **Cons**: Replication lag, eventual consistency

#### Sharding

Sharding divides data across multiple servers, with each server responsible for a subset of the data.

##### Sharding Strategies

1. **Range-Based Sharding**

   - Divides data based on ranges of a key
   - **Example**: Users A-M on shard 1, N-Z on shard 2
   - **Pros**: Simple to implement, efficient range queries
   - **Cons**: Potential for hot spots, uneven distribution

2. **Hash-Based Sharding**

   - Uses a hash function to distribute data evenly
   - **Example**: hash(user_id) % num_shards determines placement
   - **Pros**: Even distribution, prevents hot spots
   - **Cons**: Difficult to perform range queries, resharding complexity

3. **Directory-Based Sharding**

   - Uses a lookup service to track data location
   - **Example**: Shard manager service that maps keys to shards
   - **Pros**: Flexible, allows for dynamic resharding
   - **Cons**: Lookup service becomes single point of failure

4. **Geographically-Based Sharding**
   - Locates data near its users
   - **Example**: European users on EU servers, US users on US servers
   - **Pros**: Reduced latency for users, regulatory compliance
   - **Cons**: Cross-region operations can be slow

##### Sharding Challenges

- **Joins Across Shards**: Complex and performance-intensive
- **Distributed Transactions**: Difficult to maintain ACID across shards
- **Resharding**: Redistributing data as system grows
- **Hotspots**: Uneven load distribution
- **Entity Groups**: Related data should reside on the same shard

### NoSQL vs. SQL: When to Choose Each

#### Choose NoSQL When:

- Handling semi-structured or unstructured data
- Requiring horizontal scalability for large datasets
- Needing schema flexibility for evolving data models
- Prioritizing availability over strong consistency
- Building applications with specific data access patterns (graph, time-series, etc.)

#### Choose SQL When:

- Working with structured data and fixed schemas
- Requiring complex joins and transactions
- Needing strong consistency guarantees (ACID)
- Working with moderate data volumes
- Requiring standardized query language (SQL)

---

---

---

## 17 - Replication and Sharding

### Replication

#### Overview

Replication is the process of storing copies of the same data on multiple machines. This provides:

- **Redundancy**: Protection against hardware failure
- **Improved performance**: Lower latency by serving data from geographically closer nodes
- **Increased availability**: System remains operational even if some nodes fail

#### Leader-Follower Replication

- **Leader (Primary)**: Handles all write operations
- **Followers (Replicas)**: Receive copies of data changes from the leader
- **Write path**: Client → Leader → Followers
- **Read path**: Either from leader or any follower

#### Multi-Leader Replication

- Multiple nodes can accept writes
- Useful for multi-datacenter deployments
- Introduces complexity in conflict resolution

#### Leaderless Replication

- Any replica can accept writes
- Client sends write to multiple replicas
- Read requests sent to multiple replicas (quorum reads)
- Examples: Amazon Dynamo, Cassandra

### Synchronous vs Asynchronous Replication

#### Synchronous Replication

- Leader waits for confirmation from followers before acknowledging write
- **Pros**: Strong consistency guarantees
- **Cons**: Higher latency, system availability depends on follower availability

#### Asynchronous Replication

- Leader doesn't wait for follower confirmation
- **Pros**: Lower latency, higher availability
- **Cons**: Possible data loss if leader fails before replication completes

#### Semi-Synchronous Replication

- Leader waits for confirmation from at least one follower
- Balance between consistency and availability

### Sharding

#### Overview

Sharding (also called partitioning) splits a large dataset across multiple machines to distribute load and improve scalability.

#### Sharding Strategies

##### Range-Based Sharding

- Divides data into continuous ranges based on a key
- **Pros**: Simple to implement, efficient range queries
- **Cons**: Risk of hot spots if data is not evenly distributed

##### Hash-Based Sharding

- Uses a hash function to determine data placement
- **Pros**: Even data distribution
- **Cons**: Range queries become inefficient, requires hash calculation

##### Directory-Based Sharding

- Maintains a lookup service that tracks data location
- **Pros**: Flexible, can dynamically change shard allocation
- **Cons**: Lookup service can become a bottleneck

#### Rebalancing Strategies

- **Hash ring**: Consistent hashing minimizes data movement when nodes are added/removed
- **Fixed number of partitions**: Create more partitions than nodes, move entire partitions during rebalancing
- **Dynamic partitioning**: Split busy partitions, merge quiet ones

### Combining Replication and Sharding

- Each shard is typically replicated for fault tolerance
- Creates a two-dimensional distribution: horizontal (sharding) and vertical (replication)
- Improves both scalability and availability

### Common Challenges

#### Consistency Issues

- CAP theorem: Can only guarantee two of Consistency, Availability, and Partition tolerance
- Read-after-write consistency
- Monotonic reads
- Eventual consistency

#### Rebalancing Overhead

- Data movement consumes network bandwidth
- May impact performance during rebalancing

#### Hot Spots

- Uneven data or access patterns
- Solutions: better sharding keys, caching, application-level workarounds

### Popular Implementations

#### Database Systems

- MongoDB: Auto-sharding, replica sets
- Cassandra: Consistent hashing, tunable consistency
- MySQL Cluster: Auto-sharding with shared-nothing architecture

#### Caching Systems

- Redis Cluster: Hash slots for sharding
- Memcached: Client-side sharding

#### Storage Systems

- HDFS: Data replication without sharding
- Ceph: Dynamic subtree partitioning with replication

---

---

---

## 18 - CAP Theorem

### Overview

The CAP theorem, formulated by Eric Brewer in 2000, states that a distributed data system can only guarantee at most two out of the following three properties simultaneously:

- **C**onsistency
- **A**vailability
- **P**artition tolerance

### The Three Properties Explained

#### Consistency

- All nodes see the same data at the same time
- Every read receives the most recent write or an error
- Ensures that data is identical across all nodes in the system
- Similar to the concept of linearizability or atomic consistency

#### Availability

- Every request to a non-failing node receives a response
- The system remains operational and responsive even during failures
- No request can be left hanging without a response
- Does not guarantee that the response contains the most recent data

#### Partition Tolerance

- The system continues to operate despite network partitions
- Network partitions occur when nodes cannot communicate with each other
- Messages between nodes may be delayed or lost
- Essential property for distributed systems that operate across networks

### CAP Theorem in Practice

#### CA Systems (Consistency + Availability)

- Sacrifice partition tolerance
- Examples: Traditional RDBMSs (PostgreSQL, MySQL with single-node setup)
- Not truly distributed as they cannot handle network partitions
- Rarely achievable in real-world distributed environments

#### CP Systems (Consistency + Partition Tolerance)

- Sacrifice availability during partitions
- Examples: MongoDB, HBase, Redis (in certain configurations)
- May become unavailable during network partitions to maintain consistency
- Choose consistency over availability when data accuracy is critical

#### AP Systems (Availability + Partition Tolerance)

- Sacrifice strong consistency
- Examples: Cassandra, Amazon Dynamo, CouchDB
- Provide eventual consistency instead of immediate consistency
- Choose availability over consistency when system uptime is critical

#### Consistency Models

- **Strong Consistency**: All reads reflect the latest write
- **Eventual Consistency**: All replicas eventually converge given enough time
- **Causal Consistency**: Operations causally related must be seen in the same order
- **Read-your-writes Consistency**: A client always sees its own writes

### Practical Considerations

#### System Design Tradeoffs

- Business requirements should guide CAP choices
- Financial systems may prioritize consistency
- Social media may prioritize availability

#### Handling Partition Scenarios

- Detect partitions quickly
- Define recovery procedures when partitions heal
- Consider using compensating transactions

#### Regional Considerations

- Multi-region deployments face CAP challenges more acutely
- Geographic distance increases partition probability
- Consider region-specific consistency requirements

---

---

---

## 19 - Object Storage

### Overview

Object storage is a data storage architecture that manages data as discrete units called objects, rather than as files in a hierarchical file system or blocks in a block storage system. Objects are stored in a flat address space, making object storage highly scalable and well-suited for unstructured data.

### Key Characteristics

#### Object Structure

- **Data**: The actual content being stored (document, image, video, etc.)
- **Metadata**: Descriptive information about the object (creation date, size, custom attributes)
- **Unique Identifier**: Globally unique ID or key used to retrieve the object

#### Architecture

- **Flat Namespace**: No hierarchical directory structure. uses flat namespace with globally unique identifiers
- **RESTful APIs**: Typically accessed via HTTP-based APIs (GET, PUT, DELETE)
- **Web-Scale Design**: Built for massive scalability (petabytes and beyond)
- **Distributed**: Data distributed across multiple nodes and often multiple locations

### Advantages

- **Unlimited Scalability**: Can scale horizontally to exabytes of data
- **Cost-Effectiveness**: Often less expensive than block or file storage for large datasets
- **Data Durability**: Multiple copies of data stored across different devices/locations
- **Rich Metadata**: Extended object information beyond basic file attributes
- **Global Access**: Objects accessible from anywhere via HTTP(S)
- **Immutability**: Objects typically aren't modified but replaced entirely

### Limitations

- **Performance**: Generally higher latency than block storage
- **Limited File System Operations**: No direct file locking or appending
- **Not Suitable for Databases**: Poor fit for transactional workloads
- **Limited OS Integration**: Not directly mountable as a traditional file system

### Use Cases

- **Static Content**: Websites, images, videos, documents
- **Backup and Archive**: Long-term retention of data
- **Data Lakes**: Storage for big data analytics
- **Cloud-Native Applications**: Microservices and containerized applications
- **IoT Data Storage**: Sensor data and device telemetry
- **Media and Entertainment**: Large media file storage

### Storage Classes & Tiering

- **Hot Storage**: Frequently accessed data with low latency
- **Cool Storage**: Infrequently accessed data with slightly higher latency
- **Cold Storage**: Rarely accessed data with higher retrieval times
- **Archive Storage**: Long-term preservation with highest retrieval times
- **Auto-tiering**: Automatic movement between tiers based on access patterns

### Popular Object Storage Implementations

#### Public Cloud

- **Amazon S3** (Simple Storage Service)
- **Google Cloud Storage**
- **Microsoft Azure Blob Storage**
- **IBM Cloud Object Storage**

### Object Storage vs. Other Storage Types

#### Object vs. File Storage

- File storage uses hierarchical directories while object storage uses flat namespace
- File storage offers POSIX compliance while object storage offers custom metadata
- File storage often has lower latency for small files

#### Object vs. Block Storage

- Block storage stores data in fixed-sized blocks while object storage uses variable-sized objects
- Block storage offers better performance for databases and transactions
- Object storage provides better scalability for large unstructured datasets

### Blob Storage

Blob (Binary Large Object) storage is a subset of object storage optimized for storing large unstructured data.

#### Key Features

- **Unstructured Data**: Handles any binary data without enforcing structure
- **Tiered Storage**: Often offers hot, cool, and archive access tiers
- **CDN Integration**: Commonly integrated with Content Delivery Networks
- **Public/Private Access**: Configurable access controls and public URLs

#### Storage Tiers

- **Hot Tier**: Frequent access, higher storage cost, lower access cost
- **Cool Tier**: Infrequent access, lower storage cost, higher access cost
- **Archive Tier**: Rarely accessed data, lowest storage cost, highest retrieval cost

---

---

---

## 20 - Message Queues

### Message Queues: Fundamental Concepts

Message queues are asynchronous communication mechanisms that enable services to communicate without being directly connected. They serve as intermediaries for passing messages between different parts of a system.

#### Core Components

- **Producer**: Application that creates and sends messages to the queue
- **Consumer**: Application that receives and processes messages from the queue
- **Broker**: Middleware that manages the queue and ensures message delivery
- **Queue/Topic**: The storage mechanism where messages reside until processed

#### Key Benefits

- **Decoupling**: Services don't need to know about each other
- **Scalability**: Systems can handle variable loads by adding consumers
- **Resilience**: Messages persist during downstream service outages
- **Asynchronous Processing**: Producers continue without waiting for consumers
- **Load Leveling**: Absorbs traffic spikes, preventing service overload

### Messaging Patterns

#### Publish-Subscribe (Pub/Sub)

In this pattern, publishers send messages to topics, and subscribers receive all messages from the topics they subscribe to.

##### Characteristics

- **One-to-many**: Messages from one publisher go to multiple subscribers
- **Topic-based**: Messages are filtered based on topics
- **Loose coupling**: Publishers don't know who will receive their messages

##### Use Cases

- Event notifications
- Broadcasting updates
- Real-time dashboards
- Log distribution

### Message Delivery Models

#### Push Model

In the push model, the broker actively sends messages to consumers as they arrive.

##### Characteristics

- **Real-time**: Low latency for message delivery
- **Broker-controlled flow**: Broker determines delivery rate
- **Connection-oriented**: Requires persistent connections

##### Advantages

- Lower latency for message delivery
- Simpler consumer implementation
- Good for real-time updates

##### Challenges

- Risk of overwhelming consumers during traffic spikes
- Requires connection management
- May need backpressure mechanisms

#### Pull Model

In the pull model, consumers request messages from the broker when they're ready to process them.

##### Characteristics

- **Consumer-controlled flow**: Consumers determine processing rate
- **Polling-based**: Consumers periodically check for new messages
- **Connection-flexible**: Can work with intermittent connections

##### Advantages

- Consumers control their load
- Natural backpressure mechanism
- Works well with batch processing

##### Challenges

- Potential increased latency
- Polling overhead
- May waste resources if empty polls are frequent

### Fan-Out Pattern

Fan-out distributes a single message to multiple destinations simultaneously.

#### Implementation Approaches

- **Topic-based**: One producer publishes to a topic with multiple subscribers
- **Exchange-based**: Producer sends to an exchange that copies to multiple queues
- **Dedicated queue per consumer**: Each consumer has its own queue that receives copies

#### Use Cases

- Parallel processing of computationally intensive tasks
- Multi-tenancy (sending updates to all tenants)
- Broadcasting notifications across services
- Implementing different views of the same data

### Apache Kafka

Kafka is a distributed streaming platform designed for high-throughput, fault-tolerant, publish-subscribe messaging.

#### Key Concepts

- **Topics**: Categories for message streams
- **Partitions**: Subdivisions of topics for parallelism
- **Consumer Groups**: Groups of consumers that jointly process messages
- **Brokers**: Servers that store and serve messages
- **ZooKeeper/KRaft**: Manages cluster state and configuration

#### Architecture Highlights

- **Log-Based Storage**: Messages are append-only immutable logs
- **Retention Policies**: Data can be kept based on time or size
- **Replication**: Partitions are replicated across brokers for fault tolerance
- **Offsets**: Consumers track their position in each partition

#### Strengths

- Extremely high throughput (millions of messages per second)
- Durable storage with configurable retention
- Horizontal scalability for both producers and consumers
- Strong ordering guarantees within partitions
- Stream processing capabilities (with Kafka Streams)

#### Best Use Cases

- Log aggregation
- Stream processing
- Event sourcing
- Activity tracking
- Metrics collection

### RabbitMQ

RabbitMQ is a message broker implementing the Advanced Message Queuing Protocol (AMQP) with rich routing capabilities.

#### Key Concepts

- **Exchanges**: Receive messages and route them to queues
- **Queues**: Store messages for consumption
- **Bindings**: Rules that connect exchanges to queues
- **Virtual Hosts**: Isolated environments within a broker

#### Exchange Types

- **Direct**: Routes messages to queues based on exact routing key matches
- **Topic**: Routes based on pattern matching of routing keys
- **Fanout**: Broadcasts to all bound queues
- **Headers**: Routes based on message header attributes

#### Strengths

- Flexible routing with multiple exchange types
- Strong delivery guarantees
- Plugin ecosystem
- Support for multiple protocols (AMQP, MQTT, STOMP)
- Management UI out of the box

#### Best Use Cases

- Complex routing requirements
- When message order matters
- RPC-like communication patterns
- When strong delivery guarantees are required

### Comparing Kafka and RabbitMQ

| Feature            | Kafka                          | RabbitMQ                                 |
| ------------------ | ------------------------------ | ---------------------------------------- |
| Primary Pattern    | Pub/Sub with consumer groups   | Flexible (direct, fanout, topic)         |
| Storage Model      | Persistent log                 | Memory with optional persistence         |
| Message Retention  | Configurable (time/size based) | Until consumed (typically)               |
| Throughput         | Very high (millions/sec)       | Moderate to high (tens of thousands/sec) |
| Delivery Model     | Pull-based                     | Push-based by default                    |
| Ordering           | Guaranteed within partitions   | FIFO per queue                           |
| Redelivery         | Consumer managed               | Automatic with acknowledgments           |
| Routing Complexity | Simple (topic/partition)       | Rich (exchanges/bindings)                |
