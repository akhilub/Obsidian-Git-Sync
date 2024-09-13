---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data
## Text Elements
Dating App Like Tinder ^jyhQujN4

Requirements ^Q81684Nb

Non Functional Requirements ^o1BPHki6

Back of the Envelope Estimations ^D1t8FijJ

Functional Requirements ^0wiEtg3U

Storage Requirements ^TljJbjLT

Daily Active Users(DAU) ^xp2fQE2W

API Routes ^pG2xYfY5

DataBase Schema ^0ME7hiYp

High Level Design ^axodlEIN

Core Components ^octureUz

User Matching ^he4vk6Vs

Message Exchange ^u5so9ilo

User Privacy ^HQow2IZ4

How does your system handle load balancing 
to ensure smooth user experience during 
peak usage times? ^5zcucAVg

Consider how your system will distribute network or application traffic across multiple servers to ensure reliability and availability during peak usage times. ^vOnhhI0K

How does your system handle message 
exchange between matched users? ^sM0i1Jkk

Consider how your system will store and retrieve messages, ensure delivery, handle read receipts, and manage real-time communication. ^IEfaQTFg

How does your system ensure user privacy and security, 
especially in terms of location and private communication? ^V7t8P34f

Consider how your system will protect user data, especially sensitive information like location and private messages. This may involve encryption, access controls, and other security measures. ^ZKoZktcu

How would you ensure the system can scale to 
support the number of users you estimated 
in the back-of-the-envelope estimation? ^oGWxm7yi

Tips ^nNRgkcRL

Can you identify a significant bottleneck in the system 
and explain how you would mitigate it? ^QJm93HAL

Tips ^7nWiTErU

Question ^ghjSuZsN

Tips ^sBX4LSyr

Answer ^l22Mc1b8

Identify 1-2 key security measures you would implement in 
the system and provide an explanation of how they would be applied? ^N154diwE

Tips ^CzJosPaR

How would you monitor the system for performance and issues? ^xmawagYC

Tips ^0hhnws0S

What key aspects of the system would you focus on when testing for 
functionality and reliability, and what testing strategies 
would you use for these aspects?
 ^D5oXwYf3

Tips ^EH52nl7R

Scalability ^c3NFORcS

Scaling ^IbTsUJTQ

BottleNecks ^diOvxZ0I

System Assessments ^3p4sgObp

Security ^1nefJ6jH

Monitoring ^ARVX99Qn

Testing ^y4W1zgbH

Understand the Requirements ^F0PhDyrm

Requirements ^dyenJm8S

Back of the Envelope Estimations ^Q0oLphq9

CAP Theorem ^cuY5Iz2V

Data Model ^bXDRXglw

Database Type ^Lp214dOa

DataBase Schema ^vj2J0Bug

DataBase Replication ^7clgt11O

What type of database would you use for the system and why? ^A9zvVl4v

Tips ^dneJkM7S

How would you handle data 
availability, replication, and synchronization? ^YyjDOypX

Tips ^NUST9zU9

Given the constraints of the CAP theorem, 
where would you place the emphasis for your system: 
consistency or availability? Justify your answer. ^KgVMZwSh

Tips ^zuBW5PZ7

A dating app like Tinder requires a well-designed system to handle user profiles, preferences, geolocation, matching algorithm, real-time chat, and notifications. 
It necessitates considerations for data privacy, scalability, high availability, and efficient searching and matching algorithms. ^MGZUVv2D

- Users should be able to create a profile with personal information and preferences.
- Users should be able to direct message their matches
- Users should be able to browse and swipe on other users' profiles based on their preferences.
- Users should be able to match with other users when both have swiped right.
- Users should be able to chat with their matched users. ^tJQ6jbeb

- The system should scale to support a large number of users and high frequency of interactions.
- The system should provide real-time updates for matches and messages.
- The system should ensure user data privacy and security.
- The system should have a high availability and reliability to maintain user engagement." ^C5B3Mmfs

Given the popularity of dating apps, let's make a rough estimation:

3B (total smartphone users globally)
* .03 (percentage of population interested in using dating apps)
* .05 (our estimated market share among all dating apps)
= 4.5M DAU
 ^5SwNzCXm

User Data:
Profile data includes pictures, bios, interests, and other relevant information:

Profile Picture = 500KB (average picture size) * 5 (average number of pictures per user)
Bio & Interests = 1KB

4.5M (dau)
* 2.5MB (total user data)
* 2 (redundancy)
* 2 (backup)
* 2 (growth)
= about 90TB

Transactional Data:
Assuming interactions such as swipes, likes, and matches. An average user might have a total of 200 interactions a day.

4.5M (dau)
* 200 (average interactions)
* 500b (per interaction, with metadata)
* 2 (redundancy)
* 2 (backup)
* 365 (days per year)
= about 328TB annually

Log Data:
For troubleshooting, monitoring, and analytics, let's estimate:

4.5M (dau)
* 5KB (average log data per user per day)
* 365 (days per year)
= about 37TB annually

Considering an additional 20% overhead for other database operations and future scaling, the total becomes:

90TB + 328TB + 37TB + 20% overhead = about 550TB annually." ^JL5ofeAD

I would place the emphasis on availability for a dating app like Tinder. The CAP theorem states that it's impossible for a distributed data store to simultaneously provide more than two out of the following three guarantees: Consistency, Availability, and Partition Tolerance. Partition tolerance is a must-have in a distributed system to handle network failures, so the trade-off lies between consistency and availability. ^qGi06GQY

In the context of a dating app, availability takes precedence over consistency. Users expect the app to be always available for them to swipe, send messages, and update their profiles. If there is a slight delay in updating the profiles across all nodes, it might not significantly impact the user experience. However, if the app is not available, users will not be able to interact with it, leading to a poor user experience. ^mjv4O9gO

Therefore, prioritizing availability over consistency would be the optimal choice for a dating app like Tinder, considering the nature and requirements of the system." ^zkiE7Nkv

A dating app like Tinder ^MVf6AGKP

"Here are the updated API endpoints for a dating app like Tinder, taking into account the feedback received:

1. GET /users/{id}
- Request: User ID
- Response: User's profile information
- Purpose: To retrieve a user's profile. Returns a 404 error if the user does not exist.

2. POST /users
- Request: User's profile information
- Response: Created user's ID
- Purpose: To create a new user. Returns a 400 error if required profile information is missing or invalid.

3. PUT /users/{id}
- Request: User ID and new profile information
- Response: Updated user's profile information
- Purpose: To update a user's profile. Returns a 404 error if the user does not exist and a 400 error if the new profile information is invalid.

4. DELETE /users/{id}
- Request: User ID
- Response: No content
- Purpose: To delete a user's profile. Returns a 404 error if the user does not exist.

5. GET /users/{id}/matches?page={page}&limit={limit}
- Request: User ID, Page number and Limit
- Response: List of matched user's IDs
- Purpose: To retrieve the list of users that the current user has matched with, with pagination. Returns a 404 error if the user does not exist.

6. POST /users/{id}/likes
- Request: User ID and liked user's ID
- Response: Match status (whether the like resulted in a match)
- Purpose: To like another user's profile. Returns a 404 error if either user does not exist and a 400 error if the user tries to like their own profile.

7. DELETE /users/{id}/likes
- Request: User ID and liked user's ID
- Response: No content
- Purpose: To undo a like on another user's profile. Returns a 404 error if either user does not exist.

8. GET /users/{id}/likes
- Request: User ID
- Response: List of liked user's IDs
- Purpose: To retrieve the list of users that the current user has liked but not yet matched with. Returns a 404 error if the user does not exist.

9. POST /users/{id}/dislikes
- Request: User ID and disliked user's ID
- Response: No content
- Purpose: To dislike another user's profile. Returns a 404 error if either user does not exist and a 400 error if the user tries to dislike their own profile.

10. DELETE /users/{id}/dislikes
- Request: User ID and disliked user's ID
- Response: No content
- Purpose: To undo a dislike on another user's profile. Returns a 404 error if either user does not exist.

11. GET /users/{id}/messages?page={page}&limit={limit}
- Request: User ID, Page number and Limit
- Response: List of messages
- Purpose: To retrieve the list of messages for a user, with pagination. Returns a 404 error if the user does not exist.

12. POST /users/{id}/messages
- Request: User ID, recipient's ID, and message content
- Response: Sent message's ID
- Purpose: To send a message to another user. Returns a 404 error if either user does not exist and a 400 error if the message content is invalid." ^9vQEU2R3

"I would suggest using a combination of both relational and NoSQL databases for a dating application like Tinder. ^FYrJIx0W

For user profiles, which have structured data like name, age, gender, preferences, etc., a relational database like PostgreSQL would be a good fit. This is because relational databases provide strong consistency and are great for complex queries, which we might need when matching users based on multiple criteria. ^3jbYrBKN

However, when it comes to chat data, which is unstructured and can scale massively, a NoSQL database like Cassandra would be more suitable. NoSQL databases are designed to handle large amounts of data across many commodity servers, providing high availability with no single point of failure. This would be crucial for a feature like chat, which needs to be real-time and highly available. ^cdjnlMMu

Moreover, using a NoSQL database for chat data would also allow us to handle the write-heavy load efficiently as chats between users can grow rapidly. ^GbtQ1wbT

Therefore, a hybrid approach of using both relational and NoSQL databases would allow us to effectively handle the diverse data and scalability requirements of a dating application like Tinder." ^Qc1opWbc

User
- id: string (unique identifier)
- name: string
- email: string (unique)
- password: string
- gender: string
- preference: string
- bio: string
- city: string
- state: string
- country: string
- age: integer
- photos: list of strings

Match
- id: string (unique identifier)
- user1Id: string (references User.id)
- user2Id: string (references User.id)
- matchedAt: datetime
- status: string

Message
- id: string (unique identifier)
- senderId: string (references User.id)
- receiverId: string (references User.id)
- matchId: string (references Match.id)
- sentAt: datetime
- content: string

Swipe
- id: string (unique identifier)
- swiperId: string (references User.id)
- swipeeId: string (references User.id)
- direction: string
- swipedAt: datetime ^GTY60wN2

In this updated schema, I have added a 'password' field in the User table for authentication purposes. I have also broken down the 'location' field into 'city', 'state', and 'country' for more precise location-based searches. Additionally, I have added a 'status' field to the Match table to track whether a match is still active or not. ^lx1g7gJ3

The User table still represents the app users with their basic details and preferences. The Match table represents the matches between two users, now with the ability to track the status of the match. The Message table represents the messages exchanged between two matched users. The Swipe table represents the swipes made by the users. ^Q5AmHuVK

This updated schema still allows us to efficiently store all necessary data and perform queries relevant to the app's functionalities, now with added functionality for authentication, precise location-based searches, and match status tracking." ^6KjHkJLg

Load Balancing ^hGaK6aHK

"Our dating app will employ a combination of horizontal scaling and load balancing to ensure a smooth user experience during peak usage times. Our system will be designed to be stateless, which means that we can add or remove servers as demand changes without affecting the system's performance. This is achieved through a process known as horizontal scaling. ^cmsh1d8Q

For load balancing, we will use both DNS load balancing and software-based load balancing. DNS load balancing is a simple and efficient method that distributes requests across multiple IP addresses. However, it has limitations such as not considering the current load of each server. Software-based load balancers, on the other hand, can consider the current load of each server, distributing requests more evenly and preventing any single server from becoming a bottleneck. ^TqVYXYG4

We will also use session persistence, also known as sticky sessions. This ensures that once a user is connected to a specific server, all subsequent requests from that user are directed to the same server. This is important for a dating app like ours where real-time updates and interactions are key to the user experience. However, there is a potential downside to sticky sessions. If the server a user is connected to goes down, the user's session would be interrupted. To handle this, we would design our system to immediately reassign the user to a different server and restore the session data from a centralized data store or cache. ^ghykuiS9

In terms of horizontal scaling, we will monitor key metrics such as CPU usage, memory usage, and network traffic to determine when to add or remove servers. If we observe that these metrics are consistently high, it's an indication that we need to add more servers. Conversely, if these metrics are low, we can remove servers to save costs. ^QEKfIoBe

In the event of a server failure, the load balancer will automatically redirect traffic to the remaining online servers. In the meantime, the failed server will be replaced or repaired without affecting the overall performance of the system. This ensures high availability and reliability of our service. ^PovC17hN

Finally, to further optimize our load balancing, we will use real-time analytics to monitor our system performance and identify any potential issues before they affect our users. This will allow us to continually adjust and improve our load balancing strategy, ensuring a smooth user experience regardless of the load on our system." ^lUj8b9bu

"Our system ensures efficient and secure message exchange between matched users using the following strategies: ^u4rcrFX1

1. Real-time Communication: We use WebSockets for bi-directional, real-time messaging. This ensures users see messages instantly. ^jtIbdcyI

2. Storage: Messages are stored in a NoSQL database like Cassandra, optimized for high volume and scalability. Each message has fields for sender, receiver, timestamp, content, and delivery status. ^FhE6FnCA

3. Reliable Delivery: A message queue, such as RabbitMQ, ensures messages are delivered even if the recipient is temporarily unavailable. This prevents message loss due to network issues. ^RsDXHvg7

4. Security: Messages are encrypted using algorithms like AES. We also use secure WebSockets to protect the communication between client and server. ^kgxkTZZK

5. Performance: We leverage caching with Redis for recent messages, reducing database load. Sharding distributes data across databases, and a load balancer manages traffic. Auto-scaling adjusts server count based on load. ^sKgR0L8k

In essence, our system combines real-time communication, efficient storage, reliable delivery, robust security, and high performance to facilitate seamless messaging for users." ^V7HFp2tE

"Our system prioritizes user privacy and security through several measures: ^iAFdHBI0

1. Communication: We use end-to-end encryption for all user communications, ensuring only the sender and receiver can decipher messages. ^W9lPpPg4

2. Location Data: We store approximate locations, not exact GPS coordinates. Users can also disable location services. All data transmissions use secure HTTPS protocols. ^JnHRQMLw

3. Data Anonymization: User data stored on our servers is anonymized, making individual identification impossible. ^LpSThFrC

4. Controlled Access: Only authorized personnel can access user data, and we enforce strong password policies and two-factor authentication. ^b7znRB25

5. Transparency: Users are informed about data collection, usage, and retention. They control their data via privacy settings and can permanently delete their accounts. ^SzHK4F0C

6. Compliance & Audits: We adhere to data protection laws, and regularly conduct security audits, including penetration tests, to ensure robustness. ^w2MB9KQX

7. Breach Response: In case of a data breach, we have a plan to secure systems, investigate the issue, and notify affected users with protective steps." ^gBxRptFG

How does your system handle load balancing 
to ensure smooth user experience during peak usage times? ^Bnj3U9uT

Consider how your system will distribute network or application traffic across multiple servers to ensure reliability and availability during peak usage times. ^V9DRZ6QL

"Our dating app will employ a combination of horizontal scaling and load balancing to ensure a smooth user experience during peak usage times. Our system will be designed to be stateless, which means that we can add or remove servers as demand changes without affecting the system's performance. This is achieved through a process known as horizontal scaling. ^U9XkwM1w

For load balancing, we will use both DNS load balancing and software-based load balancing. DNS load balancing is a simple and efficient method that distributes requests across multiple IP addresses. However, it has limitations such as not considering the current load of each server. Software-based load balancers, on the other hand, can consider the current load of each server, distributing requests more evenly and preventing any single server from becoming a bottleneck. ^AxTdDU7Q

We will also use session persistence, also known as sticky sessions. This ensures that once a user is connected to a specific server, all subsequent requests from that user are directed to the same server. This is important for a dating app like ours where real-time updates and interactions are key to the user experience. However, there is a potential downside to sticky sessions. If the server a user is connected to goes down, the user's session would be interrupted. To handle this, we would design our system to immediately reassign the user to a different server and restore the session data from a centralized data store or cache. ^1BifhzjA

In terms of horizontal scaling, we will monitor key metrics such as CPU usage, memory usage, and network traffic to determine when to add or remove servers. If we observe that these metrics are consistently high, it's an indication that we need to add more servers. Conversely, if these metrics are low, we can remove servers to save costs. ^NbFws3y0

In the event of a server failure, the load balancer will automatically redirect traffic to the remaining online servers. In the meantime, the failed server will be replaced or repaired without affecting the overall performance of the system. This ensures high availability and reliability of our service. ^4VhOmbbt

Finally, to further optimize our load balancing, we will use real-time analytics to monitor our system performance and identify any potential issues before they affect our users. This will allow us to continually adjust and improve our load balancing strategy, ensuring a smooth user experience regardless of the load on our system." ^VSvPYgIa

"To ensure that our dating app can scale effectively to support the estimated number of users, we would employ a combination of horizontal scaling, load balancing, data partitioning, and caching. ^XlV3Yl7J

1. Horizontal Scaling: Given the nature of a dating app, it's likely that we'll experience significant growth in user base over time. We can handle this by adding more servers as the user base grows. This approach, known as horizontal scaling, will allow us to accommodate more users without degrading the performance of the system. We'll use a cloud-based infrastructure which allows us to easily add or remove servers based on demand. ^dBZr0gdX

2. Load Balancing: To ensure that the load is evenly distributed across all servers, we'll implement load balancers. This will prevent any single server from becoming a bottleneck, thereby maintaining system performance even during peak usage periods. ^stt2FXa0

3. Data Partitioning: As the amount of user data grows, we'll need to manage our database efficiently. We can use data partitioning techniques to distribute data across multiple databases. This can be done based on user location or user ID, ensuring that related data is stored together and can be quickly retrieved when needed. ^LqduD6F0

4. Caching: To further enhance performance, we can implement a caching layer. This will store frequently accessed data, such as user profiles or match recommendations, reducing the need for expensive database queries. We can use caching solutions like Redis or Memcached for this. ^LcT99Rpj

5. Microservices: To ensure each part of the system can scale independently based on its own needs, we'll adopt a microservices architecture. This way, services that experience higher demand can be scaled up independently of the rest of the system. ^HzKhqGmF

By implementing these strategies, we can build a system that scales effectively with our user base, maintaining performance and reliability as the app grows." ^oiHm6aBX

"One potential bottleneck in the system could be the high read and write demands on the database due to the nature of a dating app like Tinder. The database needs to handle a large number of user profiles, matches, messages, and other user activities, which can significantly stress the system, especially during peak usage times. This can result in slow response times and a poor user experience. ^OI3yGl2A

To mitigate this bottleneck, I would propose the following strategies: ^QZLnsyjh

1. Database Sharding: Sharding the database can distribute the load among multiple databases, reducing the stress on any single system. This would involve splitting the data based on some logical partitions (like user geographical location) and storing them across different databases. ^3xNCF38v

2. Caching: Implementing a caching layer can help reduce the read load on the database. Frequently accessed data, such as popular user profiles or recent matches, can be stored in the cache. This can significantly improve the response time for these frequent queries. Tools like Redis or Memcached can be used for this purpose. ^azrzoC8e

3. Optimized Queries: Improving the efficiency of database queries can also help mitigate this bottleneck. This could involve using indexes for frequently queried fields, optimizing the database schema, or using more efficient query methods. ^KvY1Uv0L

4. Using a NoSQL Database: Considering the nature of the app, a NoSQL database like MongoDB or Cassandra might be a good fit. These databases are designed to handle high read and write loads and can scale horizontally to handle increased demand. ^zVyMQpij

These strategies, while mitigating the primary bottleneck, also ensure the overall efficiency and scalability of the system." ^BukmQ71K

"For a dating app like Tinder, I would prioritize four key security measures: encryption, secure access controls, two-factor authentication, and regular security updates and testing. ^ZoMrvlan

Firstly, encryption is crucial to ensure the confidentiality of user data. I would implement the Advanced Encryption Standard (AES) for data at rest and Secure Sockets Layer (SSL) or Transport Layer Security (TLS) for data in transit. AES is a symmetric encryption algorithm that is widely recognized and trusted for its security. SSL/TLS, on the other hand, ensures that data transmitted between the user's device and our servers is encrypted and secure, preventing man-in-the-middle attacks. ^54Wvhsyk

Secondly, secure access controls are essential to protect user data from unauthorized access. I would implement a role-based access control (RBAC) system. In RBAC, access decisions are based on the roles that individual users have as part of an organization. Users are only allowed to access data that they are authorized to see. This will prevent unauthorized access to sensitive user data and limit the potential damage from a compromised user account. ^5JfghymL

Thirdly, I would implement two-factor authentication (2FA) for user accounts. This provides an additional layer of security, as it requires users to verify their identity using a second factor (like a text message or an email) in addition to their password. This makes it much harder for attackers to gain access to user accounts, even if they manage to steal or guess a user's password. ^wqB2CkNG

Lastly, I would ensure regular security updates and testing. This includes regularly updating our encryption algorithms and access control mechanisms to keep up with the latest security standards, as well as regularly testing our system for potential vulnerabilities. In case of a security breach, we would have an incident response plan in place to quickly identify, contain, and resolve the issue. ^qBWp9EfI

These security measures will not only protect user data but also enhance user trust in our system, which is crucial for a dating app where users share sensitive personal information." ^ptNMXl5k

"To monitor our dating app, we would focus on two key performance metrics: response time and error rates. These metrics are crucial because they directly impact the user experience and the reliability of the app. ^tMiWC6QD

Response time is a crucial metric because it measures the time it takes for the system to respond to user requests. If the response time is high, users may become frustrated and stop using the app. We would use a tool like Prometheus to monitor response times in real-time. This tool would allow us to set up alerts so that we can be notified immediately if response times exceed a certain threshold. We would also set up dashboards using Grafana to visualize the response time data, which would help us identify trends and patterns. ^0KLtOCS8

As the app grows and usage patterns change, we would regularly review and adjust these performance thresholds to ensure they remain relevant and useful. We would also use A/B testing and other methods to evaluate the impact of any changes or updates on the system's performance. ^RsNISdDS

Error rates are another important metric because they indicate the reliability of the app. High error rates could mean that there are bugs in the code or issues with the infrastructure. We would use a tool like Sentry to capture and track errors in real-time. This tool would give us detailed information about each error, including the stack trace, which would help us debug and fix the issue quickly. We would also set up alerts so that we can be notified immediately if error rates exceed a certain threshold. ^7tQZUowm

In addition to monitoring these key performance metrics, we would also monitor user behavior and engagement metrics using tools like Google Analytics. These metrics can provide valuable insights into the app's performance and user experience, helping us identify areas for improvement. ^RnWupxk0

Finally, we would regularly review the system logs to identify any potential issues that might not be captured by the monitoring tools. We would use a tool like ELK Stack (Elasticsearch, Logstash, Kibana) for log aggregation and analysis. This tool would allow us to search and filter the logs based on various criteria, which would help us identify patterns and trends. ^Q5PMEHOw

By monitoring these key performance metrics, regularly reviewing and updating performance thresholds, conducting A/B testing, and monitoring user behavior, we can ensure that our dating app is performing optimally and providing a good user experience." ^Tlb9sWvM

"For a dating app like Tinder, the key aspects to focus on when testing for functionality and reliability would be User Profile Management, Matching Algorithm, Messaging, System Performance, User Interface, and System Resilience. ^8xXHS0lt

For User Profile Management, this includes user registration, login, profile creation, and profile editing. These are core functionalities that every user will interact with. Unit testing would be used here to ensure each function works as expected. For instance, after a user has registered, we should be able to retrieve the same user's details from the system. Additionally, edge cases such as attempting to register with an already used email or invalid data should also be tested to ensure the system handles these scenarios appropriately. ^GowShhu4

The Matching Algorithm is also a critical component as it's the main feature of the app. This would require a combination of unit testing and integration testing. Unit testing would ensure that the algorithm correctly identifies potential matches based on user preferences. Integration testing would verify that the algorithm correctly interacts with other components of the system, such as the User Profile Management system. ^4zh1SM4d

Messaging is another key aspect, as it is the means by which users interact. This would require integration testing to ensure that messages are correctly sent and received between users, and that the system correctly handles situations such as users blocking each other or deleting their accounts. ^h8RrDA5s

System Performance is crucial, especially under high load. This would be tested using load testing and stress testing to ensure the system can handle a large number of simultaneous users and requests, and that it degrades gracefully under extreme load. This ensures that the user experience remains positive even during peak usage times. ^3fUoaVTG

For User Interface, usability testing would be conducted to ensure that the interface is intuitive and easy to use. This would involve tasks such as testing the navigation, layout, and responsiveness of the interface. ^sjs0yn3K

Finally, System Resilience would be tested to ensure that the system can recover from failures. This would involve testing the system's backup and recovery mechanisms, as well as its ability to handle and recover from errors and crashes. This would ensure that the system is robust and reliable, providing a seamless user experience." ^5nLEXqIL

"To handle data availability, replication, and synchronization in a dating app like Tinder, I would take the following approach:


1. Data Availability: High data availability is crucial for the app's matching and messaging features. I would use a distributed database system like Google Cloud Spanner or Amazon Aurora that provides redundant storage, ensuring that data is always accessible.


2. Replication: I would adopt a master-slave architecture where the master handles writes and slaves handle reads. This would distribute the load and improve system performance. The choice between synchronous and asynchronous replication would depend on the system's tolerance for latency and potential data inconsistency. For instance, in a dating app, if a user swipes right on a profile, we would want that action to be immediately reflected across all instances of that user's profile to prevent any potential mismatches. This would require synchronous replication. However, if the system prioritizes low latency, asynchronous replication would be a better choice, with the understanding that it might lead to temporary data inconsistency.


3. Synchronization: A consensus algorithm like Raft could be used to ensure that all copies of the data across different nodes are consistent. However, in cases where high availability is more important, we might need to accept eventual consistency. For example, in the messaging feature of the app, we might prioritize availability over absolute consistency. Even if some messages take a bit longer to appear on all devices, it's more important that the service remains available.


In case of potential data inconsistency with asynchronous replication, one strategy could be to use a conflict resolution protocol. For instance, we could use a timestamp-based approach where the most recent update wins. Another strategy could be to allow the user to manually resolve conflicts when they occur.

For handling failover in the master-slave architecture, we could implement automatic failover where one of the slaves is promoted to be the new master if the original master goes down. This would ensure that the system can recover quickly and continue to provide a good user experience.


The choice of these strategies should be based on the specific needs and constraints of the system. For instance, if the system values data consistency over availability, then the strategies would be skewed towards achieving higher consistency." ^16E9s4TM

## Embedded Files
eb2e616b6df96cfd7e61e7417b2ce9121b8f0a63: [[Tinder.png]]

%%
## Drawing
```compressed-json
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=
```
%%