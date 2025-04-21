# Computer Architecture

## Key Concepts

### 1. CPU (Central Processing Unit)

- The "brain" of the computer.
- Responsible for executing instructions from applications and the operating system.
- Performance measured in clock cycles (GHz), cores, and instruction set architecture (ISA).

### 2. Memory Hierarchy

#### a. Registers

- Fastest memory, inside the CPU.
- Very limited in size, used for immediate instruction execution.

#### b. CPU Cache

- **L1, L2, and L3 caches**: Small, fast memory located close to or inside the CPU.
- Stores frequently accessed data to reduce the need to access slower RAM.
- L1 is fastest but smallest; L3 is slower but larger.

#### c. RAM (Random Access Memory)

- Main memory used to store active processes and data.
- Volatile memory — data is lost when power is off.
- Slower than CPU cache but much larger.

#### d. Storage (Disk)

- Persistent memory (HDD or SSD).
- Used to store applications, files, and data long-term.
- SSDs are much faster than HDDs but slower than RAM.
- Storage affects I/O performance, latency, and throughput.

---

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

## Caching: The Data Journey from Storage to CPU

### Process Flow:

### Explanation:

1. **Storage → RAM**:

   - When a program or data is needed, it is loaded from the disk into RAM.
   - This is relatively slow (milliseconds), especially from HDDs.

2. **RAM → CPU Cache**:

   - The CPU requests specific data. If it's not in the cache (a cache miss), it fetches it from RAM.
   - Caches use algorithms (like LRU - Least Recently Used) to decide which data to keep.

3. **CPU Cache → CPU Registers**:
   - The most frequently accessed values are moved to registers for immediate use by the CPU.

### Cache Efficiency:

- Caches drastically reduce the time it takes for the CPU to access memory.
- A cache hit (data already in cache) is much faster than fetching from RAM or disk.

---
