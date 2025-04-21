# 11 - Caching

## Introduction to Caching

Caching is a technique that stores copies of frequently accessed data in a high-speed storage layer (the cache), allowing future requests for that data to be served faster. Caching plays a crucial role in improving application performance, reducing latency, decreasing network traffic, and minimizing load on backend servers.

## Why Use Caching?

- **Improved Performance**: Reduced data retrieval time
- **Enhanced Throughput**: Handle more requests with the same infrastructure
- **Reduced Bandwidth Usage**: Less data transferred over networks
- **Decreased Server Load**: Fewer requests to origin servers
- **Higher Availability**: Services remain available even when backend experiences issues
- **Cost Savings**: Lower compute and network resource requirements

## Key Caching Metrics

### Throughput

Throughput in caching refers to the number of requests a cache can process per unit of time (typically measured in requests per second).

- **Factors affecting throughput**:

  - Cache hit ratio
  - Cache access latency
  - Network bandwidth and latency
  - Backend service performance
  - Cache size and eviction policy
  - Hardware resources (CPU, memory, network)

- **Optimizing for throughput**:

  - Increase cache memory allocation
  - Use efficient data structures
  - Implement appropriate eviction policies
  - Distribute cache load across multiple nodes
  - Minimize network round trips
  - Pre-warm cache with frequently accessed data

- **Measuring throughput**:
  ```
  Throughput = (Total Requests × Hit Ratio) / Time Period
  ```
  - Higher hit ratio = higher effective throughput
  - Monitor throughput during peak loads to identify bottlenecks

### Cache Hit Ratio

The percentage of requests that are served from the cache:

```
Hit Ratio = (Cache Hits / Total Requests) × 100%
```

- **Good hit ratios** typically range from 80-95% depending on the application
- **Low hit ratio** may indicate:
  - Cache size too small
  - TTL values too short
  - Suboptimal eviction policy
  - Cacheable content not being cached
  - Highly random access patterns

## Types of Caches

### Client-Side Caching

- **Browser Cache**: Stores web assets (HTML, CSS, JS, images) locally in the user's browser
- **Application Cache**: Data stored within client-side applications (mobile apps, SPAs)
- **Service Workers**: Enable offline functionality for web applications

### Server-Side Caching

- **Application/In-Memory Cache**: Data stored in the application's memory space (e.g., using Redis, Memcached)
- **Page Cache**: Fully rendered HTML pages cached on the server
- **Fragment Cache**: Portions of pages or components cached separately
- **Database Query Cache**: Results of database queries stored for reuse
- **Opcode Cache**: Compiled code cached to avoid repetitive processing (e.g., PHP opcache)

### Network Caching

- **Reverse Proxy Cache**: Caches content close to the server (e.g., Nginx, Varnish)
- **CDN Cache**: Distributes cached content geographically closer to users
- **Gateway Cache**: Caches content at network boundaries

## Caching Strategies

### Cache-Aside (Lazy Loading)

1. Application checks the cache for data
2. If found (cache hit), data is returned
3. If not found (cache miss), data is fetched from the database
4. Application stores fetched data in cache for future use

```
function getData(key) {
    data = cache.get(key)
    if (data == null) {
        data = database.get(key)
        cache.put(key, data)
    }
    return data
}
```

**Pros:**

- Simple to implement
- Only caches what's actually requested
- Resilient to cache failures

**Cons:**

- Cache misses incur additional latency (database query + cache update)
- Potential for stale data
- Cold cache problem after cache restart

### Write-Through

1. Application updates the database
2. Immediately after, application updates the cache

```
function saveData(key, value) {
    database.put(key, value)
    cache.put(key, value)
    return success
}
```

**Pros:**

- Data consistency between cache and database
- No cache miss penalty for newly written data

**Cons:**

- Write operations have higher latency (must update both DB and cache)
- Cache may contain unread data, wasting resources
- Additional write operation even if data isn't read later

### Write-Behind (Write-Back)

1. Application updates the cache
2. Cache asynchronously updates the database

```
function saveData(key, value) {
    cache.put(key, value)
    cacheQueue.queueForPersistence(key, value)
    return success
}

// Separate process
function persistCacheUpdates() {
    while (item = cacheQueue.dequeue()) {
        database.put(item.key, item.value)
    }
}
```

**Pros:**

- Reduced write latency
- Ability to batch multiple writes together
- Buffer against write-heavy workloads
- Improved throughput for write operations

**Cons:**

- Risk of data loss if cache fails before writing to database
- More complex implementation
- Potential data consistency issues

### Write-Around

1. Application writes directly to the database, bypassing cache
2. Cache is only populated when data is read

```
function saveData(key, value) {
    database.put(key, value)
    return success
}

function getData(key) {
    data = cache.get(key)
    if (data == null) {
        data = database.get(key)
        cache.put(key, data)
    }
    return data
}
```

**Pros:**

- Prevents cache churn from write-heavy operations
- Good for data that won't be read immediately or frequently

**Cons:**

- Cache misses for recently written data
- Higher read latency after writes

### Refresh-Ahead

Proactively refreshes frequently accessed items before they expire

```
function setUpRefreshAhead(key, refreshThreshold) {
    item = cache.get(key)
    if (item.accessCount > threshold && item.expiryTime - now() < refreshThreshold) {
        asyncRefresh(key)
    }
}

function asyncRefresh(key) {
    data = database.get(key)
    cache.put(key, data)
}
```

**Pros:**

- Reduces latency for frequently accessed items
- Smooths out database load spikes
- Maintains high throughput during peak times

**Cons:**

- More complex to implement
- May waste resources refreshing items that won't be accessed again
- Difficult to predict which items to refresh

## Cache Eviction Policies

When a cache reaches its capacity limit, eviction policies determine which items to remove to make space for new data.

### Least Recently Used (LRU)

Removes the items that haven't been accessed for the longest time.

### Least Frequently Used (LFU)

Removes items that are accessed least often. Tracks access frequency (hit count) for each item.

### Other Notable Eviction Policies

- **First In, First Out (FIFO)**: Removes the oldest entries first
- **Random Replacement**: Randomly selects items for eviction
- **Most Recently Used (MRU)**: Removes the most recently used items (useful for certain scanning patterns)

## Redis as a Caching Solution

[Redis](https://redis.io/) (Remote Dictionary Server) is an open-source, in-memory data structure store that can be used as a database, cache, message broker, and streaming engine.

### Key Redis Features

- **In-memory storage**: Extremely fast operations
- **Rich data structures**: Strings, Lists, Sets, Sorted Sets, Hashes, Streams, HyperLogLogs, Bitmaps
- **Persistence options**: RDB snapshots and AOF logs
- **Replication**: Master-slave architecture
- **High availability**: Redis Sentinel and Redis Cluster
- **Lua scripting**: Custom atomic operations
- **Pub/Sub messaging**: Real-time communication
- **Transactions**: Atomic operation sequences
- **Geospatial indexes**: Location-based features
- **Time series data support**: Efficient time-series storage

Redis provides several eviction policies configured via the `maxmemory-policy` setting:

## Throughput Optimization in Caching Systems

Throughput is a critical metric measuring a cache's request processing capacity. Several factors influence and can be optimized to improve throughput.

### Hardware Considerations

- **Memory bandwidth**: Higher bandwidth = faster data retrieval
- **CPU speed and cores**: More processing power for complex cache operations
- **Network interface**: Higher bandwidth for distributed caches
- **Storage I/O**: For cache persistence operations

### Software Optimizations

- **Serialization format**: Choose efficient formats (Protocol Buffers, MessagePack)

### Caching Architecture for Throughput

- **Read replicas**: Scale read capacity horizontally
- **Sharding**: Distribute load across multiple cache instances
- **Local caching**: Reduce network round-trips with process-local caches

### Measuring and Monitoring Throughput

- **Requests per second (RPS)**: Primary throughput metric
- **Response time percentiles**: P50, P95, P99 latency
- **Cache hit/miss ratio**: Impact on effective throughput
- **Network bandwidth utilization**: Potential bottleneck
- **CPU and memory utilization**: Resource constraints

## Cache Consistency Patterns

### TTL-Based Invalidation

- Set reasonable expiration times based on data volatility
- Simple but can lead to stale data within the TTL window

### Explicit Invalidation

- Services explicitly invalidate cache entries when underlying data changes
- More complex but provides better consistency

```
function updateData(key, value) {
    database.put(key, value)
    cache.invalidate(key)  // OR cache.put(key, value)
}
```

### Event-Based Invalidation

- Use publish-subscribe mechanisms to notify cache of data changes
- Good for distributed systems where multiple services might update data

```
function updateData(key, value) {
    database.put(key, value)
    messageBus.publish("data-change", {key: key})
}

// Cache subscriber
messageBus.subscribe("data-change", function(message) {
    cache.invalidate(message.key)
})
```

### Version Stamping / ETag

- Associate a version identifier with cached data
- Check if version matches before using cached data

## HTTP Caching

### Cache-Control Headers

- **max-age**: Seconds the response remains fresh
- **s-maxage**: Like max-age but for shared caches
- **no-cache**: Must revalidate with server before using cached version
- **no-store**: Don't cache at all
- **public**: Response can be stored by any cache
- **private**: Response is for a single user only

```
Cache-Control: max-age=3600, public
```

## Caching Challenges

### Cache Invalidation

- Determining when and how to invalidate cached items
- Ensuring all relevant caches are invalidated
- Balancing freshness vs. performance

```
"There are only two hard things in Computer Science: cache invalidation and naming things."
- Phil Karlton
```

### Cache Stampede/Thundering Herd

When many requests simultaneously try to refresh an expired cache item

**Solutions:**

- **Lock/Mutex**: Only one request regenerates the cache
- **Stale-While-Revalidate**: Serve stale content while refreshing asynchronously
- **Probabilistic Early Expiration**: Randomly refresh before expiration
- **Sliding Expiration Window**: Reset TTL on access
