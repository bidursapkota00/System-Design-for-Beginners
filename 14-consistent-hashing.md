# 14 - Consistent Hashing

## The Challenge of Distributed Data Placement

Imagine you have a large collection of data items that need to be distributed across multiple servers. How do you decide which server should handle which data item? This decision becomes particularly challenging when servers are added or removed from the system.

## Traditional Approach and Its Limitations

**Traditional Method:** Using modulo arithmetic (`hash(key) % number_of_servers`)

**Example:**

- With 4 servers (S0, S1, S2, S3) and keys hashed between 0-999
- Key "user_profile_123" hashes to 857
- 857 % 4 = 1, so it goes to server S1

**Problem:**
When you add a 5th server, almost all keys need to be remapped:

- Now 857 % 5 = 2, so it moves to server S2
- In fact, approximately 80% of all keys would need to be moved!

## Consistent Hashing

### Conceptual Model: The Hash Ring

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

### Virtual Nodes

**Problem:** With few servers, distribution can be unbalanced

**Solution:** Each physical server is represented by multiple points on the ring

**Example:**

- Instead of just "Server A," we have "Server A-1," "Server A-2," ... "Server A-100"
- Each gets placed at different positions on the ring
- This spreads the load more evenly

### Real-World Analogy

Think of consistent hashing like postal delivery zones. Each server is responsible for a "zone" on the ring. When a new post office opens, it only takes over a portion of one existing zone, rather than changing all zone boundaries.

## Rendezvous Hashing (Highest Random Weight)

### Conceptual Model: Scoring Contest

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

### Real-World Analogy

Think of rendezvous hashing like a specialized job assignment. Each data item is a job with unique requirements, and each server has different skills for different jobs. The server that's the best match for a particular job gets assigned that job.

## Practical Applications

### Consistent Hashing Applications:

- **CDN:** Akamai uses consistent hashing to determine which edge server should cache specific content
- **Distributed Databases:** Cassandra and DynamoDB use consistent hashing for data partitioning
- **Caching Systems:** Memcached clients use consistent hashing to distribute cache entries

### Rendezvous Hashing Applications:

- **Content-addressable Storage:** Systems like Ceph use rendezvous hashing for object placement
- **Load Balancers:** Some advanced load balancers use rendezvous hashing for request distribution
- **Peer-to-peer Networks:** Used to determine which peer should store particular content

## When to Choose Which

- **Choose Consistent Hashing when:**

  - You need a well-established algorithm with extensive literature and tooling
  - The ring structure provides useful properties for your application
  - Memory overhead isn't a significant concern

- **Choose Rendezvous Hashing when:**
  - You want a simpler implementation
  - Memory efficiency is important
  - You need natural load balancing without virtual nodes
