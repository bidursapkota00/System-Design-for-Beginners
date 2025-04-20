# Networking Basics

## Overview

Networking is at the core of any distributed system. This lesson introduces the fundamentals of computer networking, focusing on concepts essential for system design.

## Key Concepts

### 1. IP Addressing

- **IP Address**: Unique identifier for a device on a network (e.g., `192.168.1.1`).
- **IPv4 vs IPv6**: IPv4 is 32-bit (limited addresses), IPv6 is 128-bit (virtually unlimited).
- **Public vs Private IPs**: Public IPs are globally routable, private IPs are used in local networks.

### 2. MAC Address

- Physical address of a network interface card (NIC).
- Used within local networks for communication between devices.

### 3. Ports

- Represent different services on a machine (e.g., port 80 for HTTP, 443 for HTTPS).
- Help distinguish between multiple services running on the same IP address.

### 4. DNS (Domain Name System)

- Resolves human-readable domain names (e.g., `example.com`) into IP addresses.
- Works in a hierarchical manner involving root, TLD, and authoritative servers.

### 5. NAT (Network Address Translation)

- Allows multiple devices in a local network to share a single public IP.
- Common in home and office routers.

### 6. Firewalls

- Filter traffic based on IPs, ports, or protocols.
- Protect networks from unauthorized access or attacks.

## Communication Models

### 1. Client-Server Model

- One device (client) makes requests to another (server), which processes and responds.

### 2. Peer-to-Peer Model

- Devices act as both clients and servers to each other.

## Why It Matters

- Efficient and secure communication between services depends on networking principles.
- Network limits often define system boundaries, latencies, and bottlenecks.

## Summary

Networking basics provide the backbone for all system interactions. A solid grasp of IPs, ports, and DNS is critical for designing and deploying scalable, connected applications.

---
