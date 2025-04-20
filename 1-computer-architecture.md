# Computer Architecture

## Overview

This lesson introduces the foundational components of computer architecture that underpin modern system design. Understanding these components is crucial for building scalable, efficient, and reliable systems.

## Key Concepts

### 1. CPU (Central Processing Unit)

- The "brain" of the computer.
- Responsible for executing instructions from applications and the operating system.
- Performance measured in clock cycles (GHz), cores, and instruction set architecture (ISA).

### 2. Memory Hierarchy

- **Registers**: Fastest, located inside the CPU.
- **Cache** (L1, L2, L3): Fast access, stores frequently used data.
- **RAM (Random Access Memory)**: Volatile memory for active processes.
- **Disk Storage**: SSDs or HDDs for persistent storage.

### 3. Disk and Storage

- **HDDs**: Slower, mechanical parts.
- **SSDs**: Faster, solid-state.
- Storage affects I/O performance, latency, and throughput.

### 4. Network Interface

- Facilitates communication with other machines via the Internet.
- Bandwidth and latency are key factors.

### 5. I/O Devices

- Keyboards, displays, etc., but more relevant for system design is how servers interact with disks and networks.

### 6. Bus

- Internal communication system that transfers data between components (CPU, memory, I/O devices).

## Why It Matters in System Design

- Performance bottlenecks often arise from CPU limitations, memory constraints, or slow disk I/O.
- A system designer must understand the hardware to make informed architectural choices (e.g., caching strategies, load balancing, parallelism).

## Common Bottlenecks

- CPU saturation under heavy computation.
- RAM limitations leading to swapping (slow performance).
- Disk I/O constraints in data-intensive applications.
- Network delays impacting distributed systems.

## Summary

A solid understanding of computer architecture helps engineers:

- Optimize systems.
- Choose the right hardware for the workload.
- Understand trade-offs between speed, cost, and scalability.

---
