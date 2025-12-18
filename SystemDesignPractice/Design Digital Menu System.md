---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
Design Digital Menu ^DW3IadcH

Questions:

What is the difference between CMS & CDN

Can you cover some aspects around authentication and security?

What is the difference between availability and scalability

How is availability handled in for microservices?

What if the entire node goes down? ^ZwPZPXt6

Availability ^SLG468As

Scalability ^k421MpLY

How is availability handled in for microservices? ^PkFK4shV

1. Availability refers to the system being operational and accessible when needed,
even if parts of it fail. ^VtJQiADb

1. Scalability refers to the system's availability to handle increasing workload by adding
resources ^Lg5zJeQ0

For microservices, availability can be enhanced through redundancy, replication and failover mechanism.

Horizontal Scaling (adding more instances) helps with scalability and also contributes to availability by
distributing the load and allowing for some instances to fail without bringing the entire service down ^YZiNnJG8

2. Availability refers to the system's uptime and ability to remain operational

For microservices high availability is achieved through 

redundancy (running multiple
instance across different nodes or availability zones to prevent a SPOF), 

replication and automatic failover mechanism, so if one instance or even an entire
node goes down, others can take over ^pDC7PXQr

2. Scalability ensures the system can handle increased load by adding resources.

For microservices, this typically means horizontal scaling, adding more instances of a service
to distribute the load ^BNf46HAx

3. Availability ensures the system remains operational despite failures, often through
redundancy and failover mechanism ^C9y71s6C

3. Scalability allows the system to handle increased load or demand by adding resources, either horizontally
(adding more instances) or vertically (increasing capacity of existing instances) ^xDBZWB2A

What does availability mean for microservices?

Availability for microservices means that even if an entire node goes down, other instances, on different nodes can
continue to serve requests ensuring the service remains accessible. ^5ZnX7SCx

For microservices, availability is handled through redundancy (multiple intances), laod balancing, failover mechanisms and
disaster recovery strategies across different nodes or data centers

If an entire node or even a data center goes down, redundant instances in other locations can take over to maintain service ^TTCPR3O1

For microservices, availability is typically achieved through redundancy and failover mechanism, such as 
deploying multiple instances across different availability zones and using load balancers to distribute traffic.

If an entire node goes down, other nodes or instance should automatically take over to maintain service availability ^DYpS1jRk

What if the entire node goes down ^zpLYqw3x

If an entire Kubernetes node goes down even with 10 instances of a "menu service", the availability of that service will be impacted if multiple
instances were running on that single failed node. However Kubernetes is designed to mitigate this

• Pod Rescheduling : The Kubernetes control plane will detect the node failure, mark it as "Not Ready" and automatically reschedule the affected pods
(the instances of your menu service that were on the failed node) to healthy nodes in the cluster

• Topology Spread Constraints : Best practices involve configuring Pod Topology Spread Constraints to ensure your 10 instances are distributed across
multiple nodes and even different availability zones from the outset. This prevents all instances of a critical service from being on a SPOF.

• Load Balancers : Traffic would be automatically routed away from the failed node and to the remaining healthy instances on other nodes by the load
balancer

So while there might be a brief interruption for the pods that were on the downed node, Kubernetes self-healing and distribution capabilities aim to restore
full service availability quickly by spinning up new instances elsewhere. Your overall availability % might dip temporarily but would recover.

 ^A6et8Nuz

In microservices, availability is handled through redundancy, replication and self-healing mechanisms.

If an entire Kubernetes node goes down, the control plane detects the failure, marks affected pods for deletion and
automatically recreates them on healthy nodes to ensure continous service operation. This also involves distributing
workloads across multiple nodes and across different availability zones/regions to prevent single point of failure (SPOF) ^WX0Jxwk9

Short Answer ^SVOzRhEr

Detailed Answer ^PXio0RPC

OAuth works by using access tokens rather than sharing user credentials directly.

Here's a breakdown of the typical Authorization Code flow

- The client (your application) redirects the user to an OAuth Authorization Server
- The user authenticates with the Authorization Server (e.g using their Google or Social Media Login)
- The  Authorization Server then redirects the user back to your application with a one-time authorization code
- Your application then exchange this code with the Authorization Server for an access token (and optionally a refresh token)
- This access token is then used to access protected resources on a Resource Server on behalf of the user, without ever storing
   the user's password in your application. ^TCO4Stqj

How OAuth provider (or Authorization Server) works? ^glQYNMTv

Detailed Answer

An OAuth provider (or Authorization Server) is the entity that authenticates the user and issue authorization grants, 
which are then exchanged for access tokens and often refresh tokens.

Here's how it generally works

1. Client request authorization
Our application (the client) redirects the user to the OAuth provider's login page

2.User authenticates:
The user logs into the OAuth provider using their credentials.

3. User grants permission:
The user is asked to grant our application permission to access certain resources on their behalf.

4. Authorization grant issued:
Upon approval, the OAuth provider sends an authorization grant (e.g an authorization code) back to our application

5.Client requests access token:
Our application exchanges this authorization grant with the OAuth provider for an access token (a JWT in our case) and a refresh token.
This exchange happens directly between our application's backend and the OAuth provider, not through the user's browser, for security

6.Accessing protected resources :
Our application the uses the access token to make requests to our API Gateway and backend services. The API Gateway validates this 
token before routing the request.

7. Refresh token:
When an access token expires the application can use the refresh token to obtain a new access token without requiring the user to
log in again ^oX6csKds

1. Define Scope ^VupcJeqQ

• Functional Requirements
    
    - Centralized Content Management : Content (menu items, prices, promotions, ads) must be managed per country from a central system
    - Location-Specific Menus : Support different menu offerings and pricing based on individual store locations
    - Display Versatility : Accommodate varying numbers and arrangements of in-store display screens
    - Scheduled Promotions : Enable scheduling of time and region-limited promotional pricing or sales
    - Visual Appeal & Advertising: Menu must be visually engaging and integrate first-party adverstising
    - Automated Meal Period Switching : Automatically display breakfast, lunch or dinner menus based on the time of day
    - Dynamic Highlighting : Highlight items based on dynamic conditions (e.g "bestsellers", "daily specials")

• Non-Functional Requirements
    
    - High Availability : Menus must be displayed continuously, even during network issues.
    - Global Scalability : Support thousands of store across multiple countries and regions
    - Eventual Data Consistency : Menu updates should propogate eventually across all relevant displays.
    - Performance: Fast delivery of menu content and responsive updates to displays
    - Security : Secure Content management and delivery
    - Maintainability : Easy Management and expansion of features
     ^z1yUI9Fh

2. High- Level Design

We'll use Microservices Architecture deployed on a Cloud Platform (GCP - Google Cloud Platform)

Core Components
• Content Management System (CMS) / Admin UI : Web Interface for administrators to manage all content
• Menu Service : Manage menu definitions, pricing, item details, and dynamic display rules.
• Store Service : Manages store metadata, location-specific configurations and display device mappings
• Promotion Service : Manages promotional campaigns, scheduling and regional applicability
• Content Delivery Service : Aggregates menu data, promotions and ad-content, generating display-ready data.
• Display Management Service : Monitors in-store display health, pushes content updates, and manages device configurations
• CDN (Content Delivery Network) : Stores and delivers static assets (images, videos) for menu and ads globally
• Display Client Application : Lightweight application running on each in-store display, responsible for fetching and rendering menus

Data Flow:

1. Admin creates/updates content via CMS.
2. CMS interacts with Menu, Store, and Promotion Service to persist data.
3. Content Delivery Service queries Menu, Store and Promotion Service to build a complete menu data package for a given store and time.
4. Static assets are pushed to the CDN.
5. Display Management Service orchestrates updates; it can either push notifications to Display Clients (e.g via WebSockets/MQTT) or Display
Clients periodically poll for updates.
6. Display Client Application fetches the latest menu data from the Content Delivery Service and static assets from the CDN, then renders the menu


API Endpoints

• POST /menu (Create an new menu)
• GET /menu/store/{storeId} (Retrieve the current menu for a specific store)
• GET /promotions/active?storeId={storeId}&time={timestamp} (Get active promotions)
• POST /displays/{displayId}/heartbeat (Display client sends heartbeat)
• PUT /displays/{displayId}/config (Update display configuration)
 ^DFtvm9A0

3. Technology and Data Design

• Backend Services : SpringBoot (Java) for microservices (Menu, Store, Promotion, Content Delivery, Display Management).
   Leverages its robustness and ecosystem.

• Databases :

    - PostgreSQL (Google Cloud SQL) : For transactional data like menu items, prices, store details, promotional schedules.
    Offer strong consistency and relational integrity
    
    - MongoDB (Google Cloud Firestore) : For flexible content storage, like advertising creatives or dynamic display metadata, 
    due to its schemaless nature and scalability.

• Caching : Redis (Google Cloud Memorystore for Redis) : For in-memory caching of frequently accessed menu data and pre-generated
menu layouts to improve read performance.

• Message Queue : Apache Kafka (Google Cloud Pub/Sub) : For asynchronous communication between services, especially for broadcasting
menu updates to the Display Management Service for efficient propogation to thousands of displays.

• Content Delivery Network (CDN): Google Cloud CDN : For globally distributed caching and rapid delivery of static menu images, videos,
and other assets to in-store displays.

• Container Orchestration : Kubernetes (GKE) : For deploying, managing and scaling microservices efficiently

• Display Client Application : A React-based Web Application running in a hardened browser on the display device or a native Android/IOS 
application for smart screens. This allows for rich, dynamic UI. ^eKMQaPBy

4. Low-Level Design

• Content Delivery Service Logic :
    - Receives requests with storeId, currentTimestamp, region.
    - Determines the appropriate "meal period" (breakfast, lunch, dinner) based on the store's timezone and pre-defined schedules.
    - Queries the Menu Service for base menu items applicable to the storeId and meal period.
    - Queries the Promotion Service to apply active, time-and region-specific promotions (price changes, special offers).
    - Applies dynamic highlighting rules (e.g items with high current inventory, special daily specials).
    - Constructs a composite JSON object representing the final menu structure, including URLs to CDN-hosted assests.
    - Caches the generated menu for a short duration (e.g 5-10 minutes) keyed by `storeId` and `mealPeriod` in Redis


• Display Client Application : 

    - On startup, registers with the Display Management Service.
    - Periodically long-polls or maintains a WebSocket connection with Display Management Service for updates.
    - Upon receiving an update notification, it fetches the latest menu JSON from the Content Delivery Service.
    - Fetches static assets (images, videos) from the CDN.
    - Uses its rendering engine (e.g React components) to display the menu visually.
    - Implements offline caching : stores the last successfully loaded menu locally to ensure continuous display in case of network interruption. ^wQzuUGwL

5. Object Model Design

• Menu Item (PostgreSQL):
    - itemId: UUID (Primary Key)
    - name : String
    - description : String
    - basePrice : DECIMAL
    - imageCDNUrl : String
    - category : String (e.g Breakfast, Sides, Drinks)
    - dietaryInfo : JSONB (e.g [Vegetarian, Gluten-Free]

• Store (PostgreSQL):
    - storeId : UUID (Primary Key)
    - name : String
    - countryCode : String
    - region : String
    - timeZone : String
    - menuOfferingId : UUID (FK to specific menu template)

• Promotion (PostgreSQL):
    - promotionId : UUID (PK)
    - name : String
    - description : String
    - startTime : Timestamp
    - endTime : Timestamp
    - discountType : ENUM ('PERCENTAGE', 'FIXED_AMOUNT', 'NEW_PRICE')
    - discountValue : DECIMAL
    - applicableItemIds : UUID [] (Array of Menu Item IDs)
    - applicableStoreIds : UUID [] (Array of Store IDs, or NULL for all)

• Display Device (PostgreSQL):
    - displayId : UUID (PK)
    - storeID : UUID (FK to store)
    - serialNumber : String
    - locationInStore : String (e.g Counter 1, Drive-thru)
    - lastHeartbeat : Timestamp
    - status : ENUM (ONLINE, OFFLINE, ERROR)
    - currentMenuVersion : String (Hash of the currently displayed menu JSON) ^1td6WhDU

6. Address NonFunctional Requirements

• High Availability:
    - Redundancy : All microservices are deployed with multiple instances across different availability zones.
    - Database Replication : PostgreSQL with primary-replica setup for failure.
    - CDN : Distributes content globally, reducing SPOF.
    - Display Client Offline Mode : Displays cache the last loaded menu, ensuring continuity if backend or network connectivity is lost

• Global Scalability : 
    - Microservices : Allow independent scaling of each service (horizontal scaling via GKE autoscaling)
    - Stateless Services : Designed to be stateless, making horizontal scaling straightforward.
    - Cloud-Native DataBases : PostGreSQL (Cloud SQL) can scale vertically, and MongoDB (Firestore) scales horizontally by design.
    - CDN : Handles massive concurrent content requests efficiently.
    - Kafka/Pub-Sub: Decouples services, buffering requests and handling spikes in load.

• Eventual Data Consistency :
    - Menu updates are propoaated asynchronously via Pub/Sub. When the CMS publishes an update, the Display Management System receives it
    and triggers relevant Display Management Service receives it and triggers relvant Display Client to fetch new content. This prioritizes availability
    and  performance over immediate, global strong consistency, which is acceptable for menu displays.

• Performance :
    - CDN : Serve static assets with low latency globally.
    - Redis Cache : Reduces database load and speeds up dynamic menu data retrieval.
    - Optimized Queries : DB queries are indexed and optimized.
    - Asynchronous Updates : Pub/Sub ensures content propagation doesn't block critical paths.

• Security :
    - API Gateway : All external requests pass through an API Gateway for centralized authentication (OAuth2/JWT) and authorization.
    - Data Encryption : Data encryption in transit (TLS/SSL) and at rest (disk encryption for databases).
    - Network Segmentation : Service deployed in private VPCs with strict firewall rules.
    - Input Validation : Robust validation on all API endpoints to prevent injection attacks.

 
 ^jZaWaJDu

Menu Service ^z2ByDsab

Store Service ^OnkS4K87

Promotion Service ^vRfqVjPr

Database ^GUorVmxV

Menu Items
Prices
Stores Details
Scheduled Promotiosn4 ^CmGJS2ZH

PostGresql ^3bU8YS4N

No SQL DATABASE  ^EaOA1Qa3

For Flexible content storage like advertising or
dynamic display metadata ^ZT3g72Od

Display Management Service ^fQdaCj1E

Cache ^knIBVXYP

Menu Item
- itemId
- name
- description
- price
- category : breakfast , sides, dinner
- dietary Info :  ^4h83C5k1

STore
- storeId 
- name
- countryCode
- region
- timeZone ^qhPO3LTe

Promotion
- promotionID
- name
-description
- startTime
- endTime
 ^zvlmubLu

Admin create
content in CMS ^TNhVFDbg

API Gateway ^bVwnbjw9


- rate limiting 
- Load Balancers : distribute incoming traffic evenly  ^43pkxjcF

OAuth Service Authentication 
Provider DB ^cegZ8RPd

OAuth2 Service ^iMsiD3x1

7. Additional Features (Evolution of Design)

   • Personalized Menus : Integrate with loyalty programs. When a customer scans a QR code or uses an app, the display could show personalized 
    recommendation or discounts. This would involve a Recommendation Engine Service and real-time user identification.

   •Interactive Displays : Implement touch-screen capabilities allowing customers to browse nutritional information, customize orders, or explore allergens.
    This would require enhanced Display Clients capabilities and potentially bidirectional communication with order management systems.

   • Real-time Analytics & A/B Testing : Collect telemetry from Display Clients (e.g popular items displayed, ads views) via Pub/Sub to a data warehouse (e.g BigQuery).
    This enables data-driven insights, A/B testing of menu layouts or promotions and dynamic menu optimization.

   • Dynamic Pricing : Implement real-time price adjustments based on factors like current demand, inventory levels or local events. This would require integration with
    inventory systems and Dynamic Pricing Service.  ^bP5A6e7V

Performance & Bottleneck

• Bottleneck: The primary bottleneck could be propogation of menu updates to thousands of display clients simultaneously.

    - Solution : Using Kafka/Pub-Sub decouples the updates initiation from the delivery mechanism. The Display Management Service
    can then efficiently manage sending notifications or triggering polls to the clients, leveraging client-side caching and eventual consistency.
    CDN reduces the load of actual content delivery on origin servers.

• Menu Generation Latency : For highly dynamic menus, the Content Delivery Service might become a bottleneck if its computing menus from
scratch on every request.

    - Solution : Redis Caching of pre-generated menu JSON for a short TTL significantly mitigates this by serving frequently requested menus
    directly from memory. ^HOTEr7B0

Trade-offs 

• Microservices vs Monolith:
Choosing microservices introduces operational complexity (deployment, monitoring, inter-service comm'n) but provides superior scalability, fault isolation
and independent development for a large, multinational system. This is a worthwhile trade-off here

• Eventual vs Strong Consistency : 
Opting for eventual consistency for menu updates allow for higher availability and better performance at scale. A slight delay in menu updates across all
displays is acceptable for this use case, making it a good trade-off. 
Strong consistency is reserved for critical configuration data within core services.

• Polling vs Push for Display Updates: 
Push-based updates (websockets/MQTT) offer real-time responsiveness but are more complex to manage at massive scale and can fail if devices are offline.
Polling is simpler but less real-time. A hybrid approach (e.g long-polling or WebSockets for critical, real-time updates and periodic polling for general status or 
background refresh) offers a good balance ^hfz8CPAp

System Assessment (Security , Monitoring & Testing)

• Security :
    - Regular Security Audits : Conduct periodic code reviews and penetration testing.
    - Dependency Scanning : Use tools to detect vulnerabilities in third-party libraries.
    - WAF (Web Application Firewall) : Protects API endpoints from common web exploits.
    -Secure Coding Practices

• Monitoring:
    - Metrics: Use Prometheus/Grafana or Google Cloud Monitoring to collect and visualize key metrics
    (CPU, memory, network, request latency, error rates for all services and display clients).

    - Distributed Testing : Implement tracing (e.g OpenTelemetry, Google Cloud Trace) to monitor request flow 
    across microservices and identify performances bottlenecks.

    - Centralized Logging : Aggregate logs from all services and display clients into a centralized system (e.g ELK stack,
    Google Cloud Logging) for analysis and debugging.

    - Alerting : Configure alerts for critical service degradation, display offline status, or unusual error rates.

• Testing :
    - Automated Testing : Comprehensive suite of unit, integration and end-to-end tests integrated into CI/CD pipelines.
    - Performance Testing : Regular load and stress testing to ensure the system handles peak traffic and identify breaking points.
    - Resilience Testing : Chaos engineering to test system behavior under failure conditions.
    - User Acceptance Testing (UAT) : Involve business stakeholders for validating new features and content workflows. ^PHA1IUpc

Client -> Load Balancer -> API Gateway -> Backend Services is a common and recommended architecture 
for achieving both high availability and robust API management.

In this setup, the load balancer ensures that requests are distributed to a healthy API gateway instance. 
The API gateway then processes the request and routes it to the appropriate backend service ^kggfY3NN

%%
## Drawing
```compressed-json
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=
```
%%