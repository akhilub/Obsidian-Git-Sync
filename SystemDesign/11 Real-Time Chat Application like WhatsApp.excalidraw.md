---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data
## Text Elements
Real-Time Chat Application like WhatsApp ^jyhQujN4

Chat applications enable real-time communication across platforms, with features like direct and group messaging, media sharing, and status updates. 
Their design emphasizes scalability, availability, latency reduction, and secure data transmission. ^MGZUVv2D

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

Message Sending ^he4vk6Vs

Group Chat ^u5so9ilo

Offline Delivery ^HQow2IZ4

How does your system handle the sending and receiving of messages, 
including different types of media? ^5zcucAVg

To send and receive messages in real-time, consider using WebSockets or similar technology that allows for bidirectional communication between client and server. For handling different types of media, you might consider storing media on a separate storage service and sending a reference or URL in the message payload. ^vOnhhI0K

How does your design support group chat functionality with 
multiple users?
 ^sM0i1Jkk

In a group chat, multiple users can send and receive messages in a single chat room. Consider how you would store and manage relationships between users and chats. Also, think about how you would handle message delivery to all users in the chat. ^IEfaQTFg

How does your system ensure messages are delivered even if 
the recipient is offline? ^V7t8P34f

To ensure messages are delivered even if the recipient is offline, consider storing the messages on a server until the recipient is online. You might also need to handle push notifications to inform the user when new messages are received. ^ZKoZktcu

How would you ensure the system can scale to support the number of 
users you estimated in the back-of-the-envelope estimation? ^oGWxm7yi

Tips ^nNRgkcRL

Can you identify a significant bottleneck in the system and explain 
how you would mitigate it? ^QJm93HAL

Tips ^7nWiTErU

Question ^ghjSuZsN

Tips ^sBX4LSyr

Answer ^l22Mc1b8

Identify 1-2 key security measures you would implement in the system and 
provide an explanation of how they would be applied? ^N154diwE

Tips ^CzJosPaR

How would you monitor the system for performance and issues? ^xmawagYC

Tips ^0hhnws0S

What key aspects of the system would you focus on when
 testing for functionality and reliability, and what testing 
strategies would you use for these aspects? ^D5oXwYf3

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

DataBase Type ^K39fwyNx

DataBase Replication ^p8n2Yg4w

What type of database would you use for the system and why? ^A9zvVl4v

Tips ^dneJkM7S

How would you handle data availability, replication,
 and synchronization? ^YyjDOypX

Tips ^NUST9zU9

Given the constraints of the CAP theorem, where would you 
place the emphasis for your system: consistency or availability? 
Justify your answer. ^KgVMZwSh

Tips ^zuBW5PZ7

High Level Design ^cJg1tNO7

Major Component ^AeDHKtLJ

API Design ^tRYCLzK7

CAP Theorem ^iiD7hRjc

Message Sending ^R3HeMv2F

Group Chat ^jmC13PiG

Offline Delivery ^W7PUpw57

Sacling ^Ia9jtYZb

BottleNecks ^M9VK2mBv

Security ^AwlkWFJC

Monitoring ^g94KT2N1

Testing ^xsv0EhUL

Chat application like WhatsApp or Messenger ^fki15RqO

- Allow users to send and receive text messages
- Support group chats with multiple participants
- Enable users to share multimedia files like images, videos, and audio
- Provide read receipts and typing indicators
- Allow users to search through their chat history
- Support offline messaging
- Implement end-to-end encryption for message privacy ^xZnZ7Tf3

- Handle a large number of concurrent users and messages
- Ensure low latency for message delivery
- Provide high availability and reliability
- Maintain user data security and privacy
- Scale to accommodate an increasing number of users and messages over time
- Optimize for mobile devices and different network conditions" ^vuSFV1Ob

Considering the popularity of chat applications, we can make a rough estimation for a chat app just entering the market:

3B (total smartphone users globally)
* .02 (percentage trying out a new chat application)
* .10 (our estimated market share among the new chat apps)
= 6M DAU ^Sp3Gb2Cx

Considering a chat app will store texts, media files, and other metadata, let's break down the storage requirement:

Message Row = 500b (text message) + 5MB (average media size, considering not every message has media) + 200b (metadata) = about 5MB

6M (dau)
* 20 (average number of messages per day, including text and media)
* 5MB (Message Row)
* 2 (redundancy)
* 2 (backup)
* 2 (growth)
* 365 (days per year)
= about 220PB annually

Transactional Data:
Assuming transactional data such as last seen, online status, and delivery receipts, each entry might be about 200 bytes. Hence,

6M (dau)
* 20 (status changes, delivery receipts, etc. per day)
* 200b
* 2 (redundancy)
* 2 (backup)
* 365 (days per year)
= about 17TB annually

Log Data:
For troubleshooting, monitoring, and analytics, let's estimate the log data:

6M (dau)
* 5KB (average log data per user per day)
* 365 (days per year)
= about 110TB annually

Considering an additional 20% overhead for other database operations and future scaling, the total becomes:

220PB + 17TB + 110TB + 20% overhead = about 300PB annually." ^o2z5kash

"For a chat application like WhatsApp or Messenger, I would prioritize partition tolerance and availability over consistency -- making this an AP system. Given the functional and non-functional requirements, users expect the service to be highly available and reliable, with low latency for message delivery. In the context of a chat application, it's acceptable to have eventual consistency, where data becomes consistent over time. This trade-off ensures high availability and partition tolerance, even if it means that messages or read receipts might exhibit slight inconsistencies for a brief period. The system should still strive to minimize these inconsistencies, but the primary focus should be on providing a seamless, responsive user experience in various network conditions and handling a large number of concurrent users and messages." ^Jb5VfL0Q

Answer coming soon. ^NtOEstpm

"1. User Registration

Endpoint: POST /users
Purpose: To register a new user.
Request: { 'username': 'string', 'password': 'string' }
Response: { 'id': 'string', 'username': 'string', 'createdAt': 'timestamp' }

2. User Login

Endpoint: POST /users/login
Purpose: To authenticate a user.
Request: { 'username': 'string', 'password': 'string' }
Response: { 'token': 'string' }

3. Send Message

Endpoint: POST /messages
Purpose: To send a message from one user to another.
Request: { 'senderId': 'string', 'receiverId': 'string', 'message': 'string' }
Response: { 'id': 'string', 'senderId': 'string', 'receiverId': 'string', 'message': 'string', 'createdAt': 'timestamp' }

4. Receive Messages

Endpoint: GET /messages
Purpose: To get all messages for a user.
Request: { 'userId': 'string' }
Response: [ { 'id': 'string', 'senderId': 'string', 'receiverId': 'string', 'message': 'string', 'createdAt': 'timestamp' } ]

5. Create Group

Endpoint: POST /groups
Purpose: To create a new group.
Request: { 'name': 'string', 'userId': 'string' }
Response: { 'id': 'string', 'name': 'string', 'createdAt': 'timestamp' }" ^jyXI9tSB

I would choose a hybrid approach, using both a relational database like PostgreSQL and a NoSQL database like Cassandra. The relational database would handle structured data such as user profiles, contact lists, and group information. This is because relational databases provide strong consistency, support for complex queries, and are well-suited for managing relationships between data entities.


On the other hand, I would use Cassandra for storing chat messages and multimedia files. This is because Cassandra offers high write and read performance, low latency, and excellent scalability, which are essential for a chat application handling a large volume of messages and files. Additionally, Cassandra's distributed architecture provides fault tolerance and ensures that the system remains available even in the case of node failures.


By combining both types of databases, we can leverage the strengths of each and address the specific needs of the chat application. We can also mitigate the limitations of each database type by using them for the tasks they are best suited for." ^0Fx0gFtk

To design the database schema for a chat application like WhatsApp or Messenger, I would identify the following primary entities and their attributes:

1) Users: This entity represents the users of the application. Attributes include 
user_id (primary key), 
username, 
display_name, 
phone_number, 
email, and profile_picture.

2) Conversations: This entity represents individual and group chats. Attributes include 
conversation_id (primary key), 
type (individual or group), and 
name (for group chats).

3) Participants: This entity represents the relationship between users and conversations. Attributes include participant_id (primary key), 
user_id (foreign key), and 
conversation_id (foreign key).

4) Messages: This entity represents the messages sent within a conversation. Attributes include 
message_id (primary key), 
conversation_id (foreign key),
 sender_id (foreign key), 
content, 
timestamp, and 
status (e.g., sent, delivered, read).

5) Attachments: This entity represents multimedia files shared within a conversation. Attributes include attachment_id (primary key), message_id (foreign key), file_url, file_type, and file_size.

The relationships between these entities can be defined as follows:
- Users have a one-to-many relationship with Participants, as a user can be a participant in multiple conversations.
- Conversations have a one-to-many relationship with Participants, as a conversation can have multiple participants.
- Conversations have a one-to-many relationship with Messages, as a conversation can have multiple messages.
- Messages have a one-to-many relationship with Attachments, as a message can have multiple attachments.

This schema design aims to strike a balance between efficient storage and query performance while considering the specific needs and use cases of the chat application. Some denormalization might be necessary to improve performance, such as including the sender's display name in the Messages entity to avoid additional joins when displaying messages. However, this should be done judiciously to minimize data redundancy and maintain consistency." ^vTMMYxSr

"To handle data availability, replication, and synchronization in a chat application like WhatsApp or Messenger, I would consider the following strategies:

1. Data Availability: To ensure high data availability, I would use redundant storage and frequent backups. We can use cloud storage services like AWS S3 or Google Cloud Storage that offer built-in redundancy and durability. This would ensure that chat history, user data, and multimedia files are always available even in case of hardware failures or other issues.

2. Replication: I would use a combination of master-slave and peer-to-peer architectures for data replication, depending on the specific needs of the system. The master-slave replication can be used for the relational database (e.g., PostgreSQL) to handle user profiles and group information, ensuring strong consistency. For the NoSQL database (e.g., Cassandra) storing chat messages and multimedia files, a peer-to-peer replication strategy can be used, which offers high availability, fault tolerance, and scalability.

In terms of synchronous and asynchronous replication, the choice between the two depends on the system's tolerance for latency and potential data inconsistency. Synchronous replication ensures that all changes are propagated to all replicas before the operation is considered complete. This ensures strong consistency but at the cost of increased latency. On the other hand, asynchronous replication allows the operation to complete as soon as the changes are stored in the master, with the changes propagated to the replicas in the background. This reduces latency but can lead to temporary inconsistencies between replicas. As chat applications typically prioritize low latency, asynchronous replication may be more suitable, accepting the trade-off of eventual consistency.

3. Synchronization: In a distributed system like our chat application, synchronization can be achieved using consensus algorithms like Paxos or Raft. These algorithms help ensure that the system maintains a consistent state across all nodes, even in the presence of failures or network partitions. In the event of network failures or partitions, these consensus algorithms can help maintain system availability and consistency by ensuring that only the majority of nodes need to agree on the system state. If a node gets partitioned from the network, it won't be able to participate in the consensus process, thus preventing it from making unilateral changes to the system state that could lead to inconsistency. Once the partition is resolved, the node can rejoin the consensus process and catch up with the latest system state." ^7Kl0zVoW

Delivery Receipts ^FFZrqZxn

How does your system handle message delivery status 
and read receipts? ^YU7T338y

Delivery Receipts ^azRKaQwR

"To handle the sending and receiving of messages in real-time, we can use WebSocket or a similar technology that allows for bidirectional communication between the client and server. This enables the server to push messages to clients as soon as they're received, providing a seamless chat experience.

For handling different types of media, we can store the media files on a separate storage service, such as Amazon S3 or Google Cloud Storage. When a user sends a multimedia message, the client application would first upload the media file to the storage service and obtain a unique URL or reference. Then, the message payload would include this URL or reference, allowing the recipient to download the media file when they receive the message.

This approach decouples the storage of media files from the chat application, allowing for better scalability and performance. Additionally, it ensures that the chat application only needs to handle the metadata of the media files, simplifying the message processing logic in the Business Logic Layer.

To ensure message delivery in the event of failures or network issues, we can implement a message acknowledgment system. The server would send an acknowledgment to the sender once it has successfully received and processed a message. If the sender does not receive this acknowledgment within a certain time frame, it would resend the message.

Regarding security, all media files would be encrypted before being uploaded to the storage service. Additionally, access to these files would be controlled through secure URLs that expire after a certain period of time. This way, even if a URL is intercepted, it would not be usable after its expiration.

To handle the implications of large media files on the system's performance, we can implement media file compression and chunking. Compression would reduce the file size, while chunking would allow the file to be uploaded and downloaded in smaller pieces, reducing the impact on the network and the storage service.

Finally, to further enhance the user experience, we can implement features like automatic image thumbnail generation and media file compression to reduce the amount of data transferred between clients and the storage service." ^A9y3nK8Y

"To support group chat functionality with multiple users, we can extend our existing chat application design by introducing the concept of Chat Rooms. A Chat Room is a virtual space where multiple users can send and receive messages simultaneously. Each Chat Room can be identified by a unique ID and contain a list of its members (users).

When a user creates a group chat, a new Chat Room is created, and the creator is added as a member. Other users can be added or removed as members by the chat administrator(s). The relationships between users and Chat Rooms can be stored in a separate table in the database, with columns like user_id, chat_room_id, and role (admin or member).

When a user sends a message to a group chat, the Business Logic Layer processes the message, associates it with the corresponding Chat Room, and stores it in the Database. The API Layer then retrieves the list of members in the Chat Room and sends the message to all members using the Notification Service. This ensures that all users in the group chat receive the message in real-time.

To handle message delivery efficiently, especially in large groups where a single message could trigger thousands of notifications, we can use a Publish-Subscribe pattern. In this pattern, the Chat Room acts as a publisher and the users act as subscribers. When a new message is published to the Chat Room, it is automatically delivered to all subscribers (users in the group chat), reducing the need for manual message distribution.

For offline users, we could store undelivered messages in a queue associated with each user. When the user comes back online, we can then deliver the messages from the queue. This ensures that offline users do not miss any messages.

To ensure message ordering, we can timestamp each message when it's sent and then order the messages based on their timestamps when displaying them. This ensures that all users see messages in the same order, regardless of any network latency or other factors." ^YO3pERE3

"To ensure messages are delivered even if the recipient is offline, we can implement a server-side message queue for each user. When a message is sent, it will be stored in the sender's queue on the server. The Business Logic Layer will then check if the recipient is online. If the recipient is offline, the message will be stored in the recipient's queue until they come online. Once the recipient is online, the server will push the queued messages to the recipient's device, updating their chat history.

In addition to the message queue, we can use a Notification Service to send push notifications to the recipient's device when they receive new messages. This way, the user will be informed of new messages even if they were offline when the messages were sent. The push notifications can also include a payload containing the message content, which can be used to update the chat history on the recipient's device without requiring a separate request to the server.

To address potential server overload from storing too many undelivered messages, we could implement a message expiry system where undelivered messages are deleted after a certain period of time. This would also handle the situation where the recipient does not come online for a long time. However, the expiry time should be sufficiently long to give the recipient a reasonable chance to receive the message.

To ensure the order of messages is maintained, each message could be timestamped when it is sent. The server can then deliver the messages in the order of their timestamps. This would ensure that the recipient sees the messages in the order they were sent, even if they were offline when the messages were sent.

By combining server-side message queues with push notifications, we can ensure reliable message delivery even when users are offline, while also providing a seamless user experience when they come back online. The additional strategies of message expiry and timestamping further enhance the reliability and user experience of the system." ^ZYRy41xW

To track the delivery status and read receipts of messages, you would need to store the status of each message and update it as the message is sent, delivered, and read. Consider how you would communicate these updates to the sender's device. ^tBYxHWKu

"To handle message delivery status and read receipts, we can use a combination of message metadata and real-time communication between the sender and receiver devices. First, we'll store the status of each message in the Database, which can have states like 'sent', 'delivered', and 'read'. This status will be updated as the message progresses through the system.

When a message is sent, the sender's device will send the message to the API Layer, which then communicates with the Business Logic Layer. The Business Logic Layer will process the message and store its initial status ('sent') in the Database. Once the message is delivered to the receiver's device, the receiver's device will send an acknowledgment back to the API Layer, which will update the message status to 'delivered' in the Database. Similarly, when the receiver opens and reads the message, their device will send another acknowledgment, updating the message status to 'read'.

To communicate these status updates back to the sender's device, we can use the Notification Service. The Notification Service will listen for changes in message status and send real-time updates to the sender's device. This way, the sender can see the updated message status (e.g., 'delivered' or 'read') in their User Interface.

In case of failure scenarios, such as the 'delivered' or 'read' acknowledgment failing to reach the API Layer, we can implement a retry mechanism. The receiver's device can attempt to resend the acknowledgment after a certain delay. If the acknowledgment still fails to send after several attempts, an error can be logged for further investigation.

To ensure scalability, we can use load balancing and horizontal scaling. Load balancing distributes network traffic across multiple servers to ensure no single server becomes overwhelmed. Horizontal scaling involves adding more machines to the system as demand increases, which can help handle a large number of messages being sent and received simultaneously.

If the receiver's device is offline when a message is sent, the message status can be set to 'pending'. Once the device comes back online, it can retrieve the 'pending' messages from the server and update their status accordingly. This approach ensures that we keep track of message delivery status and read receipts while maintaining real-time communication between sender and receiver devices. It also allows us to store the message status in the Database, enabling features like message history and synchronization across devices." ^f6huVgxk

"To ensure our chat application can scale to support the estimated number of users, we can employ a combination of horizontal scaling, vertical scaling, load balancing, data partitioning, and caching strategies. Different parts of the system may require different scaling strategies, and our approach should align with the estimated load and growth.

1. Horizontal Scaling: As the user base and message volume grow, we can add more servers to handle the increased load. Both the API Layer and the Data Access Layer can be horizontally scaled to accommodate more requests and data storage. For the NoSQL database (Cassandra), which is designed for horizontal scaling, we can add more nodes to the cluster to distribute data and handle more read and write operations. Cassandra is chosen for its ability to handle large amounts of data spread out across many commodity servers, providing high availability with no single point of failure.

2. Vertical Scaling: For the relational database (PostgreSQL), we can increase the capacity of existing machines, such as adding more CPU, memory, or storage. This can help handle more complex queries and larger datasets related to user profiles, contact lists, and group information. PostgreSQL is chosen for its robustness and capability to handle complex queries and relations, which are typical in user profile and contact list data.

3. Load Balancing: We can use load balancers to distribute incoming requests evenly across the API Layer and Data Access Layer servers. This ensures that no single server gets overwhelmed and maintains a smooth user experience. It also helps prevent single points of failure and mitigate network latency issues.

4. Data Partitioning: For the NoSQL database, we can employ partitioning techniques like consistent hashing to distribute chat messages and multimedia files across multiple nodes. This helps in managing large datasets and ensures quick data retrieval. This also helps to avoid potential bottlenecks related to database locks by spreading the data load.

5. Caching: We can introduce a caching layer between the Data Access Layer and the Database to store frequently accessed data, such as recent messages or active conversations. This reduces the load on the database and enhances performance, as data can be retrieved from the cache instead of querying the database. Tools like Redis or Memcached can be effectively used for this purpose.

In addition to these strategies, we can consider implementing a microservices architecture. This allows different functionalities of the system to scale independently based on their own demand, providing a more efficient use of resources. For example, the service handling user authentication might not need to scale at the same rate as the service handling message delivery.

By employing these strategies and continuously monitoring the performance of the system, we can ensure that our chat application can scale efficiently to handle the estimated load and growth, providing a seamless experience for users." ^d3GVeD5s

"A significant bottleneck in a chat application like WhatsApp or Messenger could be the handling of a large volume of messages and multimedia files, particularly in terms of database performance and network latency. This bottleneck can impact the user experience, causing slow message delivery, increased load times, and potential data loss.

To mitigate this bottleneck, we can employ the following strategies:

1. Data Partitioning: We can use partitioning techniques like consistent hashing to distribute chat messages and multimedia files across multiple nodes in our NoSQL database (Cassandra). This helps manage large datasets and ensures quick data retrieval, reducing the load on individual nodes.

2. Caching: Introducing a caching layer between the Data Access Layer and the Database can help store frequently accessed data, such as recent messages or active conversations. This reduces the load on the database and enhances performance, as data can be retrieved from the cache instead of querying the database. Tools like Redis or Memcached can be effectively used for this purpose.

3. Compression: To reduce network latency, we can compress messages and multimedia files before transmitting them. Compression algorithms like gzip or LZW can be used to minimize data size, reducing the time required to send and receive messages, and improving the user experience.

4. Load Balancing: Distributing incoming requests evenly across the API Layer and Data Access Layer servers using load balancers can help prevent any single server from getting overwhelmed, ensuring smooth user experience and efficient resource utilization.

5. Optimizing Database Queries: We can optimize database queries by using indexing, denormalization, and query optimization techniques. This will improve the efficiency of read and write operations, reducing the load on the database and improving overall performance.

By implementing these mitigation strategies, we can address the bottleneck in handling a large volume of messages and multimedia files, ensuring a seamless experience for users while maintaining the performance and reliability of the chat application." ^aof3NKB2

"In a chat application like WhatsApp or Messenger, two key security measures we would implement are:

1. End-to-End Encryption: To ensure the privacy and security of messages exchanged between users, we would implement end-to-end encryption using a protocol like the Signal Protocol. This means that messages are encrypted on the sender's device and can only be decrypted on the receiver's device. This way, even if an attacker gains access to the messages while they are being transmitted or stored on our servers, they would be unable to read the contents. The encryption keys are only known to the sender and receiver, and not even the chat application provider has access to them. This provides a strong level of protection against eavesdropping and unauthorized access to user data.

2. Secure User Authentication and Access Control: To protect user accounts and data, we would implement a secure user authentication system, such as using multi-factor authentication (MFA) or passwordless authentication methods. This ensures that only the legitimate owner of an account can access it, reducing the risk of unauthorized access. Additionally, we would enforce proper access control policies, such as role-based access control (RBAC) and least privilege, to ensure that users and system components can only access the data and resources necessary for their roles. This helps to minimize the potential impact of a security breach and protect user data from unauthorized access." ^EsFuDVkb

"To effectively monitor the chat application, I would focus on three main categories of metrics: system performance, error rates, and user behavior.

For system performance, the key metric would be message throughput. Application logs would record the number of messages sent and received per unit of time. Real-time analytics tools, such as Prometheus, would be used to track message throughput and generate alerts if it falls below a predefined threshold. This would enable the team to proactively identify and resolve any bottlenecks or performance issues.

Monitoring error rates involves tracking the number of failed requests, such as message delivery failures or database errors, and comparing them to the total number of requests. Tools like Grafana or ELK Stack would be used for this purpose. Alerts would be set up for high error rates, which would be integrated into the team's workflow through email or Slack notifications, allowing for quick issue identification and resolution.

In terms of user behavior, I would monitor metrics such as active users, average session length, and new sign-ups. These metrics can help identify any changes in user behavior that may be linked to system performance or usability issues.

To handle potential performance degradation, I would implement auto-scaling or load balancing strategies. This would ensure that the system can handle high traffic loads without compromising on performance.

Finally, regular health checks would be conducted to ensure the system's components, such as the database, caching system, and servers, are functioning optimally. A comprehensive dashboard would be created to provide a quick overview of all these metrics, enabling the team to monitor the system's performance and health effectively." ^eZnRZdRO

"To ensure functionality and reliability in a chat application like WhatsApp or Messenger, I would focus on testing the following key aspects of the system:

1. Message sending and receiving: Unit testing should be performed on individual functions responsible for sending and receiving messages, including text, images, and other multimedia files. This will ensure that the core features of the chat application are working correctly.

2. Integration testing: Since the system involves components like the User Interface, API Layer, Business Logic Layer, Data Access Layer, and Database, integration testing should be conducted to verify that these components interact seamlessly and produce the expected results, such as user authentication, message delivery, and group chat management.

3. System testing: To validate the overall functionality of the chat application, end-to-end system testing should be performed, simulating real user scenarios and ensuring that all requirements are met, including message encryption, notifications, and contact management.

4. Load testing: To assess the system's reliability under heavy traffic, load testing should be conducted. This will help identify bottlenecks and confirm that the system can handle a large number of concurrent users and messages without performance degradation.

5. Stress testing: To check the system's behavior under extreme conditions, stress testing should be performed. This will reveal the system's breaking points and allow for improvements in its resilience, such as handling network failures, server outages, or database issues.

Lastly, implementing automated testing using a CI/CD pipeline will enable regular checks, faster deployment, and early detection of issues, contributing to the overall functionality and reliability of the chat application." ^T9WWoZAC

%%
## Drawing
```compressed-json
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
```
%%