# 17 - Replication and Sharding

## Replication

### Overview

Replication is the process of storing copies of the same data on multiple machines. This provides:

- **Redundancy**: Protection against hardware failure
- **Improved performance**: Lower latency by serving data from geographically closer nodes
- **Increased availability**: System remains operational even if some nodes fail

### Leader-Follower Replication

- **Leader (Primary)**: Handles all write operations
- **Followers (Replicas)**: Receive copies of data changes from the leader
- **Write path**: Client → Leader → Followers
- **Read path**: Either from leader or any follower

### Multi-Leader Replication

- Multiple nodes can accept writes
- Useful for multi-datacenter deployments
- Introduces complexity in conflict resolution

### Leaderless Replication

- Any replica can accept writes
- Client sends write to multiple replicas
- Read requests sent to multiple replicas (quorum reads)
- Examples: Amazon Dynamo, Cassandra

## Synchronous vs Asynchronous Replication

### Synchronous Replication

- Leader waits for confirmation from followers before acknowledging write
- **Pros**: Strong consistency guarantees
- **Cons**: Higher latency, system availability depends on follower availability

### Asynchronous Replication

- Leader doesn't wait for follower confirmation
- **Pros**: Lower latency, higher availability
- **Cons**: Possible data loss if leader fails before replication completes

### Semi-Synchronous Replication

- Leader waits for confirmation from at least one follower
- Balance between consistency and availability

## Sharding

### Overview

Sharding (also called partitioning) splits a large dataset across multiple machines to distribute load and improve scalability.

### Sharding Strategies

#### Range-Based Sharding

- Divides data into continuous ranges based on a key
- **Pros**: Simple to implement, efficient range queries
- **Cons**: Risk of hot spots if data is not evenly distributed

#### Hash-Based Sharding

- Uses a hash function to determine data placement
- **Pros**: Even data distribution
- **Cons**: Range queries become inefficient, requires hash calculation

#### Directory-Based Sharding

- Maintains a lookup service that tracks data location
- **Pros**: Flexible, can dynamically change shard allocation
- **Cons**: Lookup service can become a bottleneck

### Rebalancing Strategies

- **Hash ring**: Consistent hashing minimizes data movement when nodes are added/removed
- **Fixed number of partitions**: Create more partitions than nodes, move entire partitions during rebalancing
- **Dynamic partitioning**: Split busy partitions, merge quiet ones

## Combining Replication and Sharding

- Each shard is typically replicated for fault tolerance
- Creates a two-dimensional distribution: horizontal (sharding) and vertical (replication)
- Improves both scalability and availability

## Common Challenges

### Consistency Issues

- CAP theorem: Can only guarantee two of Consistency, Availability, and Partition tolerance
- Read-after-write consistency
- Monotonic reads
- Eventual consistency

### Rebalancing Overhead

- Data movement consumes network bandwidth
- May impact performance during rebalancing

### Hot Spots

- Uneven data or access patterns
- Solutions: better sharding keys, caching, application-level workarounds

## Popular Implementations

### Database Systems

- MongoDB: Auto-sharding, replica sets
- Cassandra: Consistent hashing, tunable consistency
- MySQL Cluster: Auto-sharding with shared-nothing architecture

### Caching Systems

- Redis Cluster: Hash slots for sharding
- Memcached: Client-side sharding

### Storage Systems

- HDFS: Data replication without sharding
- Ceph: Dynamic subtree partitioning with replication
