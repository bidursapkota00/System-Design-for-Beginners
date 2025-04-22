# 13 - Proxies and Load Balancers

### Forward Proxy vs. Reverse Proxy

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

## Load Balancing Fundamentals

Load balancing distributes incoming traffic across multiple servers to ensure:

- High availability
- Scalability
- Reliability

### Essential Load Balancing Algorithms

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

## Layer 4 vs. Layer 7 Load Balancing

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

## Health Checks

**Active Monitoring:**

- Load balancer periodically checks backend servers
- Removes unhealthy servers from rotation
- Basic check: TCP connection
- Advanced check: Application-specific response

**Passive Monitoring:**

- Monitors actual client connections
- Marks server as failed after consecutive errors

## Session Persistence

**Problem:** Stateful applications need consistent routing to same server

**Solutions:**

- **Cookie-based:** Inserts cookie to identify server
- **IP-based:** Uses client IP (less reliable with mobile clients)
- **Application-controlled:** Application manages session data externally (Redis, DB)

## Common Load Balancing Patterns

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

## Popular Proxy & Load Balancing Solutions

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

## SSL Termination

- Load balancer handles SSL/TLS encryption/decryption
- Backend servers receive unencrypted traffic
- Reduces computational load on application servers
- Centralizes certificate management

## Common Challenges & Solutions

1. **Single Point of Failure**

   - Solution: Load balancer redundancy (active/passive pair)

2. **Uneven Load Distribution**

   - Solution: Dynamic weighting, least connections algorithm

3. **Session Management**

   - Solution: Sticky sessions, shared session storage

4. **SSL Performance**
   - Solution: SSL acceleration hardware, session caching

## Monitoring Metrics That Matter

- **Throughput:** Requests/second
- **Latency:** Response time
- **Connection counts:** Active and idle
- **Error rates:** 4xx, 5xx responses
- **Health check status:** Up/down state of backend servers

## When to Scale

- **Vertical scaling:** More powerful load balancer hardware/instances
- **Horizontal scaling:** Multiple load balancers with DNS round-robin or GSLB
- **Consider scaling when:**
  - CPU/memory utilization consistently above 70%
  - Connection queues building up
  - Increased latency under load
