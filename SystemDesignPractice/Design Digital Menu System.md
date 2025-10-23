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

distribute incoming traffic evenly
- rate limiting 
- Load Balancers  ^43pkxjcF

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

%%
## Drawing
```compressed-json
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
```
%%