---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
Design a Distributed Cache System

Design a service that provides distributed caching capabilities. ^SyIqOe9m

A distributed cache is an in-memory key-value service (think Redis or Memcached) that sits in front of your databases 
and APIs to serve data with microseconds-to-milliseconds latency. Users read and write simple records (often JSON or binary blobs) 
by key, with optional time-to-live (TTL), and the cache offloads the backend while keeping latency predictable.
 ^rPX8ZFd6

1. Requirements

    • Functional Requirements (Features of the System)

        - Store and retrieve data : System should Support Put(key, value) and Get(key) operations for key-value pairs
        - Delete data : System should Support Delete(key) to remove items
        - Time-To-Live (TTL) : Allow setting an expiration time for cached items
        - Eviction Policy : Automatically evict less relevant items when the cache is full (e.g LRU, LFU, TTL).
        - Relication : Replicate data across nodes for fault tolerance.
        - Consistency : Maintain data consistency across node, ideally eventual consistency between the cache and the source
          of truth, but strong consistency within a shard
        - Node Management : Dynamically add and remove cache nodes.

    • Non-Functional Requirements: (Qualities of the system) 

        - Performance : Achieve low latency for read and write operations (e.g., < 10ms for p99 cache hits)
        - Scalability : Horizontally scale to handle millions of queries per second and terabytes of data
        - High Availability : Tolerate hardware and network failures, ensuring the cache remains accessible without a single point of failure
        - Reliability/Fault Tolerance : Data should not be lost during node failures. ^pUmznFOv

2 . Core Entities:
    
    - Key : A unique identifier for cached data
    - Value : The actual data stored in cached (can be a serializable object like JSON, binary blobs)
    - Cache Node : An individual server or instance storing a portion of the cached data.
    - Client Application : The application or service interacting with the cache
    - Data Source : The persistent backend database or data store. 
 ^Yd4Yg2kj

4. Data Flows

1. Read Request (cache Hit):
        
        Client --> Cache Client --> Distributed Cache --> Cache Node (serves data)
2. Read Request (Cache Miss):
        
        Client --> Cache Client --> Distributed Cache --> Cache Node(determines miss) -->Data Source --> Cache Node(stores data) --> Client
3. Write Request :
        
        Client --> Cache Client --> Distribute Cache --> Cache Node(stores/updates data)
4. Eviction:
        
        Cache Node (periodically) --> Eviction Manager (removes stale/latest used data).
5 Replication: 
        
        Primary Cache Node --> Replica Cache Nodes (asynchronously or synchronously for data redundancy).

6 Invalidation:
        
        Data Source (on Update) --> Invalidation Service --> Distributed Cache (marks data stale/deletes). ^RAgZoMFC

5. High Level Design 

The distributed cache system will consists of

- Clients : Applications integrate with a client library or directly use the API.
             The client library can incorporate consistent hashing logic to route requests directly to the correct cache node, minimizing network hops

- Load Balance : Distributes incoming client requests across available cache nodes.

- Cache Cluster : A group of interconnected cache nodes.

- Cache Nodes : Individual servers responsible for storing a subset of the data.They manage local cache, apply eviction policies and handle replication.

- Distrubuted Hash Table (DHT) : For efficient data distribution , a consitent hashing algorithm maps keys to specific cache nodes, allowing
  horizontal scale and minimizing data reshuffling on node additions/removal.Virtual nodes enhance load distribution.

- Data Source : The backend persistent storage, such as database

- Configuration/ Cordination Service (e.g ZooKeeper/ Consul) : Manages cluster membership, node discovery and stores metadata about the cache topology and shard assignments.
         ^nm0wAu6s

3. API 

- PUT /v1/cache{namespace}/{key}?ttlSeconds={seconds} : Stores or updates a key-value pair with an optional TTL
- GET/v1/cache/{namespace}/{key}: Retrieve the value associated with a key
- DELETE/v1/cache/{namespace}/{key}: Removes a key-value pair. ^TlKf4wah

6. Deep Dives

• Data Partitioning (Sharding) 

- Utilize Consistent Hashing to distribute data across cache nodes. This minimizes data rebalancing when nodes are added or removed, ensuring even distribution
and scalability.Each node manages a specific range of the hash ring.

- Virtual Nodes can be used to improve the eveness of data distribution and reduce the impact of individual node failures.

• Consistency Model & Invalidation

- Eventual consistency is often acceptable between the cache and the primary data source, prioritizing availability and performance.
- Write Invalidate Strategy: After a successful write to the database, an invalidation message is published to the cache, marking the corresponding entry as stale
or deleting it.
- Write-Through/ Write-Behind : Depending on the use case, write through (updates cache and DB simultaneously) or write-behind (updates cache the asynchronously
updates DB) can be employed, offering different trade-offs in write latency and data consistency.
- TTL : Each key can have an associated TTL after which its automatically evicted, ensuring data freshness.

• Eviction Policies:
   
- Implement LRU (Least Recently Used) as a primary eviction policy.This works well for data with strong temporal locality, using a combination of a hash map and
a doubly-linked list for O(1) operations.
- Consider LFU (Least Frequently Used) or Time based Eviction(TTL) for specific use cases or as secondary policies. LFU is good for hot/cold data, while TTL is
for data with a natural expiration.

• Fault Tolerance & High Availability:

- Replication: Each data shard will have a primary node and one or more replica nodes.Data written to the primary is asychronously or synchronsouly replicated to its
replicas.
- Leader Election: For each shard, a leader election mechanism (e.g Raft or Paxos like) will promote a replica to primary if the current primary fails.
- Monitoring: Implement a monitoring service to track node health, cache hit/miss rates, latency and resource utilization.
- Quorum Reads/Writes(Optional) : For stronger consistency a quorum could be required for reads or writes, ensuring a minimum number of replicas confirm the operations
 ^MvY0AnHB

- Optional batch operations: POST/v1/cache/mget and POST/v1/cache/mset.
- Optinal invalidation via Pub/Sub for explicit cache invalidation. ^98X9S0gh

Design a Distributed Cache System ^KyNbOVIr

A distributed cache system needs to handle massive workloads across global locations with low latency, high availability and fault tolerance ^DMV1hK3o

1. Scope

• Functional Requirements
    - Read/Write/Delete key-value pairs
    - Support Time-To-Live (TTL) for cache entries
    - Implement an eviction policy, such as LRU (Least Recently Used)[I'll go with this] or LFU (Least Frequently Used), to manage cache capacity.
    - Support data replication for fault tolerance and high availability
    - Allow for cache invalidation (per-key or by tag)
    - Serve data with low latency from the nearest available node/region.

• Non-Functional Requirements
    - Performance : Sub-10ms latency for read and write operations, high throughput (e.g 100k req/sec)
    - Scalability : Horizontally scalable to handle increasing data volume (e.g 1 TB) and request load by adding more nodes
    - Availability : High available, resilient to node failures and network partitions (e.g 99 % uptime)
    - Consistency : Eventual consistency is acceptable for cached data balancing between strong consistency and performance
    - Global Distribution : Serve users from multiple geographic locations with minimal latency
------------------------
    - Durability : Data persistance across restarts (optional, depends on specific needs) ^iIpTyE8L

2. High-Level Design & Data Flow
    
    The system will consist of Clients, a Load Balancer, Cache Servers (Nodes), a Configuration Service, and a Persistent Data Store

    • Clients : Applications or services that interact with cache. They use a client library to communicate with the cache cluster.
    • Load Balancer : Distribute incoming requests from clients to the appropriate cache nodes.
    • Cache Servers (Nodes) : Individual servers storing a portion of the cached data in memory.They communicate with each other for
                                    replication and consistency.
    • Distributed Hashing (e.g Consistent Hashing) : A crucial component for distributing data evenly across nodes and minimizing data movements
                                                                during node additons/removals

    • Persistent Data Store : The backend database (e.g POSTGRESQL, Cassandra) that acts as the source of truth for data not found in the 
                                    cache (cache miss)
    • Configuration Service : Manages clusters, metadata, node discovery and consistent hashing ring information.

Data Flow (Cache Aside Strategy) :
    1. Client Request : A client requests data using a key
    2. Client Library : The client library (using consistent hashing) identifies the responsible cache node.
    3. Cache Hit : If data is found in cache it returned directly to the client.
    4. Cache Miss : If data is not found, the cache node fetches it from the Persistent Data Store.
    5. Cache Population : The fetched data is stored in the cache node, adhering to TTL and eviction policies, before being returned to the client.
    6. Writes : Writes can be directly to the Persistent Data Store, followed by invalidating or updating the corresponding cache entry
 ^2UdVTruj

3. Technology & Data Design
    
    • In-Memory Store : Redis over Memcached for global distribution because of its multi-region deployment capabilities 
    • Data Distribution : Consistent Hashing (e.g using a library or built-in functionality in Redis Cluster) to distribute keys across cache nodes
    • Client Library : Custom-built or leverage existing libraries to encapsulate consistent hashing logic, rety mechanism and failover
    • Persistent Data Store : A scalable, geographically distributed database like Cassandra (for high availability and eventual consistency)
    • Service Discovery : Zookeeper to manage cluster membership, node health and consistent hashing ring updates.
    • Load Balancing : NGINX or cloud-native LBs(e.g Google Cloud LB) to distribute client requests to cache nodes ^DOjeSSFf

4. Low Level Design

Cache Node (Redis Instance):
    
    • Key-Value Store : Store data in RAM
    • Eviction Policy Module : Implements LRU (default in Redis), LFU or TTL
        
        - LRU Implementation : Use a combination of hashmap for O(1) lookups and a doubly linked list to track usage order for O(1) updates

    • Replication Modules : For high availability, data on a primary node is replicated to one or more replica nodes
        
        - Master Slave Replication : Master handles write, slaves handle reads and replicate data
        - Sentinel/Cluster : Manages failover and cluster topology

    • APIs or System Interface
        
        - GET(key) : Retrieves value for `key`
        - SET (key, value, [TTL]): Store `value` for `key` with optional TTL
        - DELETE (key) : Remove `key`.


Client Library:
    
    • Hashing Logic : Implements consistent hashing to map `key` to a specific cache node. This involves maintaining a view of the cluster topology (obtained from the 
                         Configuration Service)
    
    • Connection Pooling : Manages connections to cache nodes.

    • Retry Mechanism : Handles transient network failures or node unavailability.  ^Oxzftsyq

• Cache Aside Logic

  function get(key):
    node = consistent_hash(key)
    value = node.GET(key)
    if value is null:                //Cache Miss
        value = persistent_store.GET(key)
        if value is not null:
            node.SET(key, value, TTL)
    return value


 function set(key, value, TTL):
    persistent_store.SET(key, value)
    node = consistent_hash(key)
    node.DELETE(key)                   //Invalidate cache for freshness
    // Or, node.SET(key, value, TTL ) for write-through  ^t1YC1wM5

Multi-Region Design
    
    - Each geographic region has its own cache cluster.
    - Global Load Balancer/DNS : Direct users to the nearest regional endpoint.
    - Cross-Region Replication: for high data consistency across regions, data changes in the persistent store can trigger invalidations or updates in remote caches
                                      This can be complex, often involves trade-off between strong consistency and low latency.Eventual consistency is usually accepted ^6CIEoYPU

Performance & Bottlenecks:
       
    • Network Latency : Minimized by placing cache nodes geographically close to users (CDNs, multi-region deployments) and
                            optimizing network routes
    
    • Hot Keys : A single key receiving disproportionately high traffic. Can be mitigated by :

            - Further sharding or replicating hot keys
            - Using a small, local cache on the application server for extremely hot data.

    • Thundering Herd : Many clients simultaneously miss a key and flood the backend database. Mitigated by:
            
            - Request Collapsing/Deduplication : Client library or cache node aggregates multiple requests for the same missing key into a single request
                                                        to the backend
            - Stale-While-Revalidate : Serve stale data while asynchronously fetching fresh data in the background.
                     ^fQDIexjY

5. Object Model Design or Core Entities

    • CacheEntry Object
    • Node (Cache Server) Object
      ^RNRHqScx

6. Address Non-Functional Requirements or Scalability & Optimization

    • Performance
        - In-memory storage : Redis provides sub-millisecond access.
        - Consistent Hashing : Efficiently routes requests to the correct node, avoiding bottlenecks
        - Multi-region deployment : Servers data from the nearest location, reducing network latency
        - Eviction Policies : LRU ensures frequently accessed data remain in cache
        - Request Collapsing: Prevents backend overload during cache misses.

    • Scalability :
        - Horizontal Scaling: Easily add more cache nodes to increase capacity and throughput. Consistent hashing automatically rebalances data
        - Stateless Cache Nodes : Simplifies scaling and recovery

    • Availability :
        - Replication : Data is replicated across multiple nodes (master-slave or peer-to-peer).If a node fails, its replicas can serve requests.
        - Failover Mechanism : Sentinel or Redis Cluster automatically detects failures and promotes replicas
        - Multi-region deployment: Regional outages wont affect global availability.

    • Consistency :
        - Eventual Consistency : Often acceptable for caches. Updates to the DB(source of truth) asynchronously propogates to invalidate/update
                                    cache entries.

    • Cache Aside : Provides eventual consistency. For stronger consistency, a Write-Through strategy (writing to cache and backend simultaneously) could
                       be used, at the cost of higher write latency.

     ^q9oE8zNP

Evolution of Design (Additional Features)

- Cache Warming/Pre-fetching : Load frequently accessed data into the cache proactively during off-peak hours or based on predictive 
                                      analytics to reduce initial latency
- Advanced Eviction Policies : Implement LFU or custom policies based on data access patterns and importance
- Tiered Caching 
- Compression
- Monitoring & Alerting : Integrate with monitoring system (e.g Prometheus, Grafana) to track cache hit/miss ratios, latency, memory usage
                              and node health. Alerts for critical metrics. ^3G0tBTxL

System Assessment (Security, Monitoring & Testing)

    • Security 
        - Network Encryption
        - Access Control
        - Data Encryption at Rest
        - Secure Configuration

    • Monitoring:
        - Metrics : Track cache hit rate, miss rate, latency (read/write) memory per node, network I/O,
                      CPU utilization and error rate,
 
        - Dashboards : Visualize key metrics using tools like Grafana
        - Alerting: Set up alerts for anomalies (e.g sudden drop in hit rate, high latency, node failure).
    
    • Testing :
        - Unit/Integration Test
        - Performance Testing
        - Fault Tolerance Testing
        - Consistency Testing
        - Security Audits
         ^tbdaq9Hv

BASIC SOLUTION ^oWfeCiIV

ADVANCED SOLUTION ^Df3OSBEw

----------------Out of Scope---------------
        Durability (Optional): Provide configurable data persistence through snapshots (e.g AOF/ RDB)
        Cost Efficiency : Optimize memory usuage through compression or off-heap storage  ^nptARBX7

%%
## Drawing
```compressed-json
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=
```
%%