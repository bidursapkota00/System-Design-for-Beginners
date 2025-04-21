# 02 - Application Architecture

## Key Architectures

### 1. Monolithic Architecture

- A single unified codebase.
- Easier to develop and deploy initially.
- Becomes harder to scale and maintain as the application grows.

### 2. Microservices Architecture

- Breaks the application into independent services (e.g., auth, payments, catalog).
- Each service has its own codebase and can be deployed, scaled, and updated independently.
- Requires robust inter-service communication (e.g., REST, gRPC) and coordination.

### 3. Client-Server Model

- Client (browser, mobile app) interacts with server via APIs.
- Server processes logic and connects to databases or other services.

### 4. N-Tier Architecture

- **Presentation Layer**: UI and frontend logic.
- **Application Layer**: Business logic and orchestration.
- **Data Layer**: Storage systems like SQL/NoSQL databases.

### 5. Service-Oriented Architecture (SOA)

- Similar to microservices but more tightly integrated with enterprise-level systems.
- Commonly uses middleware like an Enterprise Service Bus (ESB).

---

## Scaling Applications

### Vertical Scaling (Scaling Up)

- Add more CPU, RAM, or storage to a single server.
- Easier but limited by hardware capacity.
- Downtime might be required during scaling.

### Horizontal Scaling (Scaling Out)

- Add more servers to distribute the load.
- Enables near-infinite scalability.
- Often requires stateless services and a load balancer.

---

## Load Balancer

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

## Observability: Logging, Metrics, and Alerts

### Logging

- Records system events, errors, and app-specific information.
- Helps in debugging and post-mortem analysis.
- Tools: ELK stack (Elasticsearch, Logstash, Kibana), Fluentd, Loki.

### Metrics

- Quantitative data like response times, memory usage, request rates.
- Used to monitor system health.
- Tools: Prometheus, Grafana, Datadog.

### Alerts

- Notifications triggered by metrics/log patterns crossing thresholds.
- Critical for timely response to failures or performance degradation.
- Can be routed to emails, Slack, PagerDuty, etc.
