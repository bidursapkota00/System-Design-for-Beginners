# 16 - NoSQL

NoSQL ("Not Only SQL") databases are non-relational database systems designed to handle various data models, provide horizontal scalability, and deliver high performance for specific use cases.

### Key Characteristics

- **Schema Flexibility**: Dynamic or no schema requirements
- **Horizontal Scalability**: Ability to scale across multiple servers
- **High Availability**: Designed for distributed environments
- **Eventual Consistency**: Often prioritize availability over consistency
- **Specialized Workloads**: Optimized for specific data patterns

### NoSQL Data Models

#### 1. Key-Value Stores

The simplest NoSQL model, storing data as key-value pairs:

- **Structure**: Each item is stored as an attribute name (key) with its value
- **Performance**: Extremely fast lookups by key
- **Use Cases**: Caching, session storage, user preferences

#### 2. Document Stores

Store semi-structured data in document format (typically JSON or BSON):

- **Structure**: Collections of documents, each with unique structure
- **Flexibility**: Schema-free with nested data structures
- **Querying**: Rich query capabilities on document contents
- **Use Cases**: Content management, user profiles, real-time analytics

#### 3. Column-Family Stores

Store data in column families - groups of related data:

- **Structure**: Rows with dynamic columns grouped into families
- **Performance**: Optimized for queries over large datasets
- **Sparsity**: Efficiently handles sparse data
- **Use Cases**: Time-series data, logging systems, heavy write workloads

#### 4. Graph Databases

Specialized for highly connected data:

- **Structure**: Nodes (entities), edges (relationships), and properties
- **Traversal**: Optimized for relationship queries and traversals
- **Use Cases**: Social networks, recommendation engines, fraud detection

## Popular NoSQL Database Examples

### Key-Value Stores

- **Redis**: In-memory store with data structures (strings, lists, sets)
- **Amazon DynamoDB**: Fully managed, multi-region, multi-active database
- **etcd**: Distributed key-value store for configuration data

### Document Stores

- **MongoDB**: General-purpose document database
- **Couchbase**: Distributed multi-model database
- **Amazon DocumentDB**: MongoDB-compatible document database
- **Firestore**: Google's document database with real-time capabilities

### Column-Family Stores

- **Apache Cassandra**: Distributed wide-column store
- **HBase**: Hadoop's distributed column-family database
- **ScyllaDB**: High-performance Cassandra-compatible database

### Graph Databases

- **Neo4j**: Native graph database with Cypher query language
- **Amazon Neptune**: Fully managed graph database service
- **JanusGraph**: Distributed graph database with multi-model support
- **TigerGraph**: Scalable graph database for deep link analytics

## Graph Databases in Depth

Graph databases excel at managing highly connected data by focusing on the relationships between entities.

### Core Components

- **Nodes**: Represent entities (people, products, accounts)
- **Edges**: Represent relationships between nodes
- **Properties**: Attributes on both nodes and edges

### Advantages of Graph Databases

- **Relationship Performance**: Traversing connections is efficient
- **Schema Flexibility**: Easily adapt to changing business requirements
- **Intuitive Modeling**: Natural representation of real-world relationships
- **Query Simplicity**: Complex relationship queries are concise

### Common Use Cases

- **Social Networks**: Friend connections, influence mapping
- **Recommendation Engines**: Product, content, and friend suggestions
- **Knowledge Graphs**: Connecting concepts and information
- **Fraud Detection**: Identifying suspicious relationship patterns
- **Network/IT Operations**: Infrastructure dependency mapping

## BASE Properties

BASE is an alternative to ACID for distributed systems, emphasizing availability over consistency:

- **Basically Available**: System guarantees availability
- **Soft state**: State may change over time, even without input
- **Eventually consistent**: System will become consistent over time

### ACID vs. BASE Comparison

| ACID                            | BASE                            |
| ------------------------------- | ------------------------------- |
| Strong consistency              | Eventual consistency            |
| Isolation                       | Availability first              |
| Focus on reliability            | Focus on scalability            |
| Pessimistic approach            | Optimistic approach             |
| Difficulty scaling horizontally | Designed for horizontal scaling |

## Scaling Strategies for Databases

### Vertical Scaling (Scale Up)

- Add more resources (CPU, RAM, storage) to existing servers
- **Pros**: Simpler to implement, no distribution complexity
- **Cons**: Hardware limits, single point of failure, cost

### Horizontal Scaling (Scale Out)

- Add more servers to distribute the load
- **Pros**: Virtually unlimited scaling potential, fault tolerance
- **Cons**: Increased complexity, data distribution challenges

### Read Replicas

- Create copies of data optimized for read operations
- **Pros**: Scales read operations, improved read performance
- **Cons**: Replication lag, eventual consistency

### Sharding

Sharding divides data across multiple servers, with each server responsible for a subset of the data.

#### Sharding Strategies

1. **Range-Based Sharding**

   - Divides data based on ranges of a key
   - **Example**: Users A-M on shard 1, N-Z on shard 2
   - **Pros**: Simple to implement, efficient range queries
   - **Cons**: Potential for hot spots, uneven distribution

2. **Hash-Based Sharding**

   - Uses a hash function to distribute data evenly
   - **Example**: hash(user_id) % num_shards determines placement
   - **Pros**: Even distribution, prevents hot spots
   - **Cons**: Difficult to perform range queries, resharding complexity

3. **Directory-Based Sharding**

   - Uses a lookup service to track data location
   - **Example**: Shard manager service that maps keys to shards
   - **Pros**: Flexible, allows for dynamic resharding
   - **Cons**: Lookup service becomes single point of failure

4. **Geographically-Based Sharding**
   - Locates data near its users
   - **Example**: European users on EU servers, US users on US servers
   - **Pros**: Reduced latency for users, regulatory compliance
   - **Cons**: Cross-region operations can be slow

#### Sharding Challenges

- **Joins Across Shards**: Complex and performance-intensive
- **Distributed Transactions**: Difficult to maintain ACID across shards
- **Resharding**: Redistributing data as system grows
- **Hotspots**: Uneven load distribution
- **Entity Groups**: Related data should reside on the same shard

## NoSQL vs. SQL: When to Choose Each

### Choose NoSQL When:

- Handling semi-structured or unstructured data
- Requiring horizontal scalability for large datasets
- Needing schema flexibility for evolving data models
- Prioritizing availability over strong consistency
- Building applications with specific data access patterns (graph, time-series, etc.)

### Choose SQL When:

- Working with structured data and fixed schemas
- Requiring complex joins and transactions
- Needing strong consistency guarantees (ACID)
- Working with moderate data volumes
- Requiring standardized query language (SQL)
