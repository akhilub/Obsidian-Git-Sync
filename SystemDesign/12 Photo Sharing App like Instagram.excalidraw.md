---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data
## Text Elements
Photo Sharing App like Instagram ^jyhQujN4

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

Image Storage ^he4vk6Vs

Feed Generation ^u5so9ilo

Offline Delivery ^HQow2IZ4

How will your system store and retrieve the vast amount of images 
that Instagram users upload?
 ^5zcucAVg

Consider the scale of Instagram and the need for efficient storage and retrieval of images. You might want to think about different storage systems and the trade-offs between them. ^vOnhhI0K

How will your system generate and display the feed for each user? ^sM0i1Jkk

Consider the need for real-time updates, personalized content, and efficient retrieval of posts. You might want to think about caching, pre-computation, and other strategies to optimize feed generation. ^IEfaQTFg

How will your system handle user interactions such as 
likes, comments, and follows? ^V7t8P34f

Consider the need for real-time updates, consistency, and the scale of Instagram. You might want to think about different data models and the trade-offs between them. ^ZKoZktcu

How would you ensure the system can scale to support the number of 
users you estimated in the back-of-the-envelope estimation? ^oGWxm7yi

Tips ^nNRgkcRL

Can you identify a significant bottleneck in the system and explain 
how you would mitigate it? ^QJm93HAL

Tips ^7nWiTErU

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

Image Storage ^R3HeMv2F

Feed Generation ^jmC13PiG

User Interaction ^W7PUpw57

Sacling ^Ia9jtYZb

BottleNecks ^M9VK2mBv

Security ^AwlkWFJC

Monitoring ^g94KT2N1

Testing ^xsv0EhUL

A photo and video sharing social networking service like Instagram ^fki15RqO

- Users should be able to upload, view, and delete their photos and videos.
- Users should be able to follow other users and view their content in a feed.
- Users should be able to like and comment on photos and videos.
- Users should be able to search for other users, hashtags, and locations. ^xZnZ7Tf3

- The system should scale to support millions of users and billions of photos and videos.
- The system should ensure high availability and low latency for a seamless user experience.
- The system should ensure data privacy and security, especially for user profiles and content.
- The system should support efficient data retrieval for quick content loading and search." ^vuSFV1Ob

- Expected users per day: 100 million ^Sp3Gb2Cx

- User data: Considering each user's profile data to be around 1KB, for 100 million users, it would be 100GB.
- Transactional data: Assuming each user shares 2 photos/videos per day, with each photo being 2MB and video being 50MB on average. For 100 million users, this would be (2MB * 2 * 100 million) for photos and (50MB * 2 * 100 million) for videos, totaling around 10.4PB per day.
- Log data: Assuming each user generates around 50KB of log data per day, for 100 million users, it would be around 5TB per day.

Additional Features:
- Comments: Assuming each user makes 5 comments per day, with each comment being 1KB, for 100 million users, this would be 500GB per day.
- Likes: Assuming each user likes 10 items per day, with each like being 1KB, for 100 million users, this would be 1TB per day.
- Direct Messages: Assuming each user sends 5 messages per day, with each message being 1KB, for 100 million users, this would be 500GB per day.

Redundancy and Backups:
- Assuming a redundancy factor of 2, the total storage requirement would be doubled. This would also allow for backups.

Growth Over Time:
- Assuming a user growth rate of 1% per month, the storage requirements would grow accordingly. This would need to be factored into the system's scalability plans." ^o2z5kash

I would place the emphasis on availability for a photo and video sharing social networking service like Instagram. The reasoning behind this choice is that in the context of Instagram, availability is more crucial than strict consistency. Users expect to be able to access their data (photos, videos, comments, likes, etc.) at any time from anywhere, and a temporary inconsistency in the system (like seeing slightly outdated likes or comments) is generally acceptable. This is not to say that consistency isn't important, but in a system like Instagram, slight inconsistencies are tolerable, while unavailability of the service can significantly impact user experience and satisfaction.

Given the constraints of the CAP theorem, which states that a distributed system can only guarantee two out of the three properties: Consistency, Availability, and Partition Tolerance, we must choose two. Partition tolerance is a must-have for any distributed system to handle network failures, so the choice comes down to consistency and availability. In the case of a photo and video sharing service like Instagram, users value the ability to access and share their content more than they value perfect consistency, hence the emphasis on availability.

However, it's important to note that this doesn't mean we completely ignore consistency. We would still strive for eventual consistency, where the system may allow for some temporary inconsistencies but will eventually become consistent." ^Jb5VfL0Q

Answer coming soon. ^NtOEstpm

"Thank you for the feedback. Here is the updated list of API endpoints for a photo and video sharing social networking service like Instagram, including additional features and more comprehensive error handling:

1. GET /users/{id}
- Request: User ID
- Response: User's profile details
- Purpose: To retrieve a user's profile details
- Error: 404 if the user ID does not exist

2. POST /users
- Request: User's details
- Response: The created user's ID
- Purpose: To create a new user
- Error: 400 if the user's details are not valid

3. PUT /users/{id}
- Request: User ID, updated user's details
- Response: Updated user's profile
- Purpose: To update a user's profile
- Error: 404 if the user ID does not exist, 400 if the updated details are not valid

4. DELETE /users/{id}
- Request: User ID
- Response: Confirmation of deletion
- Purpose: To delete a user's profile
- Error: 404 if the user ID does not exist, 403 if the user is not authorized to delete the profile

5. POST /users/{id}/follow
- Request: User ID, target user ID
- Response: Confirmation of follow
- Purpose: To follow another user
- Error: 404 if either user ID does not exist

6. DELETE /users/{id}/follow
- Request: User ID, target user ID
- Response: Confirmation of unfollow
- Purpose: To unfollow another user
- Error: 404 if either user ID does not exist

7. GET /users/{id}/feed
- Request: User ID
- Response: List of posts from followed users
- Purpose: To retrieve a user's feed
- Error: 404 if the user ID does not exist

8. POST /users/{id}/photos
- Request: User ID, photo data
- Response: The created photo's ID
- Purpose: To upload a new photo
- Error: 404 if the user ID does not exist, 400 if the photo data is not valid

9. GET /users/{id}/photos
- Request: User ID
- Response: List of user's photos
- Purpose: To retrieve a user's photos
- Error: 404 if the user ID does not exist

10. DELETE /photos/{id}
- Request: Photo ID
- Response: Confirmation of deletion
- Purpose: To delete a photo
- Error: 404 if the photo ID does not exist, 403 if the user is not authorized to delete the photo

11. POST /photos/{id}/likes
- Request: Photo ID, user ID
- Response: Confirmation of like
- Purpose: To like a photo
- Error: 404 if the photo ID or user ID does not exist

12. DELETE /photos/{id}/likes/{userId}
- Request: Photo ID, user ID
- Response: Confirmation of unlike
- Purpose: To unlike a photo
- Error: 404 if the photo ID or user ID does not exist or if the user has not liked the photo

13. POST /photos/{id}/comments
- Request: Photo ID, user ID, comment text
- Response: The created comment's ID
- Purpose: To comment on a photo
- Error: 404 if the photo ID or user ID does not exist, 400 if the comment text is not valid

14. DELETE /comments/{id}
- Request: Comment ID
- Response: Confirmation of deletion
- Purpose: To delete a comment
- Error: 404 if the comment ID does not exist, 403 if the user is not authorized to delete the comment

15. GET /search/users?q={query}
- Request: Search query
- Response: List of matching users
- Purpose: To search for users
- Error: 400 if the query is not valid

16. GET /search/photos?q={query}
- Request: Search query
- Response: List of matching photos
- Purpose: To search for photos
- Error: 400 if the query is not valid

17. GET /trending
- Request: None
- Response: List of trending posts
- Purpose: To retrieve trending posts
- Error: None" ^jyXI9tSB

Incorporating the feedback, the entities for a photo and video sharing social networking service like Instagram would be Users, Profiles, Posts (Photos and Videos), Comments, Likes, Followers, Tags, and Direct Messages. Each of these entities would correspond to a table in a relational database.

The 'Users' table would have attributes like UserID (primary key), Email, and Password. This table would store the authentication data of users.

The 'Profiles' table would include attributes such as UserID (foreign key referencing Users), Username, Bio, and ProfilePicture. This table would store the profile information of users separately from their authentication data.

The 'Posts' table would include attributes such as PostID (primary key), UserID (foreign key referencing Users), Caption, Location, and Timestamp. This table would store the metadata of the photos and videos, while the actual files would be stored in a distributed file storage system like Google Cloud Storage or AWS S3.

The 'Comments' table would have CommentID (primary key), PostID (foreign key referencing Posts), UserID (foreign key referencing Users), CommentText, and Timestamp.

The 'Likes' table would be a junction table with UserID (foreign key referencing Users) and PostID (foreign key referencing Posts) as composite primary key. This table would help track which users have liked which posts.

The 'Followers' table would also be a junction table with FollowerID and FolloweeID (both foreign keys referencing Users) as composite primary key. This table would help keep track of the follower-followee relationships between users.

The 'Tags' table would have TagID (primary key), PostID (foreign key referencing Posts), and TagText. This table would represent hashtags associated with posts.

The 'Direct Messages' table would have MessageID (primary key), SenderID and ReceiverID (both foreign keys referencing Users), MessageText, and Timestamp. This table would store private messages between users.

This database schema is designed to minimize data redundancy and ensure data integrity, while also enabling efficient queries for common use cases such as retrieving a user's posts, comments, likes, followers, tags, and direct messages. It also considers corner cases such as a user liking a post or following another user more than once by using composite primary keys in the 'Likes' and 'Followers' tables." ^7Kl0zVoW

Delivery Receipts ^FFZrqZxn

"To handle the vast amount of images that Instagram users upload, we need to consider both the storage and retrieval aspects.

For storage, we can use a Distributed File System (DFS) like Google's Cloud Storage or Amazon's S3. These systems are designed to handle petabytes of data and are highly reliable, ensuring data is not lost even if individual nodes fail. They also automatically replicate data across multiple nodes and regions, providing redundancy and safeguarding against data loss.

However, storing images is only part of the problem. Given the scale of Instagram, we also need to consider how to efficiently retrieve images. For this, we can use a Content Delivery Network (CDN) to cache images closer to the users. CDNs have edge servers located all around the world, which can serve images to users from the nearest geographical location, reducing latency and improving user experience.

Furthermore, to optimise storage and retrieval, we can also consider compressing images and serving different image resolutions based on the user's device and network conditions. For example, we can serve lower resolution images to users on mobile networks to reduce data usage and improve load times.

Lastly, we should also consider the metadata associated with each image, such as the uploader's ID, the time of upload, and any associated tags. This metadata can be stored in a separate database, like a NoSQL database, which can handle large amounts of unstructured data and provide fast queries.

In summary, a combination of a distributed file system for storage, a content delivery network for efficient retrieval, image compression and resolution adaptation, and a NoSQL database for metadata storage would provide a robust and efficient system for handling the vast amount of images on Instagram." ^A9y3nK8Y

"The feed generation system will be a complex process that involves several steps and strategies to ensure real-time updates, personalized content, and efficient retrieval of posts.

First, let's discuss the real-time updates. In a push-based model, the server pushes new posts to the user's feed as soon as they are available. This is typically implemented using websockets or long-polling techniques. However, this approach might not scale well for a large number of users and posts due to the increased server load. On the other hand, in a pull-based model, the client requests new posts from the server at regular intervals. This approach can scale better but might cause delays in displaying new posts. Therefore, we can consider using a hybrid approach where we use the push-based model for online users and the pull-based model for offline users.

Second, for personalized content, our system will use a recommendation engine that ranks posts based on the user's interests, interactions, and connections. The recommendation engine can use machine learning algorithms to learn from the user's behavior. Now, to balance relevance with timeliness, we can assign different weights to these factors in our ranking algorithm. For instance, we can give more weight to recent posts but also consider the relevance score calculated based on the user's interests and interactions.

Third, for efficient retrieval of posts, we can use a combination of caching, pre-computation, and database indexing. Caching can store frequently accessed posts in memory to reduce the load on the database. Pre-computation can generate the feed in advance during off-peak hours and store it in a separate database table or cache. However, to handle the potential for stale data, we can use strategies like cache invalidation or expiration. For instance, we can invalidate the cache when a new post is created or update the pre-computed feed at regular intervals. Database indexing can speed up the query performance by creating an index on the columns that are frequently queried, such as the user_id and timestamp of posts.

Additionally, to further optimize feed generation, we can use pagination to limit the number of posts retrieved in a single request. We can also use content delivery networks (CDNs) to distribute the load and deliver posts faster to users in different geographical locations.

Finally, we should also consider the potential challenges and trade-offs in our design. For example, the push-based model might increase the server load, while the pull-based model might cause delays in displaying new posts. The recommendation engine might also introduce complexity in our system and require additional resources for machine learning. Therefore, we need to carefully balance these trade-offs to ensure the scalability and performance of our system." ^YO3pERE3

"Handling user interactions such as likes, comments, and follows will require a combination of different data models and architectures to ensure real-time updates, consistency, and scalability.

For instance, we could use a graph database like Neo4j for storing and querying relationships between users and posts. This would allow us to efficiently retrieve the list of users who liked a post or the followers of a user. A user's feed could be generated by traversing this graph to find posts from users they follow.

To handle write-heavy workloads in the graph database, we can use a write-behind caching strategy. This involves using a cache to absorb the write operations and then writing them to the database in batches. This approach can significantly reduce the write load on the database and improve performance.

For storing the actual likes, comments, and follows, a document database like MongoDB would be a good fit as it allows for flexible schemas and fast writes. Each document could represent a user interaction, with fields for the user ID, post ID, and interaction type (like, comment, or follow).

To ensure real-time updates, we could use a pub-sub messaging system like Kafka. Whenever a user interaction occurs, a message would be published to a topic. Subscribers to this topic, such as the service responsible for updating the user's feed, would then process the message and update their data accordingly.

In terms of consistency, we would need to implement strategies to handle situations where a user likes a post but the like doesn't immediately show up in their feed due to network latency or other issues. One approach could be to use eventual consistency, where we accept that the system might be temporarily inconsistent but will eventually reach a consistent state. For the user experience, this would mean that they might not see their like or comment immediately, but it would show up after some time.

Finally, to handle the scale of Instagram, we would need to use techniques like sharding and replication to distribute the load across multiple servers. Sharding involves splitting the data into smaller chunks, or 'shards', and distributing them across multiple servers. This allows for horizontal scaling as the system can handle more load by simply adding more servers. Replication, on the other hand, involves creating copies of the data on multiple servers. This increases the system's fault-tolerance as a failure in one server wouldn't cause the entire system to go down. In the context of user interactions, if a server handling the likes for a particular post goes down, the system can still serve the likes from a replica server." ^ZYRy41xW

"To ensure the system can scale to support the number of users estimated in the back-of-the-envelope calculation, I would use a combination of horizontal scaling, vertical scaling, load balancing, data partitioning, caching, and the use of a Content Delivery Network (CDN).

For horizontal scaling, I would add more servers to distribute the load. This approach is suitable for a photo and video sharing service like Instagram, where the load is expected to increase significantly over time. Horizontal scaling allows us to easily add more servers as the user base grows.

In addition to horizontal scaling, I would also consider vertical scaling for certain parts of the system that might require more computational power or storage. For instance, the database server could be vertically scaled to handle more simultaneous connections or store larger amounts of data.

Load balancing would be used to distribute the workload evenly among servers, ensuring that no single server becomes a bottleneck. This could be achieved using a round-robin algorithm or more sophisticated methods based on the current load of each server.

Data partitioning would be used to manage large databases more efficiently. We could partition the data based on user ID, for instance, allowing us to quickly locate a user's photos and videos. This would also make it easier to scale the database horizontally, as we could add more partitions as the amount of data grows.

To handle the massive write operations that a system like Instagram would have to deal with, I would consider using techniques like write-behind caching and batch processing. This would help to mitigate the impact of large numbers of write operations.

I would also use caching to enhance performance. Frequently accessed data, such as popular photos or videos, could be cached to reduce the load on the database and improve response times. We could use a distributed cache like Memcached or Redis for this purpose.

To further improve user experience and reduce latency, I would use a CDN. This would allow us to serve content to users from the nearest geographical location, reducing the time it takes for data to travel from the server to the user.

I would also consider implementing a Microservices architecture for different functionalities of Instagram. This would allow for better fault isolation and independent scaling. Each microservice could be scaled independently based on its specific load and requirements.

All of these strategies would be aligned with the estimated load and growth of the system. Furthermore, I would monitor the system continuously to identify potential bottlenecks and adjust the scaling strategy as necessary. By using these techniques, I believe we can build a system that can scale effectively to support the anticipated number of users." ^d3GVeD5s

"One potential bottleneck is the high read and write load on the database due to the large volume of photos and videos being shared and viewed by users. This could slow down the system and degrade the user experience.

To mitigate this, I would use a combination of strategies. Firstly, I would implement sharding to distribute the database load across multiple servers. This would reduce the load on any single server and increase the overall capacity of the database. Each shard would hold a subset of the data, reducing the amount of data each server needs to handle and speeding up queries.

Secondly, I would use a Content Delivery Network (CDN) to cache frequently accessed photos and videos closer to the users. This would decrease the load on the database as the CDN would serve the cached content, reducing the number of read requests the database needs to handle. It would also improve the user experience by reducing the latency of content delivery.

Finally, I would optimize the database queries to ensure they are as efficient as possible. This could involve using indexes to speed up searches, or denormalizing the data to reduce the number of joins required.

These strategies would help to mitigate the database bottleneck, but they would also have effects on the rest of the system. For example, sharding would increase the complexity of the system and could lead to consistency issues if not managed properly. The use of a CDN would require careful management of cache invalidation to ensure users always see the most up-to-date content. And optimizing the database queries would require ongoing maintenance as the data and usage patterns evolve. Therefore, these strategies would need to be implemented and managed carefully to ensure they do not introduce new problems." ^aof3NKB2

"The two key security measures that I would implement in a photo and video sharing social networking service are encryption and secure access controls.

Firstly, encryption is a crucial security measure for protecting user data. All user data, including photos, videos, and personal information, should be encrypted both at rest and in transit. At rest encryption ensures that data stored on our servers is unreadable without the proper decryption keys. I would consider using AES (Advanced Encryption Standard) for this, as it is a widely recognized and robust encryption standard. In transit encryption, often achieved through protocols such as SSL/TLS, ensures that data is secure as it travels over the network from the user’s device to our servers and vice versa. However, implementing encryption could potentially impact performance due to the computational overhead of encrypting and decrypting data. This is a trade-off that needs to be considered.

Secondly, secure access controls are necessary to ensure that only authorized users have access to specific resources. This can be achieved through authentication and authorization mechanisms. Authentication verifies the identity of a user, typically through a username and password combination. Passwords should be stored securely, using a method such as hashing and salting. This way, even if our password database was compromised, the attacker would not have direct access to user passwords. Authorization determines what resources an authenticated user can access. For instance, a user should only be able to edit or delete their own photos and videos, and not those of other users. However, implementing secure access controls could impact user experience if not done carefully. Users should not find it overly complex or cumbersome to access their own data.

Both encryption and secure access controls are fundamental to maintaining the integrity and confidentiality of user data. They will help to build user trust in our system, which is particularly important for a social networking service where users share personal and often sensitive content." ^EsFuDVkb

"I would monitor the system for performance and issues by focusing on two key performance metrics: response time and error rates. These metrics are particularly relevant to a photo and video sharing social networking service like Instagram, as users expect fast and reliable service when uploading or viewing content.

For response time, I would use a monitoring tool like New Relic or Datadog to track the time it takes for a request to be processed and a response to be sent. This would allow us to identify any latency issues that could be affecting user experience. I would set up real-time analytics and alerts to notify us if response times exceed a certain threshold, indicating a potential issue.

For error rates, I would set up logging and error tracking with a tool like Sentry. This would allow us to track any errors that occur in the system, whether they are user-facing or not. We would monitor for increases in error rates, which could indicate a bug or issue in the system. I would also set up alerts to notify us of any sudden spikes in error rates.

Additionally, I'd monitor system resources such as CPU usage, memory usage, and disk I/O using tools like Prometheus or Nagios. High usage of these resources could indicate a bottleneck or inefficiency in the system that needs to be addressed. Similarly, monitoring the performance of the database is crucial for a data-intensive application like Instagram. Tools like Percona Monitoring and Management could be used for this purpose.

In addition to these real-time monitoring strategies, I would also conduct regular log reviews to identify any recurring issues or trends that might not be immediately apparent from the real-time data. This would help us proactively address potential issues before they significantly impact system performance or user experience.

The data gathered from these metrics would then be used to make system improvements. For instance, if we notice that response times are consistently high during certain times of the day, we might look into scaling our servers to handle increased load during those times. Similarly, if we see that error rates are high in a particular part of the system, we would focus our debugging efforts there. This approach ensures that our monitoring efforts directly contribute to improving system performance and reliability." ^eZnRZdRO

"In testing a photo and video sharing social networking service like Instagram, there are several key aspects of the system I would focus on.

Firstly, the image and video upload functionality is critical, as it is the primary feature of the service. For this, I would use unit testing to ensure individual functions related to file upload, processing, and storage are working correctly. This includes testing the file size limit, supported file types, and upload progress tracking.

Secondly, the system's ability to handle high user traffic and data load is vital to maintaining a good user experience. Load testing would be used here to simulate a high number of concurrent users or data uploads, ensuring the system can scale and perform under heavy load.

Thirdly, the interaction between different components such as user interface, database, and server is complex and needs to be tested for smooth operation. For this, integration testing would be used to check if different parts of the system work together correctly. This might involve testing the process of a user uploading a photo, the system storing it, and then displaying it on the user's profile.

Lastly, the system's response to failures is important for maintaining service reliability. For this, I would use fault injection testing to simulate different types of system failures and observe the system's response, ensuring the system can recover quickly and maintain service availability.

These testing strategies ensure the system's functionality and reliability by identifying and fixing bugs, validating system performance, ensuring smooth interaction between components, and preparing the system for potential failures." ^T9WWoZAC

Design a popular photo and video sharing plateform like Instagram.Consider feature like uploading photos or videos ,following other users,
liking and commenting on photos and delivering a feed of photos from the users that someone follows.It necessitates consideration for massive
data storage,real-time updates,scalability and efficient searching or sorting of posts.  ^MGZUVv2D

"I would use a combination of both relational and NoSQL databases for this system. The reason for this is because a photo and video sharing service like Instagram has a variety of data that needs to be stored and accessed in different ways.

For the user data, comments, and relationships between entities (such as followers and following), a relational database like PostgreSQL would be suitable. This is because these data types are structured and have clear relationships, which are well handled by relational databases. Using SQL queries, we can easily enforce data integrity and consistency, and perform complex queries to retrieve data based on these relationships.

On the other hand, for storing the actual multimedia content (photos and videos), a NoSQL database like Cassandra would be more appropriate. This is because multimedia content is unstructured and can be quite large, making it less suitable for a relational database. A NoSQL database has high write speed, which is important for a service like Instagram where users are constantly uploading new content. Furthermore, NoSQL databases are highly scalable and can handle the large amounts of data that a popular service like Instagram would generate.

In terms of mitigating potential trade-offs, we could use database sharding to distribute the data across multiple servers, improving the scalability and performance of our system. We could also use caching to reduce the latency of frequently accessed data. Finally, we could use a Content Delivery Network (CDN) to store and deliver the multimedia content to users quickly and efficiently, which would greatly improve the user experience." ^zdNRcLaA

To ensure data availability, I would leverage a combination of redundant storage and frequent backups. Redundant storage could be achieved by using a distributed file system such as Hadoop's HDFS or Google's Cloud Storage. These systems inherently provide redundancy and fault tolerance, ensuring that data is not lost even if a node fails. Backups would be performed frequently and stored in a geographically separate location to safeguard against data loss due to catastrophic events like natural disasters.

For data replication, I would employ a master-slave architecture. In this setup, the master node would handle write operations while the slave nodes would handle read operations. This would help balance the load and improve system performance. The choice between synchronous and asynchronous replication would depend on the system's requirements. If data consistency is paramount, synchronous replication would be the best choice as it ensures that all copies of the data are updated simultaneously. However, if the system can tolerate some level of data inconsistency in favor of lower latency, asynchronous replication would be more suitable.

In addition to replication, data partitioning or sharding is another important strategy. Sharding is a method of splitting and storing a single logical dataset in multiple databases. By doing so, the system can improve its performance, as each shard is smaller and faster to manage, and each database server has fewer rows to handle.

For data synchronization, I would use a consensus algorithm such as Paxos or Raft. These algorithms ensure that all nodes in the system agree on the value of the data, thereby ensuring data consistency across the system. In practice, consensus algorithms work by a majority of nodes in a system agreeing on a value before it is committed. This prevents issues like split-brain scenarios where different parts of the system have different views of the data.

However, these algorithms can be complex and require careful implementation. The trade-off is that while they ensure data consistency, they may increase latency in the system and reduce availability during network partitions.

In all these decisions, I would carefully consider the system's specific needs and constraints. For instance, if the system needs to prioritize availability over consistency (as per the CAP theorem), the design choices would skew towards strategies that maximize availability, even if it means potential data inconsistency. On the other hand, if consistency is paramount, the design would prioritize data synchronization and synchronous replication, even at the cost of reduced availability." ^QqnU8Alo

%%
## Drawing
```compressed-json
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
```
%%