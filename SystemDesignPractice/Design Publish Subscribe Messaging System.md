---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
1. Define Scope 

Functional Requirements
    • Publishers can send messages to a specific topic
    • Subscribers can register interest in one or more topics
    • Subscribers receive all messages published to their subscribed topics
    • Messages should be delivered at least once
    —----------------------
    • Support for multiple topics
    • Client APIs for publishing and subscribing
    
Non-Functional Requirements
    • High Availability:  The system should remain operational even with component failures
    • Scalability : (1 Million mps) Able to handle a large number of publishers, subscribers and messages per second 
    • Low Latency : Messages should be delivered within minimal delay (<200ms)
    • Durability: Message should not be lost
    • At-least-once delivery guarantee
    —-----------------------
    • Ordered Delivery : (Optional per topic) : Message within a topic are delivered to subscribers in the order they were published
 ^qIzYPreC

Design Publish/Subscribe Messaging System ^dMfm4gDI

2. High Level Design, System Interface & Data Flow

Core Components:

    Publishers : Clients sending messages
    Subscribers : Clients receiving messages
    Message Brokers : The central component responsible for receiving, storing and delivering messages
    Topic Management Service : Handles topic creation, deletion and subscription management
    
Data Flow:

    Publisher → Message Broker
    1. A Publisher sends a message to the Message Broker for a specific Topic via an API endpoint
    
    2. The Message Brokers receives the message, acknowledge it, and stores it reliably
    
    3. The Message Brokers then pushes the message to all active Subscribers registered for that topic
    
    4. Subscribers consumes messages and acknowledge the receipt
    
System Interface (API Endpoints)
    
    • Publisher API 
    - Publishes message to a topic
    POST/topics/{topicName}/message (payload : {“messageContent”: …}) - 
    
    • Subscriber API 
    - Subscribe to a topic
    POST/subscribers/{subscriberId}/subscripions 
    (payload : {“topicName”: …})
    
    • Unsubscribe from a topic
    - DELETE/susbcribers/{subscriberId}/subscriptions
    
    • Retrieve Message
    - GET /subscribers/{subscriberId}/messages (WebSocket for message reception)
    
    • Acknowledges a Message
    - POST/susbsribers/{subscriberId}/acknowledgements (payload : {"messageId" : "..."}
    
    • Topic Management API (Internal/Admin)
    - POST /topics (payload : {“topicName”: …}) - Creates a topic
    - DELETE / topics/{topicName} - Deletes a topic
 ^HeWX2WT1

3. Technology and Data Design

• Message Broker : 
    - Apache Kafka is an excellent choice due to its througput, scalability, fault tolerance and durability.
    Its distributed log architecture naturally supports ordering delivery within partitions
    
    - Reasoning : Kafka's design for high-volume, real-time data streams aligns perfectly with the requirements for a pub/sub system, providing built-in replication
    and partitioning
                
• Database for Topic/Subscription Management  : 
    - Apache Cassandra or another NOSQL DB (e.g Dynamo DB if cloud native) for storing topic metadata and subscriber-topic mappings
    
    - Reasoning : Provide high availability and scalability for managing subscription information, essential for the Message Broker to know where to deliever
      messages. Eventual Consistency is acceptable her

• Client Libraries: Provide SDKs for various languages (Java, Python, Node.js) to abstract API interactions 

• Load Balancers : (NGNIX or cloud native LBs ) For distributing publishers and subscribers requests across broker instances

• Service Discovery : Zookeeper (often bundled with Kafka) to manage broker instances and client connections
 ^q6NrWEVo

4. Low-Level Design

Message Brokers (Kafka Specifics)

    - Topics : Each logical topic in our system maps directly to a Kafka topic
    - Partitions : Each Kafka topic is divided into partitions for scalability and parallelism.Publishers writes to specific partitions (e.g round robin, key-based hashing
    for ordered delivery related messages
    - Producers (Publishers): Use Kafka Producer API to send message, implement idempotent producers to avoid duplicate message during retries.
    - Consumers (Subscribers): Use Kafka Consumer API. Each subscriber instance belongs to a consumer group. To achieve fan-out each subscriber application can have its 
    own consumer group, or a common consumer group for all instances of the same logical subscriber 
    - Consumer Offsets : Kafka automatically tracks offsets for each consumer group, ensuring messages are consumed correctly and at-least-once delievery is maintained through
    commit mechanisms.


Subscription Management Service

    - Data Model : Stores 
                            - subscriberId
                            - topicName
                            - offset
        in Cassandra. The offset tracks the last message delivers to that specific subscriber for that topic
            
    - API: 
        • When a subscriber registers, it creates an entry in Cassandra
        • When a message is published the Message Brokers consults this service to find all the relevant subscribers
        • Upon successful delivery to a subscriber, the service updates the offset for that subscriber-topic pair
        
    Performance & Bottlenecks : This service is critical for fan-out. Catching subscriber-topic mappings in memory on the Message Broker or using a distributed
    cache like Redis can reduce database load.
        
 ^99beej5B

5. Object Model Design

Message:
    - messageId (UUID)
    - topicName : “”
    - publisherId: “”
    - timestamp: 
    - content : (String/JSON, actual message payload)
    - headers : 

Subscription:
    - subscriberId (UUID)
    - topicName : String
    - createdAt : 
    - lastDeliveredOffset : (Long, for tracking for subscriber’s progress)

 ^uDFIqnoh

6. Addressing Non-Functional Requirements

High Availability
• Kafka : Replicated partitions across multiple brokers. If a broker fails, a replica takes over
• Cassandra : Data replicated across multiple nodes
• Load Balancers : Distribute traffic and handle failover for broker and subscription service instances

Scalability 
• Kafka : Horizontally scalable by adding more brokers and partitions. Producers and consumers can be scaled independently
• Cassandra : Horizontally scalable by adding more nodes
• Microservices: Individual Services (e.g subscription management) can be scaled independently

Low Latency: 
• Kafka-log based architecture : Efficient for sequential reads/writes
• Batching: Producers can batch messages to improve throughput at the cost of slight latency
• Direct push : Brokers pushes to subscribers, minimizing pull delays
• Trade-off : Very low latency might require sacrificing some batching efficiency or stronger consistency guarantees

Durability:
• Kafka : Messages are persisted to disk with configurable replication factors (e.g 3x replication)
• Cassandra : Persists subscriptions data with replication  
        
At-least once delivery:
• Kafka consumer offset management: If a subscriber fails before acknowledging, the message will be redelivered upon restart
• Publisher retries: Publishers retry sending if no acknowledgment, potentially leading to duplicates. Idempotent producers and deduplication on the 
consumer side are crucial to handle this.

—---------------------------
Ordered delivery(per-topic/partition): 
• Kafka partition ordering -  Messages within a single partition are always ordered. Publishers can route messages with the same key to the same partition
• Trade-off : Strict global ordering across all partitions is difficult to achieve without significantly impacting performance and scalability
 ^e815LZUO

7. Additional Features (Evolution of Design)

Message Filtering :
    - How : Allow subscribers to specify filters (e.g based on message content or headers) when subscribing. The Message Broker would apply these filters before delivering.
    - Integration : Filtering logic can be implemented as part of the Message Broker's delivery mechanism or offloaded to a separate filtering service
    - Trade-off : Adds complexity to the broker and potenital latency if filters are computationally intensive

Dead Letter Queue (DLQ):
    - How : If a subscriber repeatedly fails to process or acknowledge a message, move it to a DLQ for later inspection or reprocessing.
    - Integration : The Message Broker or a separate "dead letter manager" service monitors subscriber acknowledgements and routes unacknowledged messages after
    a certain number of retries

Message Prioritization:
    - How : Allow publishers to assign priorities to messages. The broker would prioritize delivery of high-priority messages
    - Integration: Could involve separate Kafka topics for different priority levels or custom logic within the broker to reorder messages in memory before delivery.
    - Trade-off : Can Introduce complexity and potentially starve lower-priority messages

Schema Registry:
    - How : Enforce message schema using a schema registry (e.g Confluent Schema Registry). Publishers must send messages conforming to the schema; subscribers
    can use the schema to deserialize
    - Integration : Integrate with Kafka and client libraries to validate messages during production and consumption 
 ^ckzpjQYc

System Assessment

Security :
    - Authentication & Authorization: Secure API endpoints for publishers and subscribers
    - Encryption : Encrypt messages in transit and at rest 

Monitoring:
    - Collect metrics on message rates, latency errors, consumer lag and system resource utilization (CPU, memory, disk I/O)
using tools like Prometheus and Grafana
    - Log all significant events and error for debugging and auditing using a centralized logging system like ELK stack or Splunk

Testing :
    - Unit Tests : For individual components and business logic
    - Integration Tests : To verify interactions between services (e.g publisher to broker, broker to subscriber)
    - Performance/Load Tests :Simulate high message volumes and subscriber count to identify bottlenecks and validate scalability
    - Chaos Engineering : Inject failure to test system resilience and availability

 ^btt1LCB2

How would you implement partitions and consumer groups?
 ^ROQljnR5

What consistency guarantees would you offer?
 ^gzShF53o

How would you persist and replicate data? ^zqREjdGQ

How would you handle backpressure and dead-letter queues? ^ULSiDwOR

Here's how partitions and consumer group would be implemented:

• Partitions : Each Kafka topic would be divided into multiple partitions (e.g hundreds or thousand).
    - Publishers distribute messages across these partitions, typically using a message key (e.g userId or orderId)
    for ordered delivery of related messages, or round-robin for even distribution.

    - This enables horizontal scaling, allowing parallel reads and writes.

• Consumer Groups: A consumer group consists of one or more consumer instance that cooperatively consume messages
from one or more topics.
    - Each partition within a topic is assigned only one consumer instance within a given consumer group. This ensures that messages within
    a partition are processed in order and only once by that group.
    - For fan-out different logical application would have their own unique consumer grp allowing each app'n to receive all messages from a 
subscibed topic independently
    - Kafka handles the offset management within each consumer group, tracking which messages have been consumed by each consumer
    instance, facilitating at-least-once delivery and recovery from failures ^11D7wuCy

Backpressure Handling:

Backpressure occurs when the rate of message production exceeds the rate at which consumers can process them, leading to growing
message backlogs

1. Consumer-Side Scaling
    - Horizontal Scaling : The primary strategy is to scale out consumer instances within a consumer group. Adding more consumers allow for
parallel processing of partitions, increasing overall consumption throughput

    - Optimize Consumer Logic : Improve the efficiency of the consumer application's processing logic to reduce the time spent on each message

    - Batch Processing : Consumers can process messages in batches, which can improve efficiency by reducing the overhead of individual message
       processing. However this is a trade-off with latency.
    - Flow Control : Kafka consumer clients can be configured with flow control parameters like max.poll.recors (maximum messages fetched per poll) and 
    max.partition.fetch.bytes to prevent consumers from pulling more messages than they can handle

    - Pause/Resume : Consumers can explicity pause() consuming from a partition when overwhelmed and resume() once they have caught up


2. Broker/Topic Management:
    
    - Increase Partitions : If few partitions are consistently hot, increasing the number of partitons can distribute the load more evenly, enabling more
    parallelism.
    - Broker Monitoring : Monitor Kafka broker health and resource utilization to ensure brokers themselves aren't bottlenecks


Dead-Letter Queues (DLQs):

A DLQ is a dedicated topic (or storage) for messages that cannot be successfully processed by consumers after multiple retries

1. DLQ Topic Creation : Create a separate Kafka topic e.g `my-topic-dlq`, to serve as the dead-letter queue.

2. Consumer Error Handling:
    
    - within the consumer application's logic, implement robust `try-catch` blocks to handle processing failures (e.g malformed data, external service unavailability,
business logic errors)

    - If am message fails processing, the consumer should retry it a configurable number of times (e.g with exponential backoff)

    - After exhausting all retries, the failed message, along with relevant metadata (error message, timestamp, original topic/partition/offset, stack, trace)
is sent to the DLQ topic

3. DLQ monitoring and alerting

    - Continously monitor the DLQ for new messages using dedicated consumers.
    - Set up alerts to notify operators when messages appear in the DLQ.
    - DLQ messages can be inspected to diagnose the root cause of failures (e.g data quality issues, code bugs, transient errors)

4. Reprocessing/Reconcilliation:
    
    - Based on the analysis, messages in the DLQ can be manually or automatically reprocessed after fixes are deployed or moved to an archieve.
    This could involve moving them back to the original topic or a specific reprocessing topic.


DLQs are essential for isolating problematic messages, preventing them from blocking the main processing pipeline and ensuring system integrity. 
 ^QPFkSsh6

%%
## Drawing
```compressed-json
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
```
%%