---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
URL shortening service like bit.ly ^jyhQujN4

A URL shortening service transforms long,complex URL's into concise ,userfriendly links, reducing the length and complexity.It necessitates consideration
 for unique key generation,data persistance,high availability and redirect speed. ^MGZUVv2D

Requirements ^Q81684Nb

Non Functional Requirements ^o1BPHki6

Back of the Envelope Estimations ^D1t8FijJ

Functional Requirements ^0wiEtg3U

API Routes ^pG2xYfY5

DataBase Schema ^0ME7hiYp

High Level Design ^axodlEIN

Understand the Requirements ^xd6WaAjQ

Requirements ^HvHblMnQ

Back of the Envelope Estimations ^DQmA1X7Q

CAP Theorem ^Oem1u1oj

- Generate unique short URLs for each long URL submitted
- Redirect users from the short URL to the original long URL
- Ensure the uniqueness of the generated short URLs
- Support analytics on the number of link visits
- Allow custom, vanity, short urls provided by the user ^KiMSHMxT

- Handle a large number of URL shortening and redirection requests
- Scale to accommodate an increasing number of users and shortened URLs over time
- High availability
- Redirect times should be the same order of magnitude as navigating to the original url directly." ^AFRbMnsj

Storage Requirements ^TljJbjLT

Daily Active Users(DAU) ^xp2fQE2W

Given the dynamic nature of user behavior and market conditions, the DAU estimation might vary. 
However, here is a rough estimate:

1B (dau social media users)
* .01 (percent that post shortener links each day)
* .25 (our estimated market share)
= 2.5M DAU ^ZzC7k7Ex

Let's consider the database system storage overhead and the possibility of users creating more than one short link per day. 
The revised calculation is as follows:

dbRow = 100b (original url) + 8b (short url) + 500b (metadata & analytics) = about 1KB

2.5M (dau)
* .01 (100:1 read to write ratio)
* 2 (average links shortened /day /user, considering users might create more than one link per day)
* 1 KB (dbRow)
* 2 (redundancy)
* 2 (backup)
* 2 (growth)
* 365 (days per year)
* 10 (store data 10 years)
= about 2TB

Additionally, let's add a 20% overhead for the database system itself, bringing the total to approximately 2.4TB. ^uX4FAzvh

"For a URL shortening service, I would prioritize partition tolerance and availability over consistency -- making this an AP system. Users expect the service to be highly available and responsive. The nature of this system allows for eventual consistency, where data becomes consistent over time. This is a permissible trade-off to ensure high availability and partition tolerance, even if it means that the URL mappings might exhibit slight inconsistencies for a brief period." ^QnbYFIZf

Given the constraints of the CAP theorem, 
where would you place the emphasis for your 
system: consistency or availability? Justify your answer. ^68qi6KFv

"I would choose a NoSQL database, particularly a key-value store such as Redis or Amazon DynamoDB. The main reason for this choice is that the system largely deals with simple key-value mappings (shortened URL to original URL), and the speed of reading/writing is critical to providing a fast and responsive service. A key-value store is optimized for such use cases, offering high performance, low latency, and easy scalability. This makes it more suitable than a SQL DB, which is better suited for more complex data models and queries.

However, it's important to consider some trade-offs and limitations of using a key-value store. For instance, they typically do not support complex querying capabilities that relational databases offer. To mitigate this, we could use a hybrid approach, combining a key-value store for fast access and a relational database for complex querying if needed.

Data consistency can be a challenge with NoSQL databases, especially in a distributed environment. To handle this, we could use techniques such as eventual consistency, where updates are propagated to all nodes over time, ensuring that all copies of data are consistent eventually.

As for scalability, key-value stores like Redis or DynamoDB are designed to be highly scalable. We can add more nodes to the system to handle increased load. In the case of DynamoDB, it automatically partitions and re-partitions data to maintain consistent performance. For Redis, we can use partitioning strategies like range partitioning or hash partitioning to distribute data across multiple nodes." ^x0DpmYqq

What type of database would you use for the system and why? ^idTw5ft7

User
- id: string (unique identifier)
- name: string
- email: string (unique)
- role: string ^0mQFkK3E

ShortURL
- id: string (unique identifier)
- shortUrl: string (unique)
- originalUrl: string
- userId: string (references User.id)
- createdAt: datetime
- lastUsedAt: datetime
- isActive: boolean
- isDeleted: boolean
- clicks: integer
- count: integer ^c39jYyFv

ClickLog
- id: string (unique identifier)
- shortUrlId: string (references ShortURL.id)
- clickedAt: datetime
- ipAddress: string
- userAgent: string
- location: string
- referrer: string" ^zGPZmljC

For a URL shortening service, we need to ensure high data availability, efficient replication, and accurate synchronization. Given that we're using a NoSQL key-value store, I would handle these requirements as follows:

1. Data Availability: As URL shortening services need to be highly available and responsive, redundancy should be built into the system. We can achieve this by replicating the data across multiple nodes or regions. In addition, frequent backups are necessary to ensure data durability. If we're using a cloud-based NoSQL service like Amazon DynamoDB, Google Cloud Datastore, or Azure Cosmos DB, redundancy and backups are generally built into the service. These services also offer multi-region replication which can be utilized for higher availability and lower latency access.

2. Replication: Given the nature of key-value stores and their tendency for eventual consistency, an asynchronous replication strategy could be suitable for our system. This ensures lower latency which is a critical factor for a service like URL shortening. The trade-off is the possibility of temporary inconsistencies between replicas. However, as URL shortening is less sensitive to minor inconsistencies (since mappings do not change frequently once created), the benefits of faster response times are likely to outweigh the risks.

To handle potential inconsistencies, we could implement a conflict resolution strategy such as 'last write wins'. In this strategy, the most recent update to a record takes precedence over earlier updates. This could be implemented by timestamping each write operation and choosing the write with the latest timestamp in the event of a conflict. However, this strategy is not perfect as it assumes synchronized clocks and can lead to data loss if updates are not properly ordered.

Alternatively, we could use vector clocks to keep track of updates and resolve conflicts. Vector clocks are essentially a list of logical clocks, with each clock representing a different node. This allows us to keep track of the causal order of events, which can help in resolving conflicts. However, vector clocks can be complex to implement and manage, especially in a large distributed system.

3. Synchronization: To ensure synchronization across different nodes, we can take advantage of the eventual consistency model. For example, Amazon DynamoDB offers built-in support for eventual consistency. In the event of a network partition, we could use a strategy like hinted handoff, where failed write operations are temporarily stored at another node and replayed when the partition is resolved. We must monitor and manage the delay in synchronization to ensure that it remains within acceptable bounds. For this, we could use monitoring tools provided by the NoSQL service or use custom metrics.

In a multi-region setup, we could use active-active replication where each region serves both read and write requests. This would ensure low latency access as requests can be served by the geographically closest region. However, it would require careful management of data synchronization and conflict resolution across regions. For example, if a user in region A shortens a URL at the same time a user in region B shortens the same URL, a conflict could occur. In this case, we could use the 'last write wins' strategy or vector clocks to resolve the conflict.

All these strategies align with the CAP theorem, particularly focusing on high availability and partition tolerance, while accepting eventual consistency. ^ERunh7Et

How would you handle data availability, replication, and synchronization? ^39iJdl3q

Data Model ^fadRHcR0

Database Type ^Fia6Hi4r

DataBase Schema ^BLPe6z62

DataBase Replication ^MmpirDBJ

Core Components ^jbTaElNY

URL Creation ^wLuI4vnm

Colloisions ^USkA83hx

Redirection ^QkaAKvcv

Describe how your system generates a shorter 
and unique alias for a given URL ^Eg5lOPbn

When thinking about how to generate a shorter, unique alias for a URL, consider the properties that the alias must have. It should be unique to avoid collisions, shorter than the original URL for convenience, and likely consist of URL-friendly characters. There are various strategies and algorithms that can transform input data into a fixed, smaller size output. Additionally, think about how you might handle a scenario where the generated alias is already in use. Your strategy should balance the need for uniqueness and brevity. ^0yNzHgfk

To generate a shorter and unique alias for a given URL, we can use a combination of hashing and encoding techniques. First, we can create a hash of the input URL using a hashing algorithm like MD5 or SHA-1. Then, we can encode the hash using a URL-friendly character set, such as Base62 (which includes alphanumeric characters: A-Z, a-z, and 0-9). Finally, we can truncate the encoded hash to a desired length, like 6 or 8 characters, to create a short alias. ^lHxkGIqH

As we discussed earlier, this will give us 62^6 = 58 billion unique combinations, which is far more than enough unique combinations to satisfy our usage over the life of the system. ^r9mrvcRt

Another option would be to use a separate ID generation service to increment a unique ID for each new URL. The ID generation service may use a counter stored in a database that increments for each new URL. This approach could provide a more uniform distribution of URL lengths compared to the hashing approach. However, maintaining a globally unique and incrementing counter is challenging in a distributed system due to issues like synchronization and scalability. This is why I'm deciding to go with the hashing approach instead. ^Uo45thTs

However, it's important to note that there are trade-offs to this approach. The use of a hash function might lead to non-uniform distribution of URLs, which could result in some URLs being significantly shorter than others. This could potentially lead to a situation where we run out of unique URLs sooner than expected." ^VpJCm6qm

How does your system handle potential collisions 
in the case of the same short URL generated 
for different long URLs?
 ^TfW5vcBb

Collisions might occur if the same short URL is generated for different long URLs. To handle this, consider a collision resolution strategy like chaining or open addressing. Alternatively, upon a collision, you could generate a new hash, perhaps by adding a random element or a sequential number to the original URL before hashing it again. ^SMbwKqfy

To handle potential collisions, we can use a "Write if not exists" operation provided by most NoSQL key-value stores. This operation will attempt to store the newly generated shortened URL only if it does not already exist in the database. If a collision occurs the operation will fail. We can then catch the failure and regenerate a new hash by either using a different hashing function or by appending a counter or random value to the end of the original url and re-hashing. This process repeats until we've created a unique shortened URL. ^msfAvuqY

In terms of the hashing function, we can use a strong hashing algorithm like SHA-256. This algorithm is deterministic, meaning the same input will always produce the same output, but the output is distributed evenly across the possible output space, reducing the likelihood of collisions. ^tNNObR9o

In the case of race conditions, where two processes generate the same shortened URL at the same time, the 'Write if not exists' operation will also handle this. Only the first write operation will succeed and the second will fail, prompting the regeneration of a new hash. ^SBDUYamR

However, there are trade-offs to this approach. The process of regenerating hashes in the case of collisions can be computationally expensive, especially if collisions happen frequently. This could potentially slow down the system. To mitigate this, we could implement a limit on the number of retries for generating a unique hash. If this limit is reached, we can return an error to the user and ask them to try again later. ^O7O9n8xV

How will you redirect the user to the original URL 
when a shortened URL is accessed?
 ^UuPoM5nH

To redirect a user to the original URL, your service should map the short URL alias back to the original URL. This could involve a lookup in your database where the mapping is stored. Once the original URL is retrieved, an HTTP 301 (permanent) or 302 (temporary) redirect could be used to navigate the user to the original URL. Consider possible edge cases such as the alias not being found in the database or the database being temporarily unavailable. ^4LcseRIv

"To redirect users to the original URL when they access a shortened URL, the service should first look up the short URL alias in the database to find the corresponding original URL. Once the original URL is retrieved, we will respond with either an HTTP status of 301 (permanent) or 302 (temporary) redirect to send the user to the original URL. ^yQz9qEtl

I am going to decide to go with 302, as this will allow us to provide analytics to the user about how often their short URL was queried, even though this means additional load on our service. If we were more concerned with our server costs than analytics we would instead opt for a 301. This choice also has implications on user experience. A 301 redirect might be cached by the browser, leading to faster subsequent accesses but also potential issues if the original URL changes. Using a 302 redirect avoids this issue as it is not cached by the browser. ^5fhsVxKe

In case the alias is not found in the database, the service should return a 404 (Not Found) error. Beyond this, if the original URL is no longer valid or accessible, we should have a mechanism in place to validate URLs before redirecting users. If the URL validation fails, we could return a specific HTTP status code, such as 410 (Gone), to inform the user that the original resource is no longer available." ^fxG5LcMx

How would you ensure the system can scale to 
support the number of users you estimated in 
the back-of-the-envelope estimation? ^9AdE9rN2

Tips ^AMKp8QyQ

Can you identify a significant bottleneck in the 
system and explain how you would mitigate it? ^JxG4KFcb

Tips ^uA4QKvDV

Question ^cTiP5KeY

Tips ^XUSk35av

Answer ^o6DOoJ3j

Scalability ^mKcHSYZM

Scaling ^WvW65Jqm

BottleNecks ^dNLlLiXn

"Given our URL shortening service is a read-heavy system that uses a NoSQL key-value store database, we will primarily focus on horizontal scaling, caching, and data partitioning to handle the expected load. ^VMdpHQjg

1. Horizontal Scaling: As the user base grows, we can add more servers to handle an increasing number of requests. The NoSQL database we're using, designed for horizontal scaling, will help us distribute the data across multiple servers. Load balancers will be used to distribute incoming requests evenly across these servers to ensure none of them gets overwhelmed. The configuration and management of these load balancers would be done using tools like HAProxy or Nginx, which provide real-time monitoring and automatic failover capabilities. ^UJeswolE

2. Caching: Given the read-heavy nature of our service, we can introduce a caching layer between the application and the database. This will significantly reduce read operations on the database, as frequently accessed URLs will be stored and retrieved from the cache. Tools like Redis or Memcached can be effectively used for this purpose. In case of a cache failure, we'll have a backup cache server and an automatic failover mechanism in place. ^sqz7ZkXY

3. Data Partitioning: To manage a growing dataset in our NoSQL database, we can employ data partitioning techniques such as consistent hashing. This will help in distributing data evenly across multiple database nodes and aid in quick data retrieval. In case of a database node failure, we'll have a replica set in place to ensure data availability and consistency. ^bXx1iMK8

4. Write Operations: To handle write operations, we'll use a write-behind caching strategy. This means that write operations will first be made to the cache and then asynchronously written to the database. This will help us handle a large number of write operations while minimizing the load on the database. ^BDyEcV2u

As we monitor the performance of the system, we can fine-tune these strategies, perhaps introducing more advanced techniques such as auto-scaling based on load or time of day, to handle fluctuating user demand effectively. In case of updates or deletions, we'll use a distributed messaging system like Apache Kafka to ensure data consistency across all servers." ^3HWROFsw

"The primary bottleneck in our URL shortening service could be the latency in the retrieval of the original URL from the shortened one, especially under high loads. This is essentially a read operation on our NoSQL key-value store database. Given the nature of a URL shortening service, which is read-heavy, this operation needs to be as quick as possible for a good user experience. ^LLaFoF4s

To mitigate this bottleneck, we can employ the following strategies: ^y6XxfAFn

1. Caching: A caching layer can be introduced between the application layer and the database. The cache will store frequently accessed short URLs and their corresponding original URLs. This will significantly reduce read operations on the database. Tools like Redis or Memcached can be effectively used for this purpose. To optimize our cache size and eviction policy, we can analyze our service's usage patterns and adjust these parameters accordingly. For instance, if we observe that certain URLs are accessed more frequently during specific times of the day, we can increase the cache size during those periods and use an LRU (Least Recently Used) eviction policy to ensure that the most frequently accessed URLs remain in the cache. Furthermore, we can handle stale data in the cache by setting an appropriate Time-To-Live (TTL) for each entry. ^9llVIq68

2. Read Replicas: To handle the complexity and cost of read replicas, we can use managed database services that provide automatic replication and scaling. This not only reduces the operational overhead but also ensures that the replicas are kept in sync. However, this strategy should be used judiciously as it increases the system's complexity and costs. ^rpvEbx5i

Another potential bottleneck could be the generation of unique short URLs. To mitigate this, we can use a combination of hashing and encoding techniques to generate unique short URLs. Additionally, we can also maintain a pool of pre-generated short URLs to handle high loads. ^GljVcAvg

In addition to these strategies, it's crucial to have a robust system monitoring in place. This will allow us to detect any performance issues early and take corrective actions. Tools like Google Stackdriver or Prometheus can be used for monitoring and alerting." ^90DfjhGZ

Identify 1-2 key security measures you would 
implement in the system and provide an 
explanation of how they would be applied? ^XJH2sAPS

Tips ^TRaamAuq

How would you monitor the system 
for performance and issues? ^C1NlC72a

Tips ^2n3MDIvS

What key aspects of the system would you 
focus on when testing for functionality and reliability, 
and what testing strategies would you use for these aspects? ^9VQi3mNK

Tips ^PBBfGHZS

System Assessments ^WE9PDePx

Security ^Mq6adbHp

Monitoring ^wa4FxX3F

Testing ^ASKpp1UX

"In a URL shortening service, three key security measures we would implement are: ^Fr8IFiNN

1. URL Validation and Malicious Link Protection: Before shortening any URL, we would validate it to ensure that it is not malicious. This can be done by checking if the domain is valid, not on a blacklist of known harmful sites, and is not trying to exploit common vulnerabilities. We might also consider using a safe browsing API (like Google's) for this purpose. In case of false positives, where a legitimate site is mistakenly flagged as harmful, we would have a process in place for site owners to report and rectify the issue. Additionally, we would provide a preview feature, allowing users to see the destination of the shortened URL before they follow it, further reducing the risk of users being directed to harmful sites. ^OfN7zC3G

2. Rate Limiting and DDoS Protection: To prevent misuse of our service, such as for spamming or Denial-of-Service (DoS) attacks, we would implement rate limiting. This restricts the number of requests a single client can make in a given amount of time. We could implement this either at the IP level or user level, depending on the use case. The appropriate rate limit would be determined based on our server capacity and typical user behavior. In addition to rate limiting, we could also use IP filtering and anomaly detection as part of our DDoS mitigation strategy. IP filtering would block traffic from known harmful IPs, while anomaly detection would identify unusual traffic patterns and take appropriate action. ^Ns3VnXhk

3. Data Encryption: To protect sensitive data, we would implement encryption both at rest and in transit. For data at rest, we would use symmetric encryption for its efficiency and speed, while for data in transit, we would use asymmetric encryption for its enhanced security provided by the public-private key pair. This ensures that even if an attacker manages to gain access to the data, they would not be able to read it without the decryption key. This adds an additional layer of security and ensures the confidentiality and integrity of the data. ^IXVgsm6E

Lastly, to ensure our system remains secure in the future, we would conduct regular security audits and penetration testing. These practices would help us identify potential security vulnerabilities and address them proactively, maintaining the overall security of our service." ^2XJFJ6hW

"To effectively monitor the URL shortening service, I would focus on two key performance metrics: response time and error rates. These metrics are critical for ensuring a fast and reliable service for users. ^sKIGbDRB

To monitor response time, I would use a combination of logging and real-time analytics. Application logs would record the time taken to process each request, allowing us to analyze the data for any trends or anomalies. Real-time analytics tools, such as Prometheus, would be used to track response times and generate alerts if they exceed a predefined threshold. This would enable the team to proactively identify and resolve bottlenecks or performance issues. ^gd2A6dEM

Monitoring error rates involves tracking the number of failed requests, such as server errors or database errors, and comparing them to the total number of requests. This can be achieved using application and server logs, as well as monitoring tools like Grafana or ELK Stack. Setting up alerts for high error rates would help the team identify and fix issues before they impact a significant number of users. ^gt2lX1pL

In addition to these key metrics, I would set up regular health checks to ensure the system's components, such as the database and servers, are functioning optimally. A comprehensive dashboard would be created to provide a quick overview of the system's performance, including response times, error rates, and the results of health checks. ^JeqS5zDK

Load testing is also crucial in performance monitoring. It helps to identify how the system behaves under heavy load and identify potential bottlenecks. Regular load testing would be carried out to ensure the system can handle high traffic and to identify areas for optimization. ^WDTqEG66

Monitoring for security incidents is another important aspect of the overall health of the system. This could involve tracking unusual activity or failed login attempts and setting up alerts for potential security threats. ^mfnWKuvX

Regarding the handling of monitoring data, a log rotation strategy would be implemented to manage the storage of logs. This would involve regularly archiving old logs and only keeping recent logs readily available for analysis. The exact duration for storing logs would depend on the specific requirements and regulations of the project, but a typical approach might be to store logs for 30 days. ^hxzZ54RJ

This comprehensive approach to monitoring would enable the team to effectively monitor the system's performance and health, and address any issues in a timely manner." ^XDjVny0o

"To ensure functionality and reliability in a URL shortening service, I would focus on testing the following key aspects of the system: ^XEBbMpMd

1. URL shortening and redirection: Unit testing should be performed on individual functions responsible for generating short URLs and redirecting users to the original URLs. This will confirm that these core features are working correctly. ^cEl767jv

2. Integration testing: Since the system involves components like the web server, database, and caching, integration testing should be conducted to verify that these components interact seamlessly and produce the expected results. ^jaVztA0L

3. System testing: To validate the overall functionality of the service, end-to-end system testing should be performed, simulating real user scenarios and ensuring that all requirements are met. ^QRQLLcfO

4. Load testing: To assess the system's reliability under heavy traffic, load testing should be conducted. The strategy here would be to gradually increase the number of users or requests to the system, monitoring its response times and error rates. This will help identify bottlenecks and confirm that the system can handle a large number of concurrent users without performance degradation. ^U3kgMlQu

5. Stress testing: To check the system's behavior under extreme conditions, stress testing should be performed. The strategy for stress testing would involve subjecting the system to loads and conditions well beyond its normal operational capacity to reveal the system's breaking points and allow for improvements in its resilience. ^0HYL987H

6. Edge case testing: It's also important to test the system's ability to handle edge cases. For example, very long URLs or URLs with special characters should be tested to ensure the system can handle all possible inputs. ^L69LXFdd

Lastly, implementing automated testing using a CI/CD pipeline will enable regular checks, faster deployment, and early detection of issues, contributing to the overall functionality and reliability of the service." ^oC4Jo0MM

High Level Design ^NOHfCL5B

Major Component ^zbh10IrW

API Design ^fb4f9ovi

A URL shortening service like bit.ly or tinyurl ^eg96Bdbe

"POST /shorten
- Request: { 'url': 'original long URL', 'userId': 'user's ID (optional)' }
- Response: { 'shortUrl': 'generated short URL', 'userId': 'user's ID (if provided)' }
- Error Response: { 'message': 'Error message' }
- Purpose: Create a new short URL from the given long URL


GET /:shortUrl
- Response: Redirects to the original URL
- Error Response: { 'message': 'Short URL not found' }
- Purpose: Get and redirect to the original URL using the short URL


GET /:shortUrl/details
- Response: { 'shortUrl': 'short URL', 'url': 'original URL', 'clicks': 'number of redirects happened', 
'created_at': 'creation timestamp', 'userId': 'user's ID (if provided)' }
- Error Response: { 'message': 'Short URL details not found' }
- Purpose: Get the details of a short URL including the original URL, number of clicks or redirects, 
and the user who created the URL


GET /users/:userId/urls
- Response: { 'urls': 'Array of short URLs created by the user' }
- Error Response: { 'message': 'User or URLs not found' }
- Purpose: Get all short URLs created by a specific user

POST /users
- Request: name, email
- Response: id
- Error Response: { 'message': 'Error message' }
- Purpose: Create a user

PUT /users/:id
- Request: name, email
- Response: id, name, email
- Error Response: { 'message': 'User not found' }
- Purpose: Update a user's details

GET /users/:id
- Response: id, name, email
- Error Response: { 'message': 'User not found' }
- Purpose: Get a user by id

DELETE /:shortUrl
- Response: { 'message': 'Short URL deleted successfully' }
- Error Response: { 'message': 'Short URL not found' }
- Purpose: Delete a short URL ^aT4Ot1qh

Note: All endpoints are subject to rate limiting to prevent abuse. The rate limit could be set per IP address or user, 
and once exceeded, a response with HTTP status 429 (Too Many Requests) will be returned." ^O9tIS0vv

URL Shortener ^NrJpgdbd

Design a service that converts long URLs into short, unique aliases (e.g., tinyurl.com/abc123) and 
redirects users to the original URL when accessed. ^kTxTnd5I

• Functional Requirements
What your design needs to accomplish

- Generate unique short URLs (6-8 characters)
- Redirect users from short URLs to original URLs
- Allow custom short URLs (e.g., tinyurl.com/bitly.com)
- Track basic analytics (click count, referrer domains) ^8p0Gl8RA

• Non Functional Requirements
What your design needs to accomplish

- Handle 10M+ URL shortens per day
- Redirect latency < 100ms
- High Availability (99.9%) ^cl3SO5vC

1. Requirements ^brxQcens

2. Core Entities ^ldLMU6tt

• URL Object :
    {
        - short_url_key
        - original_url
        - creation_date
        - expiration_date
        - user_id
        - click_count
        - referrer_domain
    } ^Ho1M1Bio

3. High Level Components ^DqItsReZ

• Load Balancer : Distributes incoming traffic
• API Gateway : Handles request routing, authentication, rate limiting.
• URL Shortener Service: Generates and stores short URLs
• Redirect Service : Handles redirection from short to original URLs
• Analytics Service : Collects and processes click data
• Database (NoSQL/SQL) : Stores URL mapping and analytics data
• Cache (Redis/Memecached) : Stores frequently accessed short-to-original URL mappings.
• Asynchronous Queue (Kafka/RabbitMQ) : For analytics processing ^fAzdeLsg

4. API Endpoints ^FjYZzPLI

• POST/shorten : 
        Request body : 
            { original_url : "...",
               custom_alias: "...",                  
            }

• GET/{short_url_key} : Redirects to `original_url`

• GET/analytics/{short_url_key} : 
        Response : 
            { click_count : "...",
              referrer_domains: "..."
            }
 ^DX7sLfbJ

5. Data Flows ^QetfBwdu

1. User Request to Shorten URL:
    - User sends `POST/shorten` to LB.
    - LB forwards to API Gateway.
    - API Gateway routes to URL Shortener Service.
    - URL Shortener Service generates `short_url_key`, stores `URL Object` in Database, and populates Cache
    - URL Shortener Service returns `short_url` to user via API Gateway and LB.

2. User Request to Access Short URL:
    - User sends `GET/{short_url_key}` to LB.
    - LB forwards to API Gateway
    - API Gateway routes to Redirect Service
    - Redirect Service checks Cache for `original_url`.If not found, queries Database.
    - Redirect Service issues HTTP 301/302 redirect to `orginial_url`
    - Redirect Service published click event to Asysnchronous Queue
    - Analytics Service consumes from Asysnchronous Queue, updated `click_count` and `referrer_domains` in DB.

 ^L9B1MnM7

6. Scalability & Optimization ^8cC6zECR

• Load Balancing : Distributed incoming traffic across multiple instances of services.

•Caching : Using Redis for hot URLs to reduce database load and improve redirect latency.

• Database Sharding/Partitoning : Distributes URL data across multiple database instances to handle high write/read volumes.

• Asynchronous Processing : Using Kafka/RabbitMQ for analytics to decouple processing front critical path and ensure low direct latency.

• Service Horizontal Scaling : Ability to add more instances of URL Shortener Service, Redirect Service and Analytics Service as needed.

• Unique Key Generation : Distributed unique ID generation (e.g base62 encoding of distributed counter or UUIDs) to avoid collisions
                                and ensure uniqueness at scale.

• Content Delivery Network (CDN) : For static assets (if any) or potentially for DNS resolution of short URLs to reduce latency.

• Geo-distributed Databases/Caches : To further reduce redirect latency for global users.  ^Hy9osoql

Short Solution 2 ^waxrpcOJ

## Embedded Files
722f0ebda5224d0ed84d10005206f081fdfcbc01: [[Bitly.png]]

%%
## Drawing
```compressed-json
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

MmKzzZRKsv0yarM4shKyGrNiMs0ywzJasiMzVjPrMjKyyTLq1YwYPTP6aJMzZIgQs58yjLMzM5yzszK8tPMyMdALMkkAizJgADizAzMms7izprO3gHMyJkHcstqzpzKbM5aF5DLM01NCeZMs09zDyDOCsmgyYTCVMsEBjLKaM3syfDDcMhiyhzOUMkczifDHM+QkJzOJMuqyajOkMuczO53kM0GyMcCYslczRzLXMtiyNzNJqc6z4rIzMxKz/zLN

GFKyjzIWsk8zASjPMq4wLzISMq8zUdBvM9gy7zJdMx8y0zPKsjMz3zIvVMpVvzPss8syszJuswCzVJlas0mzPLJT4iCyujSgstMYYLIZsuCyijKfM+KBlyjKM64zITN3MqQznzLqMmo1FzMBss0YWjM4M/CzqlhYMrozUNR6M0iz+jI4ACizxeSos0YyXHiJ0eizFDMYs8GzmLIxs1izWixxs2yyJrPxsqazTTN5sviyerMOMoSzBbJEstIyukIV

s3bopLJTRB4zPzI2NHVEXjOSqN4zujLTPVY0irG+M3yzmjUHKRmz+rN0s5AAwTJT0HayjTJMslgz4TLBEyyyqTOssvgyXbIust2yrrI9svEyj5IJMuazazL9spaz0jMYlSkzfjOpM6ZimLGyspNlvrJ0syEzirJ1M0kodrMGs2qzubL2s7eBibNFM+uz2rIpsurV6TNystOyIgAKsv6y7ylGs/uyWbOnMyqzl7KisuKyubKVsnmzzTNrs32yUjP9

su0zATC6s11oerNTs7uyFTMKsoayKrJGsvuzN7OSoV2zHLPdsmQy97P5s+azD7IbsuMyirCz0NayOAAR9Bt9NrOKM5mz/DJ3skezbrJHgQ6yyJmOsvcBTrNxs7eyuLJ5siByYoHusiezHrLWgZszmAT7vYRiVFMr4+UTq+NpnJg95ODjQEK9Ew3iAR9dHdPVvFSUoABrAH4BJgHoAM7i233oDS9Dxz1yvXDSI6Kf00Sh13TIUB8glkx4/RBcKxGk

pQCR2gWSUaqiOZw6EuqiY0ATQK4sDKTufeoSzqlpkCXEFmEQMsNgPmEL0qQji9LtUznT4ULlnAbTkeL7kgXSVNPR464prqmIMkKgExIfUn/BLSVMaDtJr4EdkjJJ5pKWNHIi1QIoyOKCfdAWXFJdw3CWcTuVbOzyrSOxiwMsZJ3Q11mJSYTp/WlTw/bRh+nuwoY1lHXTtReAaggfMFUsrv2yXEHBrenLsaUs1bQDJREptbUBKWnQYAmYzOmSv4k1

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

qqeQFSAagcABGcGaTPthwd3qgEAB6oCAAA==
```
%%