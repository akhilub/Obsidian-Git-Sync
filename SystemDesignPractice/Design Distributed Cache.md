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
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

TA443AAlui/U2fQSR50BAA50CAAA
```
%%