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
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

pWvKsyBAAA==
```
%%