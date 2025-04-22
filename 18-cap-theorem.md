# 18 - CAP Theorem

## Overview

The CAP theorem, formulated by Eric Brewer in 2000, states that a distributed data system can only guarantee at most two out of the following three properties simultaneously:

- **C**onsistency
- **A**vailability
- **P**artition tolerance

## The Three Properties Explained

### Consistency

- All nodes see the same data at the same time
- Every read receives the most recent write or an error
- Ensures that data is identical across all nodes in the system
- Similar to the concept of linearizability or atomic consistency

### Availability

- Every request to a non-failing node receives a response
- The system remains operational and responsive even during failures
- No request can be left hanging without a response
- Does not guarantee that the response contains the most recent data

### Partition Tolerance

- The system continues to operate despite network partitions
- Network partitions occur when nodes cannot communicate with each other
- Messages between nodes may be delayed or lost
- Essential property for distributed systems that operate across networks

## CAP Theorem in Practice

### CA Systems (Consistency + Availability)

- Sacrifice partition tolerance
- Examples: Traditional RDBMSs (PostgreSQL, MySQL with single-node setup)
- Not truly distributed as they cannot handle network partitions
- Rarely achievable in real-world distributed environments

### CP Systems (Consistency + Partition Tolerance)

- Sacrifice availability during partitions
- Examples: MongoDB, HBase, Redis (in certain configurations)
- May become unavailable during network partitions to maintain consistency
- Choose consistency over availability when data accuracy is critical

### AP Systems (Availability + Partition Tolerance)

- Sacrifice strong consistency
- Examples: Cassandra, Amazon Dynamo, CouchDB
- Provide eventual consistency instead of immediate consistency
- Choose availability over consistency when system uptime is critical

### Consistency Models

- **Strong Consistency**: All reads reflect the latest write
- **Eventual Consistency**: All replicas eventually converge given enough time
- **Causal Consistency**: Operations causally related must be seen in the same order
- **Read-your-writes Consistency**: A client always sees its own writes

## Practical Considerations

### System Design Tradeoffs

- Business requirements should guide CAP choices
- Financial systems may prioritize consistency
- Social media may prioritize availability

### Handling Partition Scenarios

- Detect partitions quickly
- Define recovery procedures when partitions heal
- Consider using compensating transactions

### Regional Considerations

- Multi-region deployments face CAP challenges more acutely
- Geographic distance increases partition probability
- Consider region-specific consistency requirements
