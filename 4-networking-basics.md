# 04 - Networking Basics

## Key Concepts

### 1. IP Addressing

- **IP Address**: Unique identifier for a device on a network (e.g., `192.168.1.1`).
- Two major versions: IPv4 (32-bit) and IPv6 (128-bit).

### 2. Public vs Private IP

- **Public IP**:
  - Assigned by ISPs.
  - Routable over the internet.
  - Example: A server hosting a website typically has a public IP.
- **Private IP**:
  - Used within local networks (e.g., homes, offices).
  - Not routable over the public internet.
  - Common ranges: `192.168.x.x`, `10.x.x.x`, `172.16.x.x - 172.31.x.x`.

### 3. Static vs Dynamic IP

- **Static IP**:
  - Manually assigned and remains constant.
  - Useful for servers or devices that need consistent addressing.
- **Dynamic IP**:
  - Automatically assigned by DHCP (Dynamic Host Configuration Protocol).
  - Changes over time or when the device reconnects.

---

## 4. Ports

- Represent different services on a machine.
- Example: `80` for HTTP, `443` for HTTPS, `22` for SSH.
- Enables multiple services to run on the same IP address.

### 5. DNS (Domain Name System)

- Translates domain names like `example.com` to IP addresses.
- DNS Lookup Steps:
  1. Client checks local DNS cache.
  2. If not found, queries a recursive resolver.
  3. Resolver asks the root server → TLD server → Authoritative server.
  4. IP address is returned to the client.

### 6. NAT (Network Address Translation)

- Allows multiple devices on a private network to access the internet using one public IP.
- Commonly used in routers.

### 7. MAC Address

- Hardware identifier for a network interface.
- Used within local area networks (LANs).

### 8. Firewalls

- Control network access based on rules (e.g., block all traffic except on port 443).
- Used to secure systems from unauthorized access.

---

## Communication Models

### Client-Server Model

- Client sends request → Server processes it → Server sends response.
- Example: Web browser (client) requests a web page from a server.

### Peer-to-Peer Model

- Each device can act as both client and server.
- Example: File sharing in torrents.

---

## The Request-Response Lifecycle (Web Example)

### 1. User Action

- A user types `example.com` in the browser.

### 2. DNS Resolution

- Browser checks cache or asks a DNS server to resolve `example.com` to an IP (e.g., `93.184.216.34`).

### 3. TCP Connection

- A TCP connection is established between client and server (usually on port 80 or 443).

### 4. HTTP Request Sent

- The browser sends an HTTP GET request for a web page.

### 5. Server Processing

- Server receives the request, processes it, and prepares a response (e.g., HTML content).

### 6. HTTP Response Sent

- Server sends back the response via TCP to the client.

### 7. Rendering the Page

- Browser receives the HTML, renders the page, and may make additional requests for CSS, JS, images, etc.
