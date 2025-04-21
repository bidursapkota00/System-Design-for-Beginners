# 03 - Design Requirements

## 1. Core System Actions

### Move Data

- Transferring data between systems, services, or users.
- Examples: sending data from client to server, syncing between services, or replicating between data centers.

### Store Data

- Persisting data for future use.
- Involves choosing between SQL, NoSQL, file systems, or object storage.
- Considerations: durability, scalability, read/write speed, cost.

### Transform Data

- Processing data for various purposes like analytics, formatting, validation, or machine learning.
- Examples: ETL (Extract, Transform, Load) pipelines, data cleaning, real-time processing.

---

## 2. Non-Functional Requirements

### Availability

- The percentage of time the system is operational.
- Measured as uptime (e.g., 99.9%).
- High availability systems minimize downtime using failovers and redundancy.

### Reliability

- The system performs as expected under specific conditions.
- Often involves retry mechanisms, error handling, and graceful degradation.

### Fault Tolerance

- Ability to continue functioning despite component failures.
- Achieved through redundancy, backups, and fallback strategies.

### Redundancy

- Duplication of critical components (data, servers, etc.) to ensure continued operation if one fails.
- Common in high-availability systems and disaster recovery planning.

---

## 3. Performance Metrics

### Throughput

- Amount of work the system can handle over time (e.g., requests/sec).
- Higher throughput = more efficient system.
- Influenced by concurrency, resource allocation, and load balancing.

### Latency

- Time taken to respond to a request.
- Measured in milliseconds.
- Lower latency = faster user experience.
- Influenced by network delays, processing speed, and system load.

---

## 4. Security and Scalability

### DDoS (Distributed Denial of Service)

- Attack where many systems overwhelm a target with traffic.
- Defenses: rate limiting, firewalls, Web Application Firewalls (WAF), and CDNs.

### CDN (Content Delivery Network)

- Distributes content across geographically located servers.
- Reduces latency by serving content closer to the user.
- Also helps absorb traffic spikes (including DDoS attacks).
