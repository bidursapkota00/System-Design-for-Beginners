# 06 - DNS, Dynamic DNS, and the DNS Lookup Process

## What is DNS?

**DNS (Domain Name System)** is the internet’s phonebook. It translates human-readable domain names (like `openai.com`) into machine-readable IP addresses (like `104.18.12.123`), enabling devices to locate and communicate with each other.

---

## Key Components of DNS

- **Domain Name**: A readable address like `example.com`.
- **IP Address**: The actual network address (IPv4 or IPv6).
- **DNS Resolver**: A client-side service (usually your ISP) that performs DNS queries.
- **Root DNS Servers**: Top of the DNS hierarchy. Direct queries to TLD servers.
- **TLD Servers**: Handle top-level domains like `.com`, `.org`, `.net`.
- **Authoritative DNS Servers**: Contain the actual IP address for a domain.

---

## What is Dynamic DNS (DDNS)?

**Dynamic DNS** automatically updates DNS records whenever the device's IP changes (especially useful for devices on dynamic IPs, like most home internet connections).

### Use Cases:

- Hosting a server at home (e.g., security camera or web server).
- IoT devices.
- Remote access when IP changes frequently.

### How It Works:

- A DDNS service monitors your IP.
- When it changes, it sends an update to the DNS server.
- Your domain now points to the new IP without manual changes.

---

## DNS Lookup Process (Step-by-Step)

Let’s say a user visits `www.example.com`. Here’s what happens:

### 1. **Browser Cache**

- Browser checks if it has recently resolved `www.example.com`.
- If yes, it uses that cached IP.

### 2. **Operating System Cache**

- If not found in the browser, the OS checks its DNS cache.

### 3. **DNS Resolver (Usually ISP)**

- If not found locally, the query is sent to the configured DNS resolver (e.g., Google’s 8.8.8.8).

### 4. **Root Server Query**

- Resolver asks the **root server** for `.com` TLD info.

### 5. **TLD Server Query**

- Root server responds with TLD server for `.com`.
- Resolver queries this TLD server.

### 6. **Authoritative Server Query**

- TLD responds with the authoritative name server for `example.com`.
- Resolver queries this server.

### 7. **IP Address Returned**

- Authoritative server responds with the IP address of `www.example.com`.

### 8. **Client Connects to Server**

- The resolver sends the IP to the client.
- The browser makes an HTTP/S request to that IP.

### 9. **Caching for Speed**

- All intermediate steps are cached by the resolver and the client for future speed.

---

## Example Timeline:

User types www.example.com ↓
Local browser/OS cache → miss ↓
Query to ISP DNS resolver (or 8.8.8.8) ↓ → Root server (where's .com?) → TLD server (where's example.com?) → Authoritative server (what's www.example.com?) ↓ IP returned (e.g., 93.184.216.34) ↓ Browser loads the website.
