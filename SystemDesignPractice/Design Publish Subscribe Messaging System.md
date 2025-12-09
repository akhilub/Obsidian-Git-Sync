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