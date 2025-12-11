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
N4KAkARALgngDgUwgLgAQQQDwMYEMA2AlgCYBOuA7hADTgQBuCpAzoQPYB2KqATLZMzYBXUtiRoIACyhQ4zZAHoFAc0JRJQgEYA6bGwC2CgF7N6hbEcK4OCtptbErHALRY8RMpWdx8Q1TdIEfARcZgRmBShcZQUebQBGAA5tAAYaOiCEfQQOKGZuAG1wMFAwMogSbggAEQB1AGYASVxibAAJdLLIWEQqqCwoTvLMbmd6gE4AVmTEgBZxlJT4gHZp

gDYeZfr+cphRqfHtROWecfj6xMT4pdnZncgKEnVueIXtZeXxubXX+NmUjaTe5SBCEZTSbg8IHFSDWZTBbgpYHMKCkNgAawQAGE2Pg2KQqgBiBAperneJDSCaXDYdHKNFCDjEHF4gkSVHWZhwXCBXKUiAAM0I+HwAGVYAiJIIPPyUWjMbUnpJIcjURiEOKYJL0NLKsCGeCOOF8mh4sC2NzsGo9qbFsD6cI4I1iCbUAUALrAgXkbLO7gcIQi4GEJlY

Kq4NL64RMo3MV0BoMwiBhBDEbiJSZrWY8KEXYGMFjsLimub5pisTgAOU4Ykh43q/3Ga2WdyThGY1Uy/TTaAFBDCwM00eIAFFgtlcq6PcChHBiLhuy9lmsAYk1ol6jxs8tgUQOOj/YH8Lu2LTU9w+/gB0n+phBhJO6xlBxUNUwWoCKgALI5IT88gUAAKgMVSPmCL5vqoUT4N+v78gKnBQKKhBGOIvBIkmCG5AAYrg+jCjaqDQl00ADAAgkQz5VMEA

qDGWpBQOYBAUeBfQWvyei5LgIZMH6aAJseSb4u+HAEMBd6geE4Gvu+0GwQG/K4EIUBsAASlJqHcKiQgILuPFtKC4L3qg8TaFCxQAL47KU5SVBIABaFAAAr2U5AAaUBrPyPRoaREnAiMaD/ACCRTGSULAoRPD1No2bxFuDaTKcHwpNFwKPMQzxoGs9STGZryzIkW48EkqzApIhkQmgmzEeUcLahhJFyuqLL4kSJJkmS/LUrSDqMsyuJtey5AcFyPI

5HRmHCmKEq+bqaaqvKCCKplyrVYt6qatqya4nqSYGpIsaumaQmWtaLx2kmfVOi6hSephPoIHxqACcGoaBeguAUlG/VHYeiZNQg55oPWazjMs1xJPRFbFqg9QpIk0NFtWHC1qaPBXBMayTLcwYdl2wOoJe14kUO/VjlkE1TvdJGzvOi6msu1yTPUK71okjXlHuB78UeJ5nj2RP9rpN4gRIACKOkokW8gADocPLtSSAuqDtqg6gIKgjgCgKTA5GIqC

aAgUAUEDL5Yl+oqoAAZKgWLVJW8vy1i1ioDAwioHoBaoII2SoKEiDYHk/sMky/vKRVuRMYxnD+2HYTYCI1oAPxOxwSsq2rGta4QOt66jmtGybZv+/Q3H4LgmjCtacfED77iV9XsBp20bAUKrzCl+XjdELAqDK0ywR1yGRP4qg+HYGiYSkGYYjMKnCvp8rUCqwK6sVagE2EIEL1sMQmvKGw4Ra23HDJ/+lDicZECS+EMejcgacZyvWcb9ruu8gbRe

mzkduWzbdsHZpxdi+d2QhPZsG9r7TWAcEBB07jyYc4cNZRzwPfWuPs4FJ1gAvRWy8O7r01u/fOX9jY/xfLgMuwoe412sHXZgDcq69xgC3NuBDKHdyYTXAexAh6qxfAhUg49zBTyYLPcIuCl6ZzXtnLeO8OB7wPkfTuxBT7ny9IhZCmlqqc0gFhKAuF8L4EIrVbo5FKKwwgDRSaJECyMXcCxKi7J2LAk4lEHipBnqvSEqQESYkxboFvtLTgctF7Pw

IdnYhn9C5kJLhbK2tt7aO0XiAt2HsvZMB9gYGBXI4HB0Qf1ZBkd7ELiLBghO2CYCSPCa/IhucP76xicXX+HDqFcL7nQ+uBAaHN0Xq3duatWkV3aTAfudC+Ej0EcIyebBp7iPnk/fBudCGbyjvIxRqBD7H1URQM+illJqQ0mhbSIsSJ7gQAZMEVUTJmUmJZaybZBYQFFAAGQAOKzHXGRfIwIfJ9BAgFbgsx4rJHXKseIOMmyzEmDCyK3BMzJBzDwb

GgJ0pKgunEDM4wFgLEmEzCG4xyqVWMhFJM9U0K6OTGqTErU2ToEJK8CG2AeDdRpHSUOA1WR9BGmNXkNjyhChFFtOau0FpJmagqdF61xXUo1LNKo81/zCENMaF45ozqwAupS66zopwwkgAAfRgBQTQmhlC4DchwAAjvUZQ9BJj0AAEJOUSIQAAVoMGENMBWPS8XzNs71wxJCVb9VVvMAblBTILeIYNsZknGMipGnBuBkiTRwFGaNUBfChMCyYKRWw

kXbJ2YIDMhZXlOeUMmTIKYTjyHdGcc4FyExWD8FILMtis1JWckMPMXr+rOaeTEgtiYVrMRJCQZEqHDKbnsfUl8AkQEnZwmd8FNEoTQjwSl+jDEEXhb88xrEJDWP5HYpi+BHGWJUnADiiFuJGk8YLbxJFhL+H8eO9AS62kruBEpFS6lWDaPVqQHSekjSXKMi8W59zig2UgHZdA6I4pfjgC8gAmt5eAvlbz8sgB9PNbwcbLARi2PNxwdxJhMeuMyOY

UVdvKBlLKWblipE+GcVKywNybtTUmCqVySWmIgOSxEG0aWDTpRAQkPABTjAQLcVlvUOW0u5ZybkfL4LTWFQq0VspZUrUY3wGVS1NNSm0z9FVcY1WnRpOdW02qGQ3T1V0Q1xrTXmstTau1DrnWuo9RAL1XpfWPv7bZQNEgvpvJDTGMNfaI0CCBtGps0U22UoLDDIFJ1bHlmRjWNC1w5h4tyvmvGxbjaExHYOYcNaqb1qTHTJt0amZtvqB21FSZub/

UEgOgWF5hb7vfc8xh379rzv66KQbzDV25C0RurdiEd3GL3aLO8F7qIIFoiepgJTz0WLYte1xt6PF+tixAF9IY33XzG90kZ+y/1HK0sB0dVj9LEsg+ZMoVkYOPKqAANSgAAKXFoQMi1RNAYd6OyAFSYPrNbXNoFcMK1g5XhikFscLsqZm0JuTYfxxiFVJH8NFq1uCfBSAkV4FxZjLkzHRyAvGINoDzT+jg8IKUiexGJok8QEBc65/J9lw4lPDRU+N

PkXoNPypMzKNnem1q8DZ8ZnUpn9rKsOtFjL5QLTWc1bZ+09ndWFH1RAI1JqzUWutba+1TqXXus9V0b1ejAsdbevvD6gn4jfci8QP64bOuRvixdTcHMPg4zTZCCYaaM0zbOFuAE/xisE2Hb1pMVbRzjmq2gactXG2lpbczZr9RO0Cfaz7/mQ6evlr69fUyqBP3TuYagQIH9O4qRWcwGAKIsiG1BMz1AFomClM4J+TpNI56sE0MEVAFBI4vXi6mag8

sECMBfMs1Twc2BrzUELYU2gL5AQXdX2vPTRmN/LOrNgrf2/9H0F3kMyhe+IHIPfIfYcR/GkIOPzWU/f5GlTHPhfS/V5UBV9O519VYV4+xt9JskJ11IRZscI8Jd0GdK8Vsj01scMGBNsz0UD0Ar0b0uJDsgtjtTtRJ8Ar4qgD8p0j8G81tT8W9s428O9r8jZb979+8n8YJh9sBR938J8v8Xwf995iB58jQACV8eQ18N9wDy4d8f0Dl/0YC0ATlQML

kXtTQoN3sHlC0nkXllBJgjA/sEBxZIwbxMN/l/IocgUwYYpwVEhccY8UhsU0dUBsZSckoWYLh4gY181tgkwGNZcMxSdSQ7Djhrh6wyoeNVCiJKUhM0BKUJV2cuUJAGVucUi+c+omRBccCeVVMJp1MhUJdFcpdDN1QZcVRijMQFcdoiiSIDpvcTJ1UtdCJrg7NHR9cM9DdjdXMzcPNLdvMbc/M7cAs8InpCDfc4NQtPp4h8BPc6in0/dCYthjhkVg

UUsstk10YfDMtCwqwcsXhEc21VgYUDNC18YS0ysk9SZKs09JwataZs9m1Gt20C9Wtu19wnc2tB1ziK8lsq9tBUBLs68a4T8WAz8L9GCAByBBSgkZUEnhCfEMSeEIVgHvCgfEdEPEFoQ2UZFoRwZneWQIQQEQOeXfMgiQavAEqg4E5vc/egy/LISEruL9evFvOEzWBEwIUIFg1E0gdEtgTEzQbE4gXE5QfE8IYQUQcIKA6bWAjReAoxExZAnbVA9b

eiLbbA6AFxJMNxO9XiUY80XxV9Ug/fP4ikmEqk0E2kiEqE5dZk8/Vk/hREzklEtEjEuuAU/2IU2/UUwkiUn5MlOQu7RQh7ZQ8Da5UyN7MAD7MoWDCoJ5OAaoLEZYdycWAkX5UwiHcwkiaHDjZjDcBNYFBsIjJITY3YYnC4THUGV4DMHGFmY4+jKVVAEnI4CYSnVYCYAEGnEEPjSETsmI1AOI2VTIiTHnVIwcNldIzlIaLI4XNTMXfIrUEVaoyNXT

BsusgQWVSoxVMzVXCzU0Boq0bXEyS6EiHVW6dopzI3FzU3dzC3Lza3XzfzB6YYo7MYioCYwTFlH6KLXcmLV8qNF4FYGNZrP4NcjA7Y2GWYeGCPXY7KZsIqFmXGNsU40rRPb4y48ma4utDPe3CAOrHPR4/PQvUDXtOYyAPEbrXsC48obDKoOIGvaEmdagpvC0jeBgq/Bk2cRiP2YfGElvQIfQO9Vgx/IsAgNObCMeCeURGecwY+SQK5RkwEvuQZbA

OSxfVMdeA6VANOQIYgfqawbAUZAACmAw4A4BYP0EDEYh8AQHlhDBRH0pgRmTjBzjzj5V3n3hAKESGSoKME4GPhbzgECCXxXlwH+KcgAHlsIABKagLSxeQIHwaOMpYfA5AS+xLfPEb2bIFS6wdsfQWKwQQAvy/heyguXvIRNSihF8ORGy0yjZLZFRU+WKtgDWEEvAF8KITEXvAsEkhdOiw/M0mgkEug1iuk/QDiuALimBF/Xi8/fiwSvvYSwffAMS

iSkRWZMRGSzuOS8EBSqg5S1SxgOudQTS7S1MPS1GIykysynvCy/AKy4IWy0aKIMqmkKeFRepfOFeBRDy8qvamE3y2MUEwKyqkKsKyKmKuK0UxKtBZKl/VK0pbADKyBTJbKgePKgq8/ZZYquyl6g2MeSquOVZRiQIeWH6pRbZJq3vVqzudq9WXALqlG1MzCNdQDTdWUgxBAhbJAn49U49VUrApUnAzUkibUgg9459A0s7I0/rfqhi+vc0kazWNi+k

zuTiwgbimaxivirIBah/AfEg1aoRSSja6SuefueS7ymEg6wgNS46yQU6+K86pkfSq6xkG6u/O6h62q3Ghy/2Jyj61yiady4+MeK2xiwG/y8/EG4K/2cG6K2Ks6mGg2jBX9AwRG5GrKuBdG5gfKrJIqo0EqvGzWAmgA12Gqsm+q5RE+XZZqmmz2V2Tqkunq2Q27ADY5YMtrZ7bstQiMqMkoL7CQR1SsAUT5NoMiEYNM8HHAyHLMyw8GbQPNEqaYIj

cGKGCjMs5jFsK4W4BwrMGNNYQnRjWwvKcGfMjmT5Y4ATOnMM48uqZnBqNnIc5IkctIxTDnIXUaHI0XKaec7aLc8o5aVc+XAoqovaGolXOo9XSATXA8pou+yAU8xzEiTo6883TzK3HzW3MoHC70Z8vUgNF3cMHgD3L8r3aLUi5Mf3NAFsTcBYJIQ+pMVLIsSEV4aCguYnTdP4RYJsePM41CkmStK4ymG47ChtemB41tJ4oirut4kvD4iistQRsda+

Oi00xinIZgEQfy0axghul8e09kpE9S10rEj04U6gn0uebQI26ZKS+ZWK9QLOeAM9YxceEIUafuYSQGuSBhAgW/WKnE8y/ENk56hykAteUKuZGS+WFvRwOUd/ZSTWbOV03q2Wk08bGuTR7R5vXRq/fRsZQeUJx0sIOuUx90oJnvAk8U6x2xk26JueRxuS5vFx9wNx7ITkLx3xHxz8PxvcZQQJz026kJou8J3vSJzBM22quJ9sVERJ/oFZVJjm6UnR

Dm+bBU3moWqxNAjbBiQWw9YWvbLUg7e9F8/UvxGW1RjJq7DR0aHJsE/Jumwx1GDk0p1AcpwUix6pok8IGxxecS429ahp8IJp5xuAVx0ZDpzxyQbx29GCPpgJ8x4JneX2guCJuO4F2J8/eJuZzQJJxZvksVEiNO+QwDJQ2RlQnum5PuzQ2yJ5LEcYGACGZgNYLEMHLDWe4YFNDjNYMnPFU4escGMkAtUs20CFOHJKQrBFAvT4I+/wzYI4T4FslmeK

a+yIxnMlB+1nQB5+kc3nMchTAXD+6cr+kXdAwVGaBcrTJc9cpaUo6VJqDc0BgBiBvwHc46fcmzI8lo2cNot0Doq8tzdB3o+87BsAXBx3eRwtd83AeoGYih4LOLBYvFM4T4VHJhtY2GFYEsyAZhnYjhhnLMZcCnSlItBPcvZRiAFPKrURt0HCvCyRvPFrTs4vX80vL4ytmiiQGKeim0rJu5gkh5zveauyoSg2z8Dy8FhZiA3wAk5q2iX+E6lXUU3S

l2y6jBGdpm9xnKsy3OtJ6+Htga25rRwdy0/Jkdzxxa8dmCSdtQTWGdnJ+d/oDqh25djgHSi6gyjd8uLdtG3Kvd5ZhQ9CNZrmjZkibDPmnZgWhxLZ3A/bfA05ghyWi50k9AQ9+W/tk9nR5Wsa6ggS0dq99grWcIKd+98uR98Z59jSt9j9tdr9zpTdrOndvKm7Q5du+7EDCl0M4ycMu5DQz7LQqoTAaoR1eyWoR1HgMidlsw9Aj6T5BNI4Dsk4Fe+K

HNiAeB+GbQbFBYfegEUkHMOVi6SnRe14DGCGBw5rNVqljV4lrV4THV414clI/V5Pcc9+xIk13lXIucy1/+pXR1u14BwBzc/z8oWotXT1w85o3XVos8/1i81BoNnou8rBgYnBoY30JDkLIhsLWYeNn8yh/87KXKXFQqUCvN2GTccjLYmGSPF4GFDjBNBNRGJCkrUtcrZPYR2tamcR+rJcKRwil4rmHtCWrmT4gRx7LttD65xS7EkUNuXJnDvRlk8Z

Yp15kxwlv6/eASsOCpoZu/b5302K0EVqrplCOF4xeWQyypz2kZ1FueKKv609Npoyoxp0u/PADVUZUArAWZlg+78IKK/dqoHt9R+vAgPECgRbn2XDlboph09bspzbsebbzpPbr5sUn55gY7tQCqIRGF7pi7lhDga7/b8eO7sJtFx7seZ7iH17l5pElgz7xo8ZzeTAP7nvAH5gIHwDtmuAzm+UxbcDg9JxdAfmjNvZmDg5jUo50Wk53U0bmBqWkg1D

iAUHzJjpebqHod6/OH3hNb4xpHzElH3W3bz5lgw7xpzeXHzJAn87riEUYn0nix/QCn0qh7p7zAl71AQyt75Ej73AL71n376WTnynh71j0ljuzj14yl+nalvjyM2luDJ5SYeyDgNyZYUULESekw6evyWT7gbGD4OHbFeKL4Ys6YJwrw5ILYSGZoyYTwjYQz00eKUyaFesPNaYSYMIqz+P6LzVlnezgLlqRzl+0c1zw1/qIcjkU12c3+3zxc8B5cwL

onB1lfzaZ10LyAcLn86Bk7L7rVGL31uLzPEiPBzLxXt8nLz6bCfL+MRNqhh4nv3NPFEV3NzN+FZsdhzNICy4He3HHwxQoVtHs1bTCj1yzwSMGs8OYFNHg+CEpZGJFR/uRTLyUU0K1FBdOElUTHxw69eKFqPEBZOVNqc8SREe3rxTJ6mxA4+FC1yYqxCayycumsk1jk1Nk1dHZBwDrp49RmaLZqi+CiTB1yatNawPLDcQhgdIoJOZJrECBWopYwcb

Jr4h7z0EqBeHO9Agi4Jv4P8MhYbHvn6xYDq6uAmuPgIoFAsqBCyReGQJrjGCiBUzTuDQPXh0DRCkTaqkwJDqsDKatdamlwK568CXKDSXICHSEGLxRBAYZJufkkHUEZBd8TuPIJYJKCpmKg0dq/jjA8EEAWg8/qzRmwgdBePNYXsti2bi8tiapWDiLXKBi1EOV/YgudiqB6CcBGHPuEYLWrWD5kpAuoaMisH2Mtq7jTpuoAcG/wGBzgkmswKrruCO

BngzJN4N7x8DPqblQQfoxEGIQxBoQyZowAiGyDohA7WIaxWUEXs1B3BTQZH0DJAYY+w3MDJEV47QZoyg9dAE5HRDYQAA0rMGYCSBSGefDlpmS5bo5VgWnE4NCmzAHAHCThI4ixmBRbB+WzWcIiRD8JAocYWnbFEjka4dk++1yXsnZ1iJP0x+HUckG/SNYedoA2RM1nkUX7Wtl+trEokFxH4VEt+NrCALvw9ZWY4GR/K6HrlP6G4Xk9AbAKKFUjix

7IbkZQHgDuHxA2g+Ae1BQEdRkR0Mj5c/pGzbaEMwwYWbCBFjIazFH+RXdCLWWxjzB3+YFNLKaEYY1dssBbEyF8HyyZgY0QA9rlRSpBdd08dbXrvhQG4QwQU+/VtpQxQEdtJuC6fpOwlaGFN9ew8ARI0I6EkDgeEgL0YMh9Gsl/RBAuxqbWaFSkgO7NFmnKUQJERFS0vAoeUFp7bZpecHY5ghwV5RsNcyvKoaGLYThi+2fcSMfwmjGUCbB6if0m3S

A7ktY+3HV7In37oxl4MEAVDPZEICVgOAf2N5IkGk4ZlC+nw0+j8JxinAe+AIjeiDChBac8UPwDYFMEpznAW+WaJsJjk+AQogiiIolFSwH62ch+aIhzriMJDNhgUxAcYNiOn6OdZ+XnH+uf3FxWtJcJIqlKv30wgNXxhRd8bSMszPpD+OuJkbF2QblA2RHIrkTyL5G4ABRQokUWKIlGDEnyl/QseMRv6CZsILw11t+Qf7HZVRO4+ThxnK6f8aGPfH

/mhCuCvAEYzXc0e6IqwYURGWFW0ZAL66MwHRnhDGM6JG5oSrE43EAZXiqAAsYxwLbHv9UYp00jYqyAeGIHtqaVaO84S6rFQSpEBYascBjj+yY4519AfzeWK3EJ4O9/i7gFgs72RahN3egPfuEEDkCT5ceXSWbqnSvDn43EviPFv0GpLiT68ApeWDi1cnKRNhmsUxsPi14sEpk0CbgebRbwztbJ6gYQCvE0AKD/Ad+WRC4OBY10uAc6HQdfGEm1iH

GnkmuJJM1g5AZJ6lJdm62oKrtFJBlZSQgGTroINJwoX9tnX/Y6TWEBkuSACRMk3dyeKLcPpZIqj4AbJjwdQPZKoLBTCqLk+ZlHXyl9xvJTIWZn5MYiKCN4QUl/CFJ7xhTskEU6adFOGkwtlIhsRKQFOJrbxlayg9gfGL55ZCUxAmCDvkKg4S8ihOYkoZADKEFiZRyHQ0qrxykmCbBgTH0YVOkkOU5JKuCqZ+xgA1S6pcNOuIx1RrNTd2rUvpLC0M

mdSe8pk4Zr1Isnc8rJg0zuHtNGkwlxpzkxCItOmkGDZpxPXyfM2OmrS64EPNuKFLHjhSueoJXabjzimHTb8x0mqssJkrpSDh7HIMscLIrd1++6hJPgJzpZVBAIgELEE5FUj1Bwq30V4TJ35AfRi+E4rcFOP+EICSIhEVYIcA5jrALOZwLGBuPiglQ4cpnDYGDBVadkb6PHBBoJlRH9l0R54vVkgANb847xuIh8d/XNYvi/O1I+IvazlzBcqRf4yB

hF3pFesjx5QJBgbgvIQTOR3I3kfyMFHCjJgoo8UWl3DYZcRiV/EMBhNwCKjlcoaAriqOobOFPgXfSCqsXArwpNwFEvYsRka6IUTibXeiZ10Ynddbi5QBttAKbacSrgxFK/m6Im6CSJAP0poVtX+kVjRkasKsWVN2oKTXaPvL2oQGsr8Ji63PWKhXD3iGxukqMRFrDONrwy8qCCJkD5PbChB+gQiQIBklICjI5QTaVQDgIDq+CvqAQrbguFCpiBcg

5YNOI0CcEnT1k+8P6oTVCr0w/5E0TJA1XSk1TKp1gF+H1M7gjwWqXA8igbUCH01Ga3sFvPhwQ68zawmU76YGNjGzyZpC87aqtxBnlTV567QyhvK3khgd5kNfeW6SPlWhmcsVU+du20mXziA185gLfMyQPymaz8jkP0DflqD3qn8mYYok8paxf5nsWBSwCAUgKeZLA0ui0mUVRBVFACoRPAvYGIKLqKCrGdWIwWZIsF98HBU3W6qZICFd6dxC+GBa

XTMhSYgXjdLTGi9tmKpR6fs18W5i5e+Yh9BUOLGXMhJ5C0SXPKZI1xF5tC6jvQudpVSjKzC+ElxCp57zcAB86kBXGPk8LM6cM5jrnUEXCLRF98uBBIp9hSKEAMi/2nIv4H+DZhKPFRf/LvnMANFRNLRRsh0UUI9FMCwxW4MaoeCGF5ineZYtO42KZYBTexVuycWsLBKbi1umxybGd0WxZw8WR2KuE1BUMcAUUPEDdSqQDwU9N4WOOcJ4pNZvw6cT

iicI/BTI9YQsgSkKj55zZ5IBID3xWBNgr6B4/vk7L7IDklourNesylvEZF7x+I+fs+L/pL8iWG/SVGvzDkUi5UP4sBnCp35Ry9+kXeBj6wcyJySIycqCWnNgkZyEJOcyUT6nwaFyY2HQJUQm3wlVzEcuOFmKlFDxBRPCzcmhs123otcO55bNAZWzAFMSIBdxKAf1yHlOjR5vE8eQJJ+JRLCBQYkFlQoiStM6e/tQ6qVNfbJKkF67BqZlWKXaSCqQ

gFSv7E7g+TapeIGAOZUsqbyMlFit6rMkDp+CQqPoyOoItQBCB/e7zTbnkocrDVsWC0qaUBlwA6xzAukjgMAu6UuCWBxiqmlYqEQtKhEAPH2PtPwB0yEaW2NxnMvwXn5CFLi4hTAlaEhj0A08xVWJPJnUL1Yqqx3uqttpHUklK8lJWvL1VNSSledLRiatCBxV94PgNgFatuo2qWFqChpY6vkXB0K1qAN1Rgk9UsFymXC2ggGoSZuTkm5AUNdgHDWR

rGBgw1wbGo8Hxrv5Y8ZNU8OEBprkE6dTNaMmzWOLc1zipZcoIrXuKZSni9ZkLwwF5D0xD0woYEsvQvSIAb0sJbxMqGRKp50S0wbEoclgsIWtau2g2skBgy6O2JMOHwr/YIyjVnas1aGF7X9rPag6u1RModXOUmlLq+eZOr8ruqZ1PeOdfkrED+qc4S6/FhyDXUbrNF0a4YSMtGH7rE120lNSevTUqQ0qUGq9UIgWVEK0pD6lZVHw46PZzkrY3uu2

OT6xkqgRgFDKhitQUB6gufcDumRnrvDcM6YBsKZACIvKHCa4UsHOKIgTBF6pIbMJsEpwrhOyUItAAXkOBnAz6XwfNGCiRH8YmcJ412WeKnISZMRXUL2RORn6QrvOC/ELsHJXKIrQK8RKLZHLdZQNsVjIk8syJFWUrUJH07LnKM+h/Z7+V/VUSsGmC2FIKnyNlamIl61cYKRECFBDHXAGdWu/KpRqAOtG1sz+/c+4tALbT1hswK4CEScKQHHYZVAq

j0boKWQyIN4PS8BbuoynaDVeNSSbUVNY0za2Bp8R9as2fWgdX1KjSDv4q/VS8glv6/9Wcx8QodMBE2lZNNoprsaBZay4WU9lOGHitlCmrsWRDWDGxEglYIQEYBHE6bzlG4YFEcDzTGavg64LUYREuCHAco4KKYJcGazmy3CFZBYJTgzAQohutOSIsFB82P1/N4mcfi51JhuccRAWv2QSJ84Jb0VH4skbFu/FBzEt5mOkYBMaKpb456WvuQ7ipW8S

i5uWwTC8gK28TVRrm4FMinXr6j1iJkAlJyqPJTAY0LYPUbZGQoWj0BVonuTaI62QAB5/XHrTCnhgJpKULo5AfxNG2Tz0Am6gYadNQB3CtATAI0O5J3Vrbdkm8AAvjOuDbT0WoVWWBAAnDgJMWNAFZBOtAI9CV4aUx4CKC7yqx9AlobsIAXSU+1h1psHeNdRYKxxg9PsW/BPhnbqVyafxfpGpSETW6jYpAO3cfDVgeVwIpU3NWoDBBNp147YNOIAC

ICVAE5APn/oVK51fpqgDQCAQN4he23aVlpokzcQQBfJZ/mmjEd+gQcFZCwIfaBBYqAlHkmAVNVaUIA1YFeOpBaAsIIAqdDNVBoJLt7dKE+bOCGt1hBx1KcAPeJ0pJ7ZwWZoBMBGfIDAFr7BK8RPSXRfZkdhQ2exRI9xZIhB7qkgUZLMJHjZxsAvgDvKQEb2oBAIFoQaMoFGSigQamJHEM9XICsLO4aAR1HfCALkAg4nQkMPQFxArDOIQoZQEnB7w

t6640Bi/XiDgP/FEDdcZAy/LQOgl5Bmse/UeXd0hw6k9GmPQRqv1x7v5nSQmkRuVVTrvQBgFZHFLCBQA/iPetWDHSpj+xw9t+iZpPGr3uAn94hpgt3jvzqT464apvS8k26Op51IJbvauqFBI1USgYN0jAl33e8GQvBigLgDaFohdeG8LPXXBYGdIlaCQj2lZIIDqAF5w62OJxsUVmMUmhLeWL6oLgQHF4ooc/FPk/2EId4+EIyBHtCoJTbaG+Qxc

BkmplIpk2cC/S6Gf2T49YkwlZOwK/37xYqfe4vQPswT4ABQzgCqP4x7ydIqZbkspMz3aS20EEGtUEgSRUik0OAAoI8E/onUyDzA6JUZO6S5Ahh/Ds4GfAMmHVBAwgX+QIH8VQzikHF5AcPROoACkwidI44DgDqwsgF+8gL4jcZuTJ8vG6go/PDXFqIA5u0BZrDqMl7O4Max3dVRd12S3dKhuOl7p90FqvdTTQtSRqD34JQ94+qSRrWj3qVlkcep6

hYtf0N53aKel9irH96Z7y41R1IagDz3ewPjDRsvVJGfCV7hEjEc1AsycZX75YTeig6gDb0VRD9LBbvb3pt31H7dLk4fT4GsBj7w9+8SfSvGzgz7yOc+8eDyHRBL6u1XutfcyZCDEAt9O+/jYjTVX77WTgYZJhvBP15Jz9l+q7jfpCNrx2DIJtKWnrROp6PDeJrw9/thJ/6gj384AxvFAOeq75kBqg7AfgP0G7YISDkMwYwNYHAqNIexKXo4AEHhR

msYg2CDIN34mTXpmgz6Y5IMH/TqBycCwYHZsGdjAJ4deNDo24skmdMgOvLAEOzChDABEQxOrENuHJDykaQ7IeabYHQaCCZQyabjpqGtsmh2s8wR7x6HRQEVbCAYdQBGHMSJh6jafnMMn7zAdxmwxkfsMam4p6lSgC4aJi1ns4nh1wT4ZpIbwL2LBVo//uCMWLQjp3WYe6UiMtBojphtOAkcnxyUj9ePTWGkekAZHOZa2beUwDyPoJCjG8Yo7QJf3

lHrTRCU+PidqNcnPjjR5oweZYIdHA1XR2OD0abh9H/YAxnWiiBCbywxj4esTT6KmO0gbjz88FqZRYJLGjQKxixWsYQAbGCT2xkQLsYh7Kqjjz5leKcfONR78QPIAiIbAOnWHT14igsE8d54eL0hyY7mpVtyFQA9t6BLMeqWCWlD5eAG7LUr3O39ZXjPMkk/bu+MjDndv8V3SkE4OgFPd3u38KCf93H6fRUJ7E8oLD0wQ4TUe0M4ibXjImbqqJ8o8

nr7NYmQ9Gej/Xwhz2Em24+eq3eBdJMqJyTRoY6lXupO166TkBpkyyY73smoDnJovRBd5MwR+ThdGyxPryTT6Nks+3SFKcX2b45Tq+lqoqc31e7VT56vfeEAP3amA9ecM/XXD/NGmN4gJs0yZYtP4IrT7+jKviZ/12lHTAB5071bdPgHPTMBpM3QZTN+mUDzi9A6gEwMohsDoZvAxGcIPRnOAJBuM83oPmJm2AtBhAzNcYMBmMzLeVg2kjou5n7VO

8To0WZHVxhSzuGoYb9QrO/wqzrqsjWuYkPZwpDxsRs/IaCqKGGLgJv+b4i7NpStDN+Dy3HQHORVhzo5uuOOb9ULXAIFhmc7xdsNnqBNDhpc3TOcOuGfrNpz/XafAXbmVke5nvAeadO36XwYR36ueZWlRGOAMRmjTecSP3mdT5R5i6+ayPvnWFn59WrHB/Oaw/zpRnq5UZAuk2CrGl4+GECaMtG/9MFsOHdfQSIXe4yF7iLrzmp3wMLox8YzhZI14

WZjZjeY8RZ7ykWqLnByi9Ra2M7GmaDFw48cZfOsWr8lxzizcZ4v3H+LTAQSw2NWVkt1lJwuPmGRe2SyU+1QtyCkD+yYAKA6IG8actVmAo0AgOwzSDo3AmbwddyzcNuJzCZhUo8MGNAJkc0mQW0JnM4C4XbIY6uy/fFEb5sBWj93ZIKz8pP29ngrfZ4Wp8QKkDmwqdMn42XHFqdaoqXWYXTFUzo1xATvWx/PFWIxQkFzudMbSsALqUtP9o0iweGP/

xDxVaWGxXeudVsNFfBPgkFUrXRInndzq04AjnbhS63a6e+NZUzkXh4kr2RtLW03S8ZfC5TKFE6hJUUzoWNqdV1U6glDPUnxwggUFpW7dXPmlLmNUa7dbLa+NsaEFKyNKyPoFPZX4EKyfK/PulMIJGrMesW1Mn3glpoZ8sNOjjY1NwIOS9ujWNfljjU3hrsw861mYgRRwFEnqp/YRyLAA3WzhVfAxtY+o8GvSHAbkryRaCyLR1ZZ8I5wUaXTDx1n1

2MAoECCqAQkwNIG/4JxOi22ArC1nvlZ95w3oqzxxoB/d+l5Tv7NC3+7Bvg2pLIZqklOp0nlvgO2jntKB/oGYAwOt1lu+Bw7pGHgmWHaodK6PvQfBwNzEpgqwvvRC4PT9+Dy/dGKIfGwSHokBczWoflUPsOtDgxkNcAPhGmHWHPxyGGECdw0pnDzgNw6UO8P1rUZgR4WaWkilhHLpQluI+cqSPXrL+D+R9ZI1urFHdSmZQFTUdeWtWQBLR/4NAK6P

DK+jnnp4pWbActt2Q8S2+skv3T9tmYr3sxGKGy95LoS07Z9Olqq8jHIksDcqp/t+jLHDCwBypKSogP6EYDxW04/4UtS3HXSjxzvC8daX2NvjlBxlaISlYMHIT4UNo2wc8lIn+p5qzE8IddgEnZD9U8k8ochBqHFUdJwEcPPfycn2jPJ2w8KfKDinHAUp/2CxoVPGAVTxaUI5EeulGntg564IdacyOg6/g6s2Rq6fKPPGvT0GunoGcX7tHIz0J3o8

HMTPiWAZQWUcOk2izQ7NLcO4pokCihvs4VIwKpEkAjhmaWm/PjRWTuoBU7wO/NBnbB1ma9Z8KXHO8FsICtkoCwFHObLJCxRXgOnZcSVAcJaiHZkIATACrdkBaGUzdsFZOXExk6oV3dmFcSKp0hzyR8KlFfTqp3/i9yMcqLk7ITmz2pRXOlezztdy4Bwqy9wrlXKLswpUocFCrTZ2WfgU6uIMZmI3zJC8rFdncs++hQvvCqr7Wu9iTruhQP2pVz94

3a/blUSuCeK8MiKNET3PHRQbbmvJ26YAbbpnIlrxWJduki9LEGYj/pL1WfPT1nr0hS1s6LEqWLsvbjt8wC7cSbDhzY4O7JoT4XCB6gnCQO5HYApBVITkNlondHFqz9NQOozZq9M0Q70wnyWKCDvmBzBXN2r+soivR2KswdhxYCja9+XIiHXLshu6JndnOdPZrd0LRCpnIRboVRIt8X65i1fjw5w97fjSLHsASJ7LO4CWltAlX2L+89uNzGycjJvK

5hMTdAjGbAIwSJDc00FX23v5tf+Rd5rpTlLZK6u55b1PJW+jedaxVNbu+58hyhOzDdw2ptx1wkuSR3EfCNdxu/m0LpOwMn9SnJ4HdCWn1w7l9TkPmdSXdmT0o7XO7/ULusuylr6Yp+Ni2m+367tT/7ck1CyhXT2sWaK8uGHv0Ass8KrMHFBWo3Uf2gvte6c244YokFEqPDGXBFQW0ThTYBzFijLibgMabMAjHNmbBmMDhfMmSCzD7iIiVLKENEVA

9Ov8dHst12Frg9d29EPd3133Zp2ofkVlOkNIzuw8wNJ7ccxBuzvPIkQnIZEN5LkEmCEBqgygGQXcNQxQBxgpAc9MoHsjixc5EbWN5Q3jdVBNAAIbCaPfLl4S/yVc8GBuC2CJYKtnhDcNLsKgswNwrMDj6W9lXcea2zEjXdfYE+S6HRiwMjA29dESfLRfniQOFTIgRw7jgLsxhRrvxJDqSmITxo/i4E9DXFysRKR6uniewdKW8fsDnAflQBjE4agy

IEAZKZGOS6Idgaz2zg9Az0NeCON4xTo4hwFAoSHmnGcBJXozRAYOoZXYOB9gHHAR7jpVOk/ON4nq69UTXe+fePvsU7pinVFBiImA8sCnz3s1js+vKEcLeGgmPj4zs4PPu3kYH5+C+hEhlVIXfl++EJt4qAN5GwAOsT4x4CRq0J+B/COBQqRh/wFFWF+U/UA+P3nyhCV8zxHF0+Znwj+h7i/D5tIUEnT7gAM+YpcG0Kn5WcBTUikhPtW4oit+0WvK

Pv2x+ghQRs8d2ygHU2rD0DgLZfG8eX6H7KQC/HfQiKZK7H+9n5AfPvTpBaHYJuNQqjeAknBpUiA/LfLgJK8pVHyF++hi3F8Oz4iv+0m/gVFqsC8sY1NQ6/S/9P3/+LK+KjRsZWE0ex9s/p4sVPaRzMCvoXEp8sG3ysnF8MluQcYbklGO98M+0hYXEbNfC58jSRHncd0hr4L81/NGDeBcCD4HgprOLFtqH4iX3hRw4fjgBH0j5bh6w0fh0kIJj9Ph

T+yTNWq2+CvkT55WZPovAi+rptT7+CtPjsb0+MfkWBM+qYCz7BO0/hz6uwR/nBoZ+fPugjZ+BYFb6i+kPpki/oxSNHAy+dknL4E+uAVn6j+qvtoDq+XqhrBa+Ovnr4l0QiIb5WAMECb5WAI5gdYhgdflAGawIAZn6xw+AU76/wLvnkhu+UPj1AymLeDv6IBscPjIB+RoEH4a0dhnb6K+YfvvAR+8AdH4XO7+tVQ4AA8In516g+qn6UB6ftQH2+eA

aP55+FCOoLOUl/i+DXcYcKX4iUNahX5rYVfs36M+hAU2YX+6oMvit+xAR34F+3fiKbqUlvAP5x0Q/j8wj+OfmP4IAE/mvBQmYvjP5++8/lAjDGQjiv7Zwa/p3Ab+67viDb++gbv6DuiYpp7ba2nrtqLO0lis7ZiBnngQuK70pQxAaqvFgFfeETj95eqQQYD6dwwPk76uwTwg/6MBmSM/6w+V4PD55In/n0jf+CCL/4M0WPukFVq4LBoY4BtgWUjE

+97BAEBBVPrbSwBigYYHIB7/tIGr+UPi3iYB8viIE0BYgcr77BxARL4oIJSPbpp+msJsHaBtAUkH0BjAQFIsBuvizh/UnAcb4oBZvvwH+B9fkQG3BWwfcFJBcflIGs+GQZkhyBXvuUFKBL4CoGTCCAOoHcUNgV8EIW4fvX6R+/sAYFqSRgfH6mBSfhYFj6I0lQFaBDvt7AOBnfsaB+Bxfu4H5Gy1OX5MUvgS4GCBDfrsKshLgREjT47fqCQRBaIF

EF1wMQSASD+mPBKSJB3sLHDj+BAGkFLaTwbP7syB0gv65BeJC+A2+BQdPDr+oQCUGkAZQXRYIBhgXv6wg/LvdoOeIdjxxh2LnlLISA8IOLCoYlYF+CAQ9AL57KuFhAF7QocODl4/AYUFigK6kAFFBEYvLBsCfI5wACDgwMOhuJJQpkFt4xhBdpBQDamOtl4ge9dvl7tQ+OMFrQe7nKTqd2Acj65IelXgirVegbrV7bkyWuG44q09n6xFAF5B15de

UAD159eA3kN4jeY3hN5Te+cou7oSvOgt7LA5HgyqEw4MFCAIUgAkx4QUDYNLq2EuUERiXAJ3s1qSeQjGrrta9bDfbsSTbPd4fAj3kbqKMG4SoxVAXol0Hd+ZgPvAq+Y8J8GMhTAI9wn+9YjUQH+54WwiXhaINeGZIhlHeH4hD4aQBPhaJPPCVB/PFp5zOdQR+pLOU7vp4/qhnidomeJ2BEqq8F4TcFXhJAD+F/hDIXYE5+QEYC4vhdULaGB2D2jJ

qbKznge4uh6AGwBuQawNgDMAdwrdCXu/2v55EQHwMxhgoyKElBZgUwOGHqckIBxik4sYSuB/A8XpujFuDwA2TnAGOLAJJAzKuFBeaxONm6wgeXnjqc4rriFrFhHrqWGEitYYAyhyg9kZgRyIblh5huzOgyJ4ebOgR5te5QG2HdevXv15CAg3sN6jeZEON6TeFKpzpZas3u+QLew4nSoVyE4YLDgiMaCVCNa4ulmxB40ulJHEYcwE7Jls/DGd6bhF

br3J8emuruG3e+4a3JHh4nieEveU3DUAWeJNlZ6J6acB26oAn4ZAgYRt4UIj3hOEQWCPctSCdJ9waeqQFS+TaDIEkBYcO2BaMmgaAHoI9IMgpiS8sEkadqO8HH5uAVITDJh0TgQD5X+JfguwvglfuEDV+wQQ87zBqPttRliK8In73oaqif5pw1eFiAwBK8NIKyCIfncGLw4VBaFkhKdIZQgGx0acGoBHUcJo7mmsBVHfhpAAyQ0GI8NyCJ+acHEA

AAqlD6tRqCO1GPwHAEQHu+NBmgq5Ar0eVFoRX4VVGQ+/wUIiTBr/leDhqPbEDFwKI0MHAP4+EMkKcA4MZDFQ+gyMwBDooJANHDO10b774x3UWUhXBM0aooMQglDKEVGzAUIgqhTRuGqzAfxLVFlIVMS/BxgOkMQDgxAMRfoUIPvl+EEAvju9FIxYQEyCXy50bCEvggsT7xq+RNKQGiBL4Cn4IAj3KiEt46IYYFpweUEdGHBJ0QgCRCKIIKHOBwQe

DFXRUfr74TRcIDoyDI/4f1G4xfvishyxN4dGL5+TMcKHXcqAH9i1AgEJYp0WeAGECPcw+DyHLRfgX8xyG0QiYEuxYyD75X+ZwUHA3GsSL/BGx5IQyRyBOQHTJhw2cD7FMAsVAoiimWqrtSGhTAPnFogUPGXHRiFSODbE88sLyxkQM0SwSRBvfmzH2xNMRiEXB2HCyG2xRfk4pdUp0VEKgkOxmRBOQjQNr5NoBNhggFxoDjYKNmHwbPHzx/QIvFlw

HgO1HmBcVMKFGwgiFIJxSx0hPEog4asxiKm3oHHEuB4MUrAtIjgU37ChWAOCynsupjdFq2rsOz4U2PgbfHBBU8ZoD5qoVGRbDxs0ZiFahFsVMYQ+NcS9HywNBtWLRAd6M8ZKelnqp5xG8sGVGlx1UTCEEhL4OIGARIoctqMQzUfgggxrwUPHu+nSN1HiCWsRdGbIuMUNHCOclKNFc2xgQn7qUefgHGrRGCOvhUcS0U8J+Ba0XpILBXjAMjbROQP3

g1q+0YvCHRx0asJYGdCSrHywDsaSG++d0dAHmxj0a76DxL0d7EIxlUTeFfREIUATRAtVPLCAxwMZL6gx7ksTHoBQiNDHbycMdgnIxy0qCCoxMPujHCJHAFjFQ+gsUUFMABMTDB2JyIUmoII5MZSZqxucSnR0xhMR1Tn4BfjRr5qbMUBZa+XMQKA8xfMe7ECxnsTQmpgYsRLFqJ0sceD6Jn3uhG+xCsSUb5+2SbHBqxvwZrE1JOsfaYGx5+NEn3wJ

sdoBmxwdOfH5I3CYD59xjsQPHOxzOK7EIIjSQwnIKXsSXEGJH0X7FPxQoQAlBxIcWHHoKEcaEB6xqdLHGCJLgQnFNmwyWYHKwacZ4wZxiPrMbZxdNv3GGB+cWyiFxGCNMllJiMTeHlxZVsvLV+9iXXELcjcWFJYILcWnDtxncT3jdxTVn35Y8XeiomXJ5IYPHQ8/QYuw3q48ZbFrCU8XRYzxc8W8gLxq5mjw3JK8fMhrxNeBvFopW8auY7xJAHvF

0mB8QAlHxIzI4ZnxCKXfCXxfxOpA3x2yXbF4Ij8WAlshr8adLQpn8d0bfxYQL/FMpK0aPGtJQCYJQgJVtjCkQJsUgdKnR28MdLu+KkPAkHWiCeaghgoEddKjuPihO6fqObrBG7YLQTqSKW7QchHmeynnXDoJpUS+AuJv4TVHjJBCQ1HQ8W8KQkqw5CeQHPRGCHknKxeCRMmTgidMwnmA/vmNHT4+yZwnTRz8TwnzR/CX/HMpAwcj6iJMLOImbIki

XsZuMMifLByJ5sQonLWSiXgngpgyYYE+890VokVST0boksUb0TMlVRxif4CmJf0YvCWJJAdYkUJoSCTHWKB1jDG+GLiRr4cx0Pr/heJmMX8TYxRiowlAEgSfTFEx8sK2lhJpqhTEt4USRCkxJY6XEnihTMUkmsx8oebSpJnMSkGqhmSbgkp0asXkmix8sOLHqSUsZAgyxpSSNLlJmSJUlKxOaQemex9SdUnYR3Rs0lsoSKfmnkhHSV0n+CPSTbHg

JAyWolDJycSMm5Mbsa+m1Jnse8HwxDyYYmZIzIZKnF+wcaHHhxqMesnRxL+FslCpOQLslqwIaanGIAxyagFZxzSBclfpBtNclngL+MXEbw2Cc8mVxmlLAkfJDcaQC8KTMj8nWgfydoAdx3BICmShPcRunHwQGZaGQpNcdCl9JsKVKbwpVscE6tJyKfinopiGpwrUZVzqvHW+KKZvFUWRKf4wSM4GRhqHxa2FSmnxbiVmkyGacFfGMpvISylLwbKU

hmcp78TAg8pCFnylc22GWyGGxoqSPDip7cEhlz+MqQilyppmQqlsASqXfjeZqqXNp8ujYsRH2hu7ucL8czoRHYSA32LODYABhFaiTeTEX54quYwKuIvuCwDywriW4E4TOAV9B8qpQS4mSA3AxdpJGFQsUBMCrhAHr3xAe3moPy46yKrqyQeRXrB5z88Ht66Iev4sh792ZRDV5GRdXu6wNeB/Lh5T2IEifwZankcR7eRGEgt6Oo44Wt7NoiOEjhfK

4kdqI72vAGpwVcebiZBvu2KPni7Z8UcAIm659jx4pRLEqKpsSt3quDg6GXo/ZyMjbrlEq6r3ugDV4nYEKCF0Y2H3jPGv2Wtg8QRkkDnqem2tUGzOY7u+q+Kk7mBR6pziPBHGe4Ssu7kEfxH9lg5gOb0CbuArtu4iyjniK7yaYrl2JGA8QDAAAxjQOMDYQyoNll+hc9GgB5ZLMDF6eEJOI1mOE5mmVko4FWVCDFskMNCimu2YFpwTAYkV8AZhCkUF

CZhzsrmGqRSRIV4aRJOlpEleZYYNloqlYUAy06aHsG4TZ9YWZGxykbq173ZmWktmP8c3hIALeF7mXK4ShWqm4ysmYFmBrgO3o3wCYR2TVpiRCMCChZgp9olGq6yUero7hN3i2gOEKOMRjl82Ua+Qv2p4d9kQATethCMguBstSKm0CcKrL+K/mnk2+FPliATQexuuipmACv4Jfg1gGYm1oXerNbPsK8EwomWd7K46xUgVJQrd+rvLYqDM2MhZTLWU

kjtxmJzVhMHRgqIITbX4gyhyDwsY1BnmoAFPkYbkhzgAgZwIucDOY/gAYAtaigs4JcYsWsjv4Igm6+B/C347qvXncKd+NSBvMscEXKEA14UIC9MwxoFKng2CqPkU+b4FyAVwoyN9jlgpSPXhoAvGQYCu8EjKgBlwT8iwQBg+gEXruqPICNCJ+3XKzwhgzgIv7cG/Js/KIkmjDflGSWpnwhOQbhi1QzKaACOCiQH+PXBIFKejIgaBGCEo5FgzgEQD

4Q+DqgVEcu+SnpCIIisEBX6K/mPmoA32O2Bn5MEGRBpxn4LbBkQxAKej+8aAPPngIbefFKawZgFoxqqOQOahJSHqQAoDR07NvAog3gOISCkqWIxD+8CBTz7nq6lD+CfgTkEwDsAdcKKDDSKlIlYaF5DjWrxMMBUsHogfYCiB7yCeXBoo8CxnDIL5h8ofm9WwfqATzgxPAwW35MAKJATwhJlciUQ0gIlZtAQRXxhgEWQKf7rJdcLHDKm/hTOacQjg

LYrqxDASvpGw1sUEDBA6iv7pe684Fxa5IRvleBe6dfgyaoAKMM4Dx5qMERzqQKed1yj5CBWEW7UFgn3D8Fv4OS7t50BQ/nqUwQkIAFOxiMdyVmO1nboiOHcD1FrRPhdr54geSkZI3Mr+f8TL5+IJXEFOdCOixQFD1uS73UtqptaMgczDgJhwRBSEgIFI4MFSsFr4CoqMGszPrCjIbRY/oNshTqmrNWaIBfo0mRUqcVqqfBkoYwQgQMEBlw/ghYUP

5ExSv4U+uhaQCCIO3GIBoAuEMtZEOJ+UwDfcTliZZuI46gcUkcISHCUeq9xB5IAlLhvQXAlI/onAtxZeQL6ElmsMgYV5UpqJCgFKJXXCwlBYN4X4lReYspnYMJBgWhAoyEXlUlTEhgivxnIGUgjOsLjkyj5zxnHkJ5NRYFn8UVMA0X6hmeXbA55/jKhD55FJZyUl5wdGgDklNPiCY15YkrvlKqjeWgUhILeY9xCFEep3mJ+3eajG95T8t9YD5Bik

Pkw8jBAgUT5BtFPmBws+UjQCFi+YsUMQY6uvkmWm+XoXM4O+b4h75rhepRH580qfnn5IzNMpHFMpYwV35lhU/ksAL+TXBv5XBB/l7wtej/nYafaAAWn4w+KQAgFwquAUuA6xTiWwFgQPAXxlFPmNhIF6lCgUGABpZ4wYFWBRPgMIuBX2b4FmtNKHdOnACQUa0d7E8VNllBSGXUFPsAQDhACBcwViFbBRwUwQXBTwWYEfBfJCCF7phHqiFrBW4wSF

0QMrb+i0io/j3s8hVACKFDEMoXlgqhXkH4lphfViwQOhXoUHyhhWoDGFPeG/lJObjBWVWFNhVAB2FqMA4VCIuJPejuMLhQfnhl7hQQWeFLhggXVAfhQgRI0TRZIDBFNTmXkIVSFZEWuOYZbEV8CsFQEVJF1eio71JXuhkV5AWRYAq5FEAPkVuMhRVwGdKEAKUUcATehUVVFieSQTJ5QgKdL1F8ZY0XyULRTcWrlHRcIV0aMBT0ULCAYP0UQyOlnw

LDFZCGiRjFUsH8yTFbyNMWfgYPGmULFPvksXrwKxYrGs86xZ8UCGegLsW+I+xb2WMueJbKUnFE0GcXVAFxeiUd467LcXgI9xTxpzm3fi8W16oNFuXYkH8gxY/Fi+JMkVlQJbKWgl4JQ5RQlt8sRxEA9JazwgmyJbS6olXIOiUrCzlTMz35uJQgUkllSMSU/JZJYhDB0ZpdyUdGQQHCVPyCBUyUIcVBGyVt434MXnUl8VXXC8lo0PyVrwusAuBCl8

ZeqkzO3ipsxQRDQdO5NBcEQani0gGian9YopdUWeBbFRxVSlXFTWVyluQLnmKl5ecHQqldVSvDqleVbAFalV+DqUhlepRQUywRpePDrlHebVXn6PeUZX95HZvKXD5jpXNXOl98K6Uz5lhvxXEl3pavk0uK8BvmuU2+RghUFPeKBVYV/CI4BRl8LBfneq5IeZUJl7YEmXP5jEPMXv5+gJ/nZlPILmX/5gBanRFlLsWAWgEEBeWUw1D+fXBVlo0BlV

1WyBQdUqOrZZXDtlZNXgXqwBBZ0iHFLgKQVDl2BiOWTV/1boY0Fk5VDUU+M5WcXsFiAJwU14S5XswrlAhcdWdF3+SwXiFzOLuXtGXUTIWHlRMMeWnlHSKLXSwahXNU3lMetoUwQoJfoX/ERhXJSvltvtVbe8n5QlJ/+35b+UmqjhaZTOF7DoDXsxG8B4VrwXhdBU4VM5qhV8YoReEXpG2pZhUVG8RXBUsOyRTMqEVVbFEKkVORYnQUV5cIRYz5/Y

CUWQGTFWKWTVtRexWSlk4NKWTFCFb2xxKrRa9UmlUkhWUiVrDn0WeqAxZJVaw0lSbCyVeSUFWMFSlXYAqVGvHxVL5GlT6XSpIijpWgEelR/IGVVpZraolZlccXvFMEDZX6KlxfZVfsjlZiV6ZLlaepuVB1h5UT15fj5Xh6flX8Wr5aVe3gKV+JSFX4gEJQgDhVMJcVXRVoBLFWbVIVAlUSxrAMlVYloJIFUZVnGUXWZVO8BqXr5Z1aXlFVUVfCVl

Vt6iyWMUVVRyU/1NJWzzcgTVaEYtVgpQSTCleOXaEhkZESTlJZ4rugDVA2EFAD0A+gOMBkQxhIq5nKLEczlBe9yuzlfAnOTq5M5OaFpx85Goj8DIo4MLVnfunyJiifKG9j3yWcrWemB12HWYG5dZr9Irk+yJYSrk6R42dLgBupIpSLoe1IqG71EDYazoteVkcbmLZQ4dfwjhKQNUDrZyIFXIlQ5fKLlOyFXD2QcY0umSDnAxrhdmceZbklG3ZAeX

aKSMIeZfSrhu2WJ6R5z3l9n5RdFAhXj5alJPVhWT8AgDgk4ej/FfgJjp0JkQogHJQimqLj2qWqYFXHRHRwgHXBOQFcFADglPvG8hyyzdYCET4iTUIDJNqTeCX0VOIJ/UGAEsTNVN6X9SvCrV3JaKC4chlPEiPcCgCLX4QL4FTll5tQAgCaAqAEY53yfYAbB5+xAK00LSC4PiAeSBVV8V+OE0GUUS1+AXzL8FZ1cBXgI+8P9n4Vo0HXljlhSjXkT6

5cOWoq2XtUjSflwGHQV/MTeuKAjMczQbALNXJYU7g12QFEDQKe8lfmPV1FS9UxmpBktSeMsFnvXEc4iFKY++2+WUWNlTed8F/N1zWYlFBFNaxV4A9luBBiSHZQlYK1plZNWiZR+GUVVNr4BfXwliofM014ygPSB1Ke8SCaPNbNSC0qOw+MQDOAcVT+VJpu0chUVlzgCmajI0Cqc0yQPzTU2l5lzZrD8FnAGoBjN/CJAXg1n5fQ515nqhVCD6BeSv

D3FgTGHAFVoVn83vNIgNfkMVgCJWA+8GLcWglVoyJWAyVPJI9xoA5zYOx/1JVbc0Z0JocbCdwvvAJSJ+Ykt+GzIj3BQImWFLZ3DwgrdY7xlFiZYTW/p7bs5kvgaAC8h8YpsBEWot6CO5a6G1VDSBwaeNUK0E1LhjVKJVTVdgVTIusFAAvlf3qiWhgEPj7r0mHAFPWhU2EJDzgx6aXzFDNI8I6TuSCgM5XUt0taFTxIfzHRTxIH5jgbBw+MgIWxUh

rQVadIwLc2U4tBsAFRZYMJb/J/MPbBq1Yt1pVy2oAkQsZWdwbbf8Tg1XbVC29tSwnizCgdMhAhR6ZxEs0DKpibSBmJfsZshwlrivO3FxGgX8y8xc7Wa1xgFrVwZAEYrZSbZwSSH8x5QbLZYUctwdBO34g7ei/L269xQADcS+nTQncXAnABitu8IxCWG2CqCSetq5t62WtGsWYChUnTZoCG+mIHkAKAX4OLAyy1PEIjQdxPLB2jpviHvBQa1BjBBT

I9xX8y8sL7V63yJgtQPEptX7YsztRX1SZbQKNpSsijt/9eO33q8cFEDpU5rcHBQ2D7Q7Dgmi0YXG0EG8D7ppwmCRvGYFxAOy451i8IybhUooGHEKAIJg02pORNKAk+6dfk3pvII4Kp0+6CgFAUKAwAFAXOgFkD7zqQexSsIgGIgG5Qgmefj7BulL1VAW6d2vgZ2oACgPqW2KCgKtaMAycOZ3EAAALxmdF+RZ3WwU1KF1TU9lFHqWdhlG8jGw/tIx

ArCPnTLDudEVCp1edgVaZ0VlFnQoCtGDEEbAqwhlLh2ew8iXekBGRXbC4ZdAMap05dwAHl3EAFkAoDvNPvOLFf5n5Yq2fNdfs8ZeNVyD42MAfjU+CLwisIE3BN/KaE0zy5tBE3GF0TbdYWqfavE21teIPk3N6hTcfUZNWTRT6sBQIXk0FNC4EU3AIIzDiDsWRoAp2VNN9TVVclnLfU2NNXnS00jw7TWgBIdPTYYr9N97GHSltu7FIr8tTilyWTN1

LTM0mWE7eC1mBxLaDlmUzedgbmAiLNs3CmuzTK20lBzUJWE1xzb8xlFHbUu1l5NTbc0jM9zS0C/yTzZPmvNiRVtaxmnze6qfl+8H80CUALUGVAti7aD3XdELaS3Nln4DC3cgcLQVS01SLdQSMuQ+DdEjI6LVd2at0Vcz2uRBLa8W2CLHUT1s9KRRS1UtN9bFQ7RbBCwQMtTLQMqstZXW+3+CzPV+C8twxjDGCtIzMK1DWorU8LHw1bdK0YIcrb81

8y3Xcq2VNDsOq2i9Y7dq26t6IPq1ztITFT1jtprXx1XtwcFa0QtsVHa3YyjrY/rOtmyMpXutKrWV3etNeL61l5AbUZBBt6RiG1lIYbRUYhAJqtG2m9sbRJUEk99SkLRi9HSbXptvZZm3mU7RWnB5tqAAW1twRbRwAH433b2ntRlbU/XVtCHX/Cig9bX8SNtAts214ydkrO0dtSPc3pM9ygv22Fgg7VEDDtA/W72cd8BsoJTtyFmP3HtyTVP3xCLe

Cu2nqf8mU2btxLSoqWg6IHu1OdqgAAR6VJ7dkBntJpLx0zm/HQgg7wIHZb0d+QnZWBPtmOYX0s9a1dj2ft4rbUpq0WJf+2b4gHTbxCIr/XBoVx7pZB0t4CfcdFwdaRT31IdKHRa3odmHYBDYdVHVBUcA+HQ/j6FxHbiCkdY8OR1tx3/T82J9tHQWnl9Q8ak1YGx/foqCdG8Bx1at2PfY4P9SNE/1sdH/SJ3UEmbdDySdo3RwCaZsnfJ15AsVsp2G

dJlhp2wu01MsZLN7nfp3SDAYMZ0X5pnUF3xd1ncZW2drpvZ35VJlk52k9SNG51lFyg151pdISH524GAXUF2hdmg5F0aB0XRoGxdcAPF2JdIVDYOi2ULdzxAtUg9l2F9EQI12F9+XYV1QAxXZXlldoBpmmVdYQxEO1d9XYEO5dIQ812td5PXfiGUHXbXpdd6Q0q33wvXZDlDuAqHNg1BEEQXy6e0HDO7NB8HK0FGpj/B0F9UuegN0jmvjZi0jdATU

E0wQITWE0zdkTXexBwMTYt0wAy3XbCrdB3Wk2bdCXdt3a+OTWSVjD63Yd3H1xTSd1lNflBd3LVheeA169d3ZbBNNj3W01zxL3V01vdfTTSCfdXlN90jNRvaCQTNDFkD0qtszcoJg9T5ix2Q9azbtWw9WzfkwI9woHs3I9CRYc0/96PWtFnN4Nfr1nVePakYWeJLbGVllLnWT2jGFPZB3fNlhTT18ydPURbKAV+oybb9YLb/3Hwlg9C14QXPc+Dwt

vPZX389KLUL0zoIvZK2Yty/dj1v5+LUo5EtsvVEAbN7NeS0v4lLdS0q9yaaUg94GvUqbMtQ7R60/9uvSvD69hvfy359t1j/0itt7Zb0StFJTb2dIdvWiMGwjvbYrotLvQ01L9bAzq311ercSUX5fvZx0B9j/UH2WtGtKH3S1+8Pa01iTrdyMutsfZdzx9P/VQPJ9/rYG3EowGQWnZ9scLn1RtZZTG1718bSX1JtY8OX17l/AzeE19C+XX0qKjfRQ

DN9rfa03t9FbVW1XdPfXW0WJA/f/BD9oZiP0jSG/SEwT93bXVF8yM/awBz9uAAv0bDK8GL3YtE7Wv3HwpYzvALtnI/gnT95+Hv1rtegBu2lYW7ax2n95/WHQHtV/Zv301t/fLDnt4oJe3SGz/aLZ3t7/SwMOwX/TgNgNN3e+2YukTXfCHlwA3pmgDK8OAOncUA2B2wDKRfAMejiA6kV34KA101oDaHRh1Ydf1Lh3Owt4wQNEd3vCR3RiZAxwCUdC

A5mnUDkKbQPQ89A8taMDoVMwO5VdI02Ncd8QhwMLj17TBOqtfA7yA3hgg7+BSdIgzJ1Mg4gzm1KdWXWp0yDWIJp2uw2nb+BKDnnSROqDJnWF0hMFnVZ3GwOg25mElDnYYPjjxgzUohM1E6p2EjEQP50IAgXeF0hdDE4EARdUXcAAxdUQHF0ZNSXUJPy96XX4PETDXU10tdcQ7C4+8UQxV2Fx21CEDVdC4AkMBDe9UEPqTaQ4iMZDWQwsw5Dlk3kN

IB8sHdqxZyDc9rkRnYk8gIAdwhh24ATkI6izoKsle65ZOYApzXABjRcAv8rMKVlB4vOVVkC5zDYxjnAz7ligHhthJl4kQdruji5esuZ1lj8CuUWFK5ymH1mlegoOV4VhEjVrljZMjQzqTZpkTh7mRs2fh7zZhHtKLLZGjSODaN4qFXInARUFq67ZRjbqLS5buYaL/ADYBmD/8Puddnnel9qlHXej2cHmFZIefjgR57bNY1nh3bLIbZ0CiFNadI9f

WBDPgkBiYaqZS7Yvn15zOI6i6+leX9gcIDraBo2CPvO2MFWFY0WCxUrA/SWxUOvVsNQAUVAfUtDBYKz1qAgwXYDumR0DyV6AKtIjIetv8qBXoGacJMUt6KIAS2ig4sC8gZNsw6MNJN/xMjPe9wkg+KrWSeax1EAXVNtVRFGzZQrllhUVeAcjZLaxUIth+hj3xl4VK5Q1KaID3icQNY8+z0cqJak2TVAtvSBcZs1ZMUG9zOGwAicqM2wHoza3dhBc

pF+djNRjwQOzzYF1bYv5mJe8oQBdULQLwVM8VDnCVKKIdQEWflBPSS2j5ulEsIAzOBbrR0FXxm1Udj8cO3XDmLsGm1l56kPExize3fMM/grvE/LrFUyM7Ptgss0mouA2QJ7OjIeAI7MjOE8bkAb1o+OpRQTf1YEDOAqvYeVCKHACCYP5UhqCTwmX4VIJKmBHaFUFww5j+Bxge7bfDiCb+ZaC96Iamf2uzuTfMNOQWgAoBL5mgP7OmqfhSpQszBTu

u0WUZlJCnfwJcDEqbw1FWqpTICUoSyRxNTqWYmWKVXDEfT243r3KCUyGtiWGmaSvU0mDMTubaVJRp4WBD9s/qPRVho6MUNNDsFFRoAu3TXMYzSSGXnCSrrXkoflcFvdahzFfYQWB8JAJFVsD/dZwNbtNoza1h9GEbMjCEJfqdzcDLeLKNdFuJTvOhK5UXuPftZSGgBeOCXXcIjgzc7E19qiLGaUxjCLLdQ9Dx8IvOw9E0G6NijlAzR1ejNeOVZBw

zgM7WvdIEynTZ93mWMhmhkiW6T1xUPkBao9q5hqPsBcdKJApdHwUyBogJAAoCNAynXFSZ9wtkzIL6IenAWjQOLlrydwUyCGWSAsVHrMzmVOdaE0ib4etNQGm096YYIu0/42Kdi1likGFpgsSWnTygOdNlWhlFdNlwN0wqoUK5tIZQPTsVE9OcAL07vPwl70+KOfT30xnkvI+ev9PBwaIHiwogIM0IZgzY1MOZ5t0M2CmLwcM7MhQAiM8jPVzcwxj

NIzLyM3O4zLFROwqKhMy8OP62paTNW85MzJ67VXY70xk19MwwWMzH8MzOcAH3HZUczymdQTczSebzO/JAs4yW1LIs46hJLEs3XBSzQxrxOXzcs79yKzV3crOJ+qs+rOi1l5azPazBLltwo9Bs9CNy9xs+ILALwcAi0CUlsy9DWz01PQh2zkBg7OPzaAL7OWtp88ktrdHs/iCX4IzD7MoB2MlCWHqgc1kDXLDdGHMtVEc6clgJMc2yOhUnSCDQJz/

I92ATzj+mnP1mGc1HpZz1BJiQP4ec2IAFzxoMXM6Qpc0n2Rt7xpXOhUCXWjP7dzevXONzzc+yV/l7c+w4DjXcwWm9zv8P3MkcSdTWrDzaIC0BjzQjiCZTzKyDPOqlc8/EILza6svPPFq9bH7rznqqsWs8gVeAsUl8Ex71GjMpofOVgx8zMPizOKxfOPLRiq6PMtd8zHoPzMY+QDgstJe726VH81qXWtSquH3/z7gYAtWjGcyGMF9pk6KuGpkC1+1

SKMC0FYpWDRvAuILQywBVDDqC7VXoLxkpgvTd2C9yt4Lrce6OELwE8QtkQpCyeUULxw1QuhtGJmHxx04Pi/7qUI86xku1oC8y2L4fMuONcLGJR25kAWjsQACLQi6Q7J9YUuItE1ZsG44ChXxQtzRi8i4oso9Ki51XQ53VRJYVDASodqDVNQ4alqNDQ/1g9swECpRbTB1o0u6L7Q/ouHTgPkYt3TBraYvmLl09dM1iWC5a2OLk/V2OuLcE+70eL7L

V4s/Tvi39M2tYBIDNBLUACEthwVSuDMRLUM+skwzMS4fVxLCSyjNYr8q/MOpL6Szyh4zrFQTNqzuS+Aj5LMPYUtCtFMyUvUzZS/WVN1VS7elqgrM/UvXFhBUEDXsH5nzO9IDBWVWdLosy+tuzGM/0u62gQM3Ok+IyxPhKzwxirPvMv6x6Sazsy7C46zCywCOsLkLMsvsjcVAwUmzGcxsvt6Wy6yFcLqLvY4HL+i0cuJWpyz0s4rVy17Pg1dy/EzN

zEBUHOvLGq12VrmtKZHPeV0c3XCxz/y/HOJz9WCCvgIYK3JmR6V4dnMwrTAHCupCkBoXMiKZgSXPctqK+3pW6GKyJu1zeK1oAErbeESucAHc6SuMg5K+ckFqYktStFFbjHSujzt8kyuTzT9b4Zsrf/RO1crS88HQrzKdCNQbz6LCKuHLbi9aX7zsldKuyr5y70uqt7qzH1utt8zwY9FkbZqvPzOqwyPvzGdAau2jxq6Q6mrXAkAt4uJvXKPWraWx

AvhUUCw6uxwsC8Fb26rq0guerhSmgt89GC57Qrrm8IGvKbkBkBPB0sa46sRrG+mQvRr3TQtuxwNC/0pJrDC4dKfJQiCwvU9Wa/jReUOy9wt9uBa/wuCLVsKWu++5a+ISVrmjNIuQ8si2PANrWsE2uNAqiySxbuQdoTkOhbYvu7uTVQBQDiwRgEIAAxbyBQD869OZyx6a1DacBxAoU/rrhTgXrxHwMvLJt7HA64FVyfIkU74SSRFwMkCN8KOqcCea

3DQzj/KKkTlMQegjflPCNyuUVOq5ukcir6RdOr3Z1h0cvrkRuuKs2HTeXkWbk+RKQHfz+Rq3jo2SMGMFmCbge9vtkQolDTm772v/FGGbgnULxGXZyuoKptal3oHlzTy4CHlFsVwORKICY8u42dsC6Oe1GGFAM4AHrQQG0PgQHW2Ku6rE7eb4zm4MZMXqQYgLRv/pXsUF2xU7ExNCAQLg7JNwANUoy4/TCZaVikArTUPH0+zxb4i16wJn/oEd+hZV

aGUVtQzQ217zPYWKLThYQnO1LC1AUMkU1JHRxzOISs08Q+yxBth7FPrfDTtKyI8OcrY8KBWfzO1X6N4A2Bb4ZBdtvYntfjxAFXuoANe8hbZwzi92M79CSdH7eV3C44waBzgIzV9lcI89UzmAkz7y6lnsFSHwtbpZ+ABlLAN4va1Bgdsgo9O1IhW+1VTNqZID4WS3v4yh+57D6D/gvgYTQ1ywVQb7N7AnXOdNK9zx97J1sBgYOB/exasACzH9iig4

VGq12AbqDlYJUBJFvCmZ/2Z+Dmm2kAMOSmCJL4AWMAMapAvIHkkkgtGcS8uZXt1sR/tor0PFpsx6jnVxO9uulJ813jREM4Bu6rTUkzYymIMMNukoyAAAGQXUwcYITBx0z4ABtXvBsHI8Kcs4TTenNv+C62361Q0c1eFRHt4hLOAh7VxSCQwZUW7U1UCfe9weOA3vHiDM43gMQNKKeaqoJx0qA58QnjnAEaAsVXsQoect886QNYlfe6emidHu2YAK

1C9bXowDEHffCxUm+GBOMd7ksx2P6AB0Ac8DLA+lsr9UzH3vYQxsAx32UgfYuM+8X80au/zEfeuZrjn/QgXYxaCgEtidEPhIVg59SctuGHZ3VTADWDGysggmm5XTx97jQIOM41OsOchvLxyzxOOZ7zBFUdqo+FhZuMrpD8ugrV+TWoouO8L0Udzn5WW3rJrPCMUN1uRkLbYuzxubttwVu60N7Twg5d1brDI07v8BSNK7v4l7u6CDzLXu/jI+71+0

WX+7ge8SMyHJTtBUR7Ue9yluVcewswJ7n4D3sp7ae9YW3yttQos5wDtbnsxF6a3UeBNzeBoHF7Gm6XuQ96lLTOn7fewPtDx9e383Dzgx8TMYVqLe3twxne2qPd7D5b3sIFoJ9DzD72PVcHj7yXXCVT72QDPuj1xBdxNL7hlCvscJ6+zSvjMTeDvuTF1A/vv0bh+0hUW8p+xQeB1l+/JR+7t+xGb37T8o/uUnlFYnVFF7+wgWf7xqvkjrtF+n/uaw

vh8AeaAoB1PrgH4QJAfJSHhtLRbtcoGKf/ODpEgcsEKB2gegkGBzCwd4dMjgd5AeBwx3ZwhB+0fgIRg6Qf2TscPUmTAVB/pY0H7ko9z0HKa8wesH7B5wcqHvBy+D8Hcx5uOjDYa775oAsM/iUSHNSlIfB7lI+AbFjbyZrBmHO48EcIFKh1BrqHygJocig2h0A1fNqAPodnghhw7UmH+MsmccrEJ5Yd6Z1h4UkPyGx8raOHCzM4eGBbh+ARhH4rZ4

cMDJljKf+HsEw7uLHShwgWhHqbR2cRHlo1Ech9383aMIADo6hOPtyR2ECpHgwekcsEmR4XTZHufbkflNk4AUefl2cMUcy1jvGUcVHihpvnVH8m3fhoAUBeBONHxqs0dHgoyG0dqbJllgpdH5+Bda9H7Dv0c6xgx6ATDHRVqMech4x4UNVBxQ6JZgcOnvUF6e36vqk9rw1Svb9r18JMeW71u8N127Am4EfY9zuyscIF6x57u0p1sd7uiTvuzftQAA

e9kCuDRx9i4nHd8mccB656fXnx7xljcfIndxxj4Z7vgH+XZ7rx/rHvH+exfmF7Px8VR/HzgGXvhW5s3TNN11ezpC172cOCd8ykJ/ynQnCCEL1H68J6JNd7LF4R0onc1WicrIGJxO1YnPgBPu4n04ziGz7jLpAXwjSNCSdkna+3ydFFVJ+WA0n15XvsqIB+/7UhFJ+3QWsnF+3ZJX7nJygrBUD+6/uOXAp6Fc0VLl7KWin3+xKezId7MHGAHsp/Kc

WxINArHIVG5mqewHX+1ABaniB/k26nqB+gcOwmB8aemqi52acin+ByshWnz54/q2nmlWQe3RGsU6fUHYgm6eoAHp4weoALB6JNsHnSBwd/6fp9WKBns2zeOhnA8eGf3rspVGf2UDENIdxnHSlMkbw5Z5KODnc1emdqHtS9mfTBElHmeLBhZ6h0sOxh+ghlnni7PNrXDe0Ij/jkxTYcPG9Zw4cNsF4y4fPTS+h4cpMTHVu09nc55hdctIR+2dy2H8

9wOTnsR/aPxHRNmSXrjC56XppH1fT3hrnmsBuehmcV+d15Au5z/37nJliUdHnCBeUfWUlR6T5g5F52XnXnjHctZNHxoC0ePnhLNacQ1aqt0ebWFdX0c/9Ax/yl/nnvU21fmXDk5PR8cWSg2A7OyvEBQAxAGsBKw1QADG+hsOxAAfQzgJBSHArwN3wx4HMDmDlaXORzC8sLuc2DLgEu6sC8RJdrruL0WMLt7xeHZOTtZoSkTLl8NUjQkTOueU0TpT

87diI2M7YjVVPDZVXgPZs7FXhztYqCjRZFKNTUzNNEeajebnoAC3qXI4S5DAFEbZgsNjtIocwH1OkSJkNjhRRUIArc1yoFGrtceNjRd4LZs0/aLXA8wJuh4oWwMtMKMqAs25SeEgM+3hUcpzlYG9RDrbv7T+ixLWNA+TIZTwz8S4EDvrqx7KU15zoGgAAxVOdUA+8KBTaPWldwggAwAdfpMX+FNm+KBL+c1R5RqGQFyaML3kxaBWj3Vza+AjgWII

0BfgZEC8gIFMR0kgAxo3qvdXlspdL6Hw1pQa1zMaMhrGOo7F48f/EGEWJLVACghE7T3+JY4AWeT8kY4IQZeTKfdL9SQUBP5iflEBx7owkpVJMLgNhBVl7oJAZY97d4+td3WMz3eMFnewPdD3I974gL6oyBPdT3CBbPfn3eoZMWGVC1TAA7BJD7U6TFTNdQ8IFU1PZDFUt92vf4lPulBuJSzoGXmD3jQMPeGU9whILWXW7W7b0D9FTiNdjI9yg8ag

aDwgWEjXD1g+8PI93cJf3spcQ8sPF94wVL3viCvfqPpD/iVzXZFwQXd6Bx1HoIFhceRc2bFj64MIF8TOQ9kXmGGXkjglYADFfgPvOCROQI4KpBYgTj4BCdeI4OCSxU4JNhCNAbkCODVABqGRBfg4VADGVggEAE+oA4JJWAjgtQAajyyjQN4/gkKj5o/tgdj99gEAKK9UA73e9wfcIFsJ8ECt3WQLqjcP2Dx6A+8ETeQAIlq5T035MvD74NzVZT3K

iMTJRgo/D3tT4ZT1Pq5qARY9rT81RCIzjy8gozefiKBiPwZ52B/NyDwjOoPaS+g8U+TXdU+KP7d8o8IF5ncPc9PPvAI8t4pg3NXTwXAd9r5lQiLo80P+JbCNGOWPZc8UHOILsWZIZoK+C+IjAEH4O0QgFk8U+FcCiAGQ4hBENl5Vj0HvbPvHew4YFzj64+GUQBy8iNAST7FSRU2ELC/wvqAJ4+qQ4VKpDfPux3ygCFyZTDDUPPvG0ChADheqGBXx

W8JV1X4CDKe8u+/llJVA1d7XdT69dzbuzH5myZYVP1+As+d30j8s9H3V+P3eoAPD3w+j3eD1bqT3WL2o9ztrD7KVaPm8ugiXPCBRvd7VZeYU+73+94fdzVx9w7Cn3MEAq9zVV968v3P9SY/fW1z98hAeU70x/dtPkxT/cQPMAP/fn4aAEA8UHoD3Uq/3TgLFTQPz7JUXwPiD+DVcvT68fOgvXT+s/CvuD6jVivhD3NWSv89xo8U+dj0/JUPer7Q9

z79D3NWMPzD1K9xvSzRw+348j4K/YP/D3cKCPC+0jQgmIj02gzPGJwG9LPQb3NVyPdcHs+bPEr8MRpvNr7VbaP8r1m96PspQY8WPQLyY9wAZj0yD9vxjxRcgvi9zk+95gEA48QvLj248ePXjz49+P8T0E8hPYTxE9RPMT3E+BPSTyk9pPGT1i+2PveXk++ANm6q/FPGr5MUdPHL1U97PfTwM+NPLdy0/VA1r/iUdPHbXe8Fvijw+9Y1jT8M+vvoz

+UUAxEz37HTPY1z81zPfMjW88vdbza8pDob0o9YvOz4h9FvEgjLPbPehQQBnPRem2/XPzzUWC3P4NUa8axjz0MovP793CUfPwGFi+/PUAP8+GT61VAaDvoL21ULWTj/O/QvlYMi8jgCL9hBIvcL7x+ovqkOi+YvCBYFe4vmbAS+GURL4ImrBZL6qt71tN9S8trYFyO4QXkEfDk6pMETBfI5Q1eUIjV6OVXd/ENdylffgiiGhdN3wPY/ocvkj4s+w

fKz+hUCvQrzg9j3+D+K9EPrb8m/f3Hb3K+Orsbz2+MFSr7i0Xv6r0feGrJ92ffefl96/KGv3bxkMP3T97YUv3Fr6889ob7zK+20drw6+APSV8A8axrr+A+cW1gJ6++A3r3A9AwCD/otIPHd4G+OfmD9+9hvbn5G8tvfsNF+MFCb5Q8bI7XxT50PPX2ZdMPhdP1/sPP1czj5vLn2h+HPQj+W8XGoj7FaLtMH93eyPULeN+FvTkFs/RvXn/F82Pvnz

o/bfxzxA+GPbX8x/jvxI8O/EAo7yd93wZ35O8MI077O+ovkLwu+eP3j7E8rvgT8E+hP4T5E/RPsT6u97vqT6pDpP/j0e9Tvuxae8FPRT2F/tPql09D8v3T4191sdT3++s8z753itPWLx++iTC1i5+/vDT6zwAfYkmPDjPkz2HTgf+i2V1QfBsIt8yPt3zAWrfGz+t/If4Xbs+I/k32EIYfxz1h/4AOH5kj9fNzxwB3P8Xw8+95zz5a9UfJ1F88IF

dHwx/hDWk2O/Xfpjwd9sfjj099cfPH3x8CfKL2i8YvWLxJ+/geL/5933GQ7J8kvyDqRfkv3RZS+JXQBzS82hMWbzcuTTnqg0URyWegBuo9kLgC1AuAH9jVAf4DDu6a0t6MAd8FZLlAaiGMA4TMwkXhsCL07DbYT1gfwJ8haiJdn8BJQ3wsa5V29spEQBEOOtqzU7tt91lCNjtwzuPiTO+I16RkjdTrSNOud7fj2jXjNnNeEAFG4qNgoC1MC7K2Sk

C0q1uZHei7nU4TDxoEKLFEVa0UFvbhRx2SChGujuRNMV32d9NMt/1bhlFto3DMuDN8Ru9Kom7Y2tfCUd3BWQCshKMMxXildRTNVlF+dbxWOfzs+DJl5FEDBCf25tPmbILDB17ECGLMp8XtOhdaMhuqfe5EuDH6kGGe7W9n6ksvYvXk8Hoy0LVExBMELlczjMm1QnB/sXemgA78tU4reld1r5nTxTFKGV9HF/9xrsHQqltUdmXiq9AhjUc3MnR9vV

C0BrTsdwNhLBsK6vEo14MvFYigmoObpxATrifl4lJ3A8QCiBIDC3UZiqpUi6mVVJtm/kteMDVapGJ11HL6tdDGvAgxk/pDKAr44WPZIWCD303kAgsz1ONssXvON+gNssuWgtZZjpSYpJBEdMgHGBsHOiB9zMjJfGKIDmZtxAjIIIhnDGaEP9mMNnAJWBSkCsI82iYZFzmXkO7m8glnuq031ljMCmH0wRCo0Fq6p0ghZofBMNrhs3OvZJZKMYC1VO

6Ry9M+BYAWq00AES8imLYITQhiVOIIFcpmn+kCLnIJptqck+9ncIMVgoA65poAp8loB4AVUpZwD5d+5nixRvgdwsge6o4SCwR5jJiAYYiQCdLmUVLKrkBrKrZUmqrPUbimVVwtovV8zCvVcALeVCVm3MPNlXVRkD30igQ3MtAH8QH4r1ZG2iB1x8O2AOzt/F7iL45Vrv8RcOHWdaNmoBR8j4YDSIn4QSNvVJklsCJ2rsD5lsVZi4kcDT8D8Ud6sG

dE+lFJ2zgoNqWqU568sJBGIKhBrSO/8DgWHBc5sfU/aFuwNaNkBTfP0AVerH0alpQD2ZtcVZ/Cwko2oKFJqNTVzhsONt5rFYTNoCCyqI59FVoqFlaEDdzVvjJIeA0cGloVsb5jAA+9sJsjljZsL/ubRoFE3taZK/tUwMAN3tvRtY5oEAbOgQA+9uFR8jPhAlqnpd4Ad0tWxkuNBAZgBlzByEuKHnk+9t8hW5q+x0XIK8n6mgAZgY3NVkFhxlRvFt

nikgl0ENgJmABwBwSPFIUBND51DDccb/N4kzmm/U+gdrVFMoSk+Ktf82eHfJWKl7tigpY5XYJpkCUtpk2hGPB2lItVlzE2kC0tC95fDwAFAMslMMnxo+olw5oKiopMCpPB4AF296+vrAn5CvdgDDyhN8IZRAIC8hRQA3NXkCGD/YBbFlrIZR4mDKZ4wTGCCjK0oogNDMorowVMtovoBfMoBa0CnQDWudIhhoiYXwPXky4AsxvsOe4EzszNzAOART

pM4Yt6sCdcbhwAQOivBT3iSku3qpAgZstZiUvTB+ShQhw9JplC4gRNVHCy4QwCldkqDIA2UN4k4qM8Zt/kKQCSJ3B9/unUk8pnVpqgp0T/jxVWhOf8m1A5Ua8OHpb/jgIFulhp1KPjJn/nmY2nGvliNO/9SNLGAv/jet+Ur/9Jrv/9uXoAD8ZMADUaqADk6BACljNAC/nJsYRTnAC2WogC1Qf4IUAY7w0ASwQMAdBUsAf4IcAWDk8AfACCAQ/MiA

RFUnzks1yASewmeKJV2KkpQaAYYs/qP+cZTIwC8kMwClKKwC4lhwDIQdwC+KrwD/VgtZr/mWJMNMICJFrc4fuJG1JAdIDDJONsa2tr5FAWnRlARlVeOnoDO4BoCVXmFZtAXiCm0JbMDAUYD2pGUtbnEwYLAfiArATpdJivt07AQ4Ckzr/JnAcJlgIe4CeXp4CUlt4C6aL4Dv8v4CJKoECMNsA9QgTLNwgZtE9ITWpogWFY4gShVVuMkDkhEQYawK

RcMgVAlEUjgsrQEGs8gQUCigSUDNAGUDDKtZQMXH9JuLDUCzMvUDxkI0DwWM0DqxK6RhzB0Dcrp+B6+jPUSQY58Jas5UhgbysRgbwY3NuMD0XG4xpgc5sdAAWdp8A+1/4MsCiAEqMiaA2xNgedd2VpKMdgXAgNjjDc/gcdRbgScDMgA8DzgdsJJoXsDb6rNCwQMcDlzsKIzgThDRTOfhy+q8Cb6u8DCOuDZ10D8DZuDNCbfLCtMQfjRvYCCCwQuC

DSQefkWZnUsegQ0s4QQGl2EOoIkQZGMH9Ms00Qfosj6pHs/aNiDEIeIFozpEdr2oSC2EPQN12GhC5gm7t7lnbA0Vk7NzqLSD/wZflMSPY5EAEyDMSiyDQ6myCWJnWpOQQgVuQRKC+QTJdkLAKDJ2pTDHwaEwXcGKDYijyDJQeoUWobKCO5tZNbIUqCtACqD7mNW03KpqCykNqDdQfqDB0IaCuzNyB1AKaCCSllVHPq6ClMlf9w9AMBbdJ+BHQSaF

nQS+B5YdaDoxF6CFSj6CXgn6CsAoGDgwaqYwwccc5qvX0owQmDYwSooiwYmCOqMmDK8mmCMwaKAswanRcwZXkCwashowSvdCHBjDhTnNUqwTKYawXWCu3hO0H/s2CYem2DNYB2CsQF2CEmFPohQIEB+wd8VBwXNUjHCOCmCrpl6wcyYpwSvAZwSnR1JAuCN4kuChnPpsFDFyd1wepJNwbSBtwY5MQLmBFShrDkFnL1VoLl2tYLnmJahn2tRqlv8S

2rv9nKEeCJqieCJSqnl9Fqf8rwbhcbwXPU7wTf9Jtvf8mwXXBXwRS4X/h+DPqqIYyNH+CywT/8wAZClFQVI9QIXZJwIU/JIIbY5oIVACoxjACEIfECkIaTIUIdtEVVhhCe8FhCLYbtDyolUd8Id19NxkII7Nh9dlrGRCfdBRCdrL0VqAR75p1gxCGAUYcWIWYA2Id6p2AfotOAW3U5impVeIWWpFYUSCedERkX/CJDO9GJCTVGlIpAZED4WKYD5A

XJCDkApC5qqoDlIcdM1ISN0NIRDDlITpCqbIQjZAT3hDIdIBLATyBTIfiVzIfYCztk4Db1q4C4lvZDAAQ00vAWksfAe4A/Af1UAgWHAggV0t9ntLNBlr4D/IfbxoIARZiOCN0QoQkCwoVKYIoYzd0gdW0vdvFDzYgjD8SvkCBQGf1CgVoBUoelDHQJUCDnNUCt8lUw6gRggGgawiioeGZWgWVCJ6ucVp6nBsHKv0C7ik/UGoTAYmodgcZQcStmAO

1DeAtzCuoQsD2On1CtAANC1gW34NgaytRodFsJoXYdpofGVDgRtC7gQtCdobusLrtj1LgTDc7knNCtoYtDX4c8CRzodDJWsdD2AKdDvgVQpLoQCDgYWVRgQUjUHoQVY0IVCDXoTCClJHeZPoY35apGWDM9BJQWOgDCgWhiCOkVc0r4dlVHfBDDxzlDC7JESDYYV+x4YeSDJ4S7MqQajDdKOjCt4fykGQbkhcYUsYlFmW9fltQQOQfgAuQczCKYXo

VbIaLMhQTe143IzD78OTDUwFKC2YZEj5QYvVFQZ1DeYYOx+YRqDV5nEVlECLDDYAaDOzHj5JYZIBpYR/U0yuoUrQe6C0EXaCVYd8UXEU6DXkkTQtYaiipkLrCiAEtVXUpCl/QRHBjYaHFswQ+l74JvDQqFbDiwb1tfEaFQ7YeggkwXyUnYemDMweIjh8B7CfeF7CWUSWCAKv7CKwRT4g4SP5awRNAc4eHCF4dWJWwbXpY4fHC5mInC+wb5U04ZMU

M4QdIxwbODGUZODT1t/ls4eggi4WwUS4fhMy4R5IK4Sgoq4RQga4RE5w1DuDEGs5MuOPzdEsq790GlWwnIJMB3tAgBlgEt5ugNpocsv6Ex8oTsYoIHgXcjWQcyAJhCIMF44gBxhzgHmRsdrYRzZEWx8oD8osvPHxNwDmErblX8bbgV5C/nTti/oVNS/i7ca/hX8KpjWFy/q6x6vLVN6/vVNG/s38rvEHdEIiHcq2CkBftCLtbcs2hPCLjg20FuAE

7vR4TIJuBeIkNNf+IVAPgGDB/gJY1TvJNNZ/rx55/ulFc8E1ge+GERdZINpjdp9lTdv1gr4jv81mp+BQjrstLWicVcQP5IYGo3dIQhnlGTOWBlqHnlXqmgBemnUplatDCYAIEZRkN34Boq455gdPg/5O6ZskDQV2qIsFxYKpAIEOApSBi4D8/D75fHDkM5zMep24A/hBACQRr0aPlxFN0iXaIaiAKmD9JwKU5MbPwhIzCsJQqO7sP8oXEU6JgV/A

JrAJ2rPsCALiFQkqrAsEZeNwwdNcG9Hejm2hiVcOgtY8bphQz8MapJAJAVJFg3RuQL0YcBOtIPuN+jsgLRpU1vykAwHMwiOCGBQqq4dr9uhZeQewFMJkB9X4qyAYECKAmADtEm6onFZzHxYR4UDIC4HXAhDsHB1bNXoTKoM4K8lwEbjCQBUApNUvNt3MU6PjJSgqjRPpg6UdqnaibfE3oN9PgBKMX24CADAB7EJ3AuCgoBulsBBQ+Jec/TOpip9G

oDKYH3k2OkZiz9oM4QOhXAk1C3sy6kIQPSJ3AzAFRZsZB1DNALMDumlcFt2lYCUggU5Ebg/cwQDXsp7mHttMTkBkQSohf5KJc3nn0ImqkZAxJGRBgsecYwsTFUXzi4Z05mPAl9h0YUehvl7kQbR3MagBBDij1N7olY2MdyUOSN5jg/CvsWgG6h3TGAU89gIhQzPy0clti9g6KjwmQG4duTrDFrSr8U1jH9RXzpJUzTjWssMbKlepAeUHMbjxR8nf

tDsc/IxqO6oYKvRspsU/ClDugBSFAugt0Z6QiOHujcroOxDKIeiyvs1VT0fRUV/BeiWAFeilqp6Uy8nejZCrSE4NJapn0WzU30dWt4kV+j5Mbek/0XHQAMUBiOFuz570uBjKjD/1DKqepoMQR04MXrC64IhiqlMhjtUXTY0MXd9ditWttMVhi+HFGY4ggziJwEzjUXszhsctx1eyhRjg/O74MIlHAXrubDz0YxjFJixiEcSed/BCpBOMdxjiarxi

ekJrZBMXJj+NAupdtg3E+0JJieZqMZAQbJjE4PJj10OVQlMRApMAL2oOxupjSAJpjqsU2YrsXpjipMDJHgbeMTMYxAzMRfoLMVEDrMQj5bMR/lvNpClHMfQtjaC5jwZtuCPMeVZ5sQQUO3H5iAsQAh2sSFi74IlYWQMEAosZkB7mtaUobPFiKDhfoksTyB0KgI4KXi3lpatljHuLlj8seKEiseNB9pPyljXhViZLlVjR8jVi2ytsgGsWQBD2iVQ+

MG1iOsV4c6aqnNeseCt+sT4MMEBcit2h4FeQaNiIzuNjXwJNjNmuFiZsaXk5sT5jFscQBlsSiBVsXxd1sUHBNsRRt0gbtj0sY9jhjI+dfGkoozsS2ZMMd7Y9Ma0s7seoAHsQdiz8a5ioijotF8Z8M78H9dvsZM4ExI3CYclqlVsNBFEcrp9DmPp82gvUMe4VUA/seHUk8oDj7mCDjIzMei6bGvBZjpDiY8aCUacUSitCu0UEcUrVa9I+i0ca+ifQ

JjjP0drif0V0h8zgTjdYn9RicZrFScZEhycfcYqcbBjYcepR6caStCMahi6NHY92cc7j7jFzjcMYqZOCShiykMRihcYhNUSqLiCCuLiaMVLjqLvRjZcV4Mv4Qrj8bsHRlcSpRVcSXAvcZrjIeEzxhMbrixMcwJlIKdCWlsbjgYabjhMRbinMSwBlMTbjVMV8UNMY9s28fwS5zNdjltCVJDMbtChBHxikLD7ie/OjErMSckg8UjUQ8Q/j/yr7ECqq

Xko8WNjPMX/ofMQnjjEEnigsaniusRtVIsaKZs8SxMrqvnj6koXjAwMXjA6mljy8VlioeFXiYkYCjCsax1isQ3iysWkVHUM3j4ShWD28XViBlI1ie8XZQ+8bFQU8Z1jkKlfUesWAg18JANx8YNjWQf6URsTSi58RNj3sUvjVCexi18QtjlXktiVsYoY1sULB98SCQtsUfjTeCfjn8a8tjsbtcHEp0dzsXwS1YC7iU8shtyDntIn8cFdrSlHj38TM

TP8Uu0/iDzcpNE79icgLdXPBAA2gOFRAIPK5lgI6gCGtRR/UQzkPhGPkVgLYQX3L8J17NZom5OZpo0fq440VjANwImj8dt+4LgDFBsYLih8UKVBJcnDBeGnn9+GrlM80fbc27O65C0f7Ji0eztS0dWFrbsztlvFWj5GlztGwnNkZ7C39G0dSoVstcAOpoDBCYMcB/gEXZlwBVopgHR4FdrlgUUAXhgUO3IS3OuEXvEKo7sld4F/guiWYEujmsCuj

CckNo3GuujN/lUAgYSfUAEOdMZAMEBjDuiADpi1REfJIlaQByZvBs18hwEaTLSUxD7jFJIEtoajESkEjF6kltBVn3V3auTjbxqwAvaAKZxKmNjGCgkYwcYyigYiwRzEZYiUocqD94BlCfLgUEn6iGBq9CnRUJnSVsWiho8qLikloVMxR8nTRxojkD2mIs1Kkn/k0CvISlFHMx8WoGU78CR0PJEWkqYHvI/FlIVohhNBIChhEajjGNPKhz1/EQZQw

9hfMP2JFImbMbxImAMNuyXSN0ydaVQjMrxlhOWAEVo/pEunS0ykC8gm0LeDhJIydmWkNj2ir45Xps2NlBDzYjYAOMYEIbBzScaS4EDKZlkGbMBxiOD4xuw4tDPLAGEI/g7ar8ZsWj0lgybWUj0ROCkYYJsFNgCtarl9ckrvu1j1D6UZZCjMRurANlNlSYa9HC41YHMZNqBtJPlm4wektacoaickgtrWZZNk/JVFgBBVeHqS/aLbBDSRaSTSWaT7S

SaTrSTD1RXnaTCKWeSIEHOZnSbyswUSgSt2iyse6kKst5j80WyRmZ/SZZRAyZMC3yf8QPyY6sIyT3goybgArEcUDYyeUDMoav4kyVD0rAN+YEjl84GRpmTc6NmSMkYodcyfGV8ycGlCyZCxiyYXFSyeB1DAhWS5oV3EtDuWlyuubExJMdjyAM2Tjom2TwFMTc3rJ0CxyYMjeyaPl+yWjC6BsjwRyRVCYINW0Jyd9xmcSJAZySwA5yeAgFyWr1Y4M

uSaoQ30x4OuT8YQEVs2tuTfrnuSIigeStpJkYTyQ6TACJeSymv5JIHC4U7yaNBJ4AuAnyTpZrSq+S58SGSBKTqivyWVsfyZpsgVsp8AKQ1dgKWmD09M+BwKV8syClBTRkqbY4KXfgb4pEIIKUhTKXihSSMldUMKeSDVPnogShgASeqlp9gCTJY1nOAS6hkQQoCUe5ZkfqT8KVlSiKZOtdqWeSyKUfDZjAdTPfBTisbC6TwccysItgKte6pvMfSex

S/SRrRuKUaAgyVVT3yWGTRDkJS78CJSxKalDiOPGTKEjJSUyfJSIbq/NoqspSdJNb4cyeIg8yY3RtKXFsIKRM0SyT3hmzpDU/qJWTjgaZSczuZSOKXkBGyYesmeHZSHANGZ6qRSMuyX5Seydsj4yh5SDkV5Thycl0ziv5TdVlOTgqZIJQqWy95yUCslySuTp4WuSgihuSxiQvlkqQsc2BhO19yQzijyZRTTyZ74LycZi8qchVs2jaV7ySVS6kYGN

oqpVTprtVTPqfsi1YN+SxAc2ZAVnS1mqX4dWqSvAQKR1SzKBB0kadXppevvFYKdJR4KUpsvlqNSlmuNSP/JNSXlphTXifZ53iY6E3JjspJAAKAjAIkA5ZOwVJboH8ZbhCTDgL8IcYDCSGwHCSqGqgBKcBjB3gPjgqJLYRjgOqSIACn8VSaXxbCPLoLONLkMpnDBpco645cvSg7bpWhidPTsKSeTpItBWjA3KzttctSTK0TVMmSXVMDcjztT+HztT

csdhm0ZoBPCDyT5iAlhPCA2AcwCcAdvNFBs6cOi0INMBjXBjBJ0bKSvsvKS7GqxJ87ouizsrKw1/h9ly7tHl8omjZSAc4BN8hhoZmpNt6ADO0PNr3BJAODEsQDCx4rn6tUEawo0QHTSQCPrQiOFeT5ZjXB8wUMNa0PPppRolJ9sXfJICsoJSVuCRGfNxYV4DelCnLOAHyr+ikEbABeFEpB7qB3BcQAbQGtv6Ie1MJDfmkEALQKXknOsliJlsdUti

rmsk8lesa1oMgvvOoAkjEfpyAPvBj6TrArJCMZ2gT4iL6VK9alrNZXKTxCOAGTDGZBVQfEWzMriuuxI+k5VgkQICpkIfsSAj6I0eMbA75O0j9SdiZJEXzEfYGhUiHKuYR4NdTBgZvUVqPNJTJl9CxAD9CJkcJomzD/FI4mE4GaP9wwaIfAD5ByB6GZvk/iPLB57hwzBGb0CCEBAdHfFNEPEkaDKaXZNyDtUTceGW0RmKJJhzC3oRQHIDO4HXNBEl

MgyupzD5AHFRImVxjnas5VDKKbB7AAYcIgC+MsBk5dKlNIS/YMX0kqpIlnKLcZ8zJ7NNrIONMADcNFmirABKHojwgRgg6aNFJlkOwthQWeceIH8xQmZ3o1YP6TrKJzEDpNssFiae0SFgAYsjHTJz0uJD6kpmcdruOVDrihNPQadD3ADVJcmWL5gkWHAe9ovtiBnwzaWlIkIYew4x4FeYHbr2VBUtTxXKIsFrGZwoJzM8ZD6XYydYKfSHhufTL6Vt

NceLfT76V6oHwR2kX6capQ6O/SQidZR2eH3Af6Vho/6eTwoesJAtmoYoQGfEIwGRAzbjNAyfYLAyToQTIZ0EgzLKKgzmlovBqEkJDQwP8VfGngzg6AQyeQEQyN5KQyaZuEsKGYsFuSNQzObMGprmWvBHzJAZyoWcU2GU4ye8NVD4NlNdeGRtI+lL5SWHFwzHRu6S94vTJ24BIyrkFIySNDIyZAJkhroXMiYENgiCTBGsokREU1GcEYmKcEjtGcIo

YCmgpEQeMiUQWSlTGeslGEefs46GczqWTiF7GXFRmWQMihGV+w1YO4z61ASj5mWOTtrH4yVFHtJAmTvBgmbFYNmT3g2GQkzoxDEysSlNcEmeQt3jskzUmYIAizhkzMBsczqlgMy8mWiUmqkvhWQsUzUjCMxP6VgBKmQD1qmSkCVhK5DOkA0zy4IARmmTe1WmUaB2mV6zwsoU54TNkVIGe8xWQrGy5Wf3AYACMyiknyQTVBMztriR1pmY+N0mTrCH

WSUl62Y2cfccid1mWEzOWUYp+RmDUVfnsyWbDXTUSkczsmaczdfOcyHKDNTBQHNS21pBdW4ZUMBqh3CQlF3DEIohdpZHQzTWTcyxDk3opuqgi2GULNdoOoBnmbr5XmZNtn6XvBPmW/TIqdC1D+r9x/mQ/8gWa7wQWYAym2hCyFWh/lwGfrEDpLCytGF+N4GbNxkWSgz2wGgz2kqJBFalgysWSxYcWXAB8GeONCGWE4bVMSzemKSztMZQyKWZIAaG

Suoj6ZvkmGeYkVWgyzPwEyyYNnfhWWbeCVEvkYx2edimaVTS+WaIzBgeIy4qSKyvKNIzduLIzJWVtS/aIozJysoyFWScZENsqzNGQKy1WboyNWfoyxkciDoxLqz+UmYyDWbKZWBDYzj2QwyMkuay6OTyyrWZWpbWaGkvGV2YtRkLCXWQEymku6zTBCEzy2d/kImaB1omT/1YmYGyxWsGy3mKGyumuGzUOpGzXxlvtoVnHi42Qm0H6oUzT/AdISma

myP2RUz/unu0s2bUzc2WHB82cKBC2Uds6YVSdzkGWzR2RWz09IONemSvB+mfESpqMozhmb4hRmd35xmRrFJmZ2y+zEIgZmQJ05md4zFmcFzlmYMDVmcOybLg5ypkFptJ2blclFPsyySdhlo2QWUtOcuyOGA6jHfk6jXJi78gdke5x6PEBGgOLFsABHTzlDmBBIiTh20K8BsYIbsk6QiTY0RiTvlBcBs6Sn8MYKfQy+NCg80LGis/lSxq7OXT8/rm

jadqSSYPB3ZRGhTpG6dbdm6ZVMS0W3S9cp3Tudk2Ee6YOEm0T5F4oMPSk2NGhgUCiSoQBjAdvNrcoou4Qe+KsAisE1oEotOi/crY1twvY1B5JvTl0aXcusHvS8ogug6mnoxvkIudc6DT5EUX3BYqELM+WhD5bYKFianDM8qeRaDEYaQZksTLCiSh95kigtZkDAcioGZ1zCcdQRSiTvlJEj1sOqGnjmcF/8hAViyv2JdhzbOFjsYmfhcQNiVvnPnD

AwLtF+MS0CnGGaE1ao+d38FcY+jH3tagGRBsID7xKFsn1cNinDveo2URTJ3BFwaajmDFDZSVsoFjhipitHJVcayh/VcqhYwUCqtY54OZsAGbfhaoUTC6IgPd+UsC1jYBVBPVAoB3ASGpi8n9RctqJsg+YoJiZBkSMENjcLcfQd3GEqioag00nIADF59F7SJKoxCapLJliQbCDN4EWVXtnvEpnvCxjFiiNCanjSA4dhCStpQZJecvjFcaKYcDCZIN

YtyCcgMBA08H3lPXtit5hofSxAAUdf2XTyzMkTAiQQcDB6nPCuonISX0cJy0WMeSSKWeTo8dFcbqtejzfFIUmRlL1a9I4kobAxZRJBPjfSZZSnEtdUFqrTjX8Zy8NYiOAXkMW97KLSBhCAwUk+fMM9+bfgbFnHA/MTWMJ8V00/AElI+KRRBMCKbVkDE6y1MZtgXtmZy8fGlJ94ANEmcdnsfmiWzNIf1ygPoyBPVGcVPzDXz3JMOZGeeyZ1Chmp1K

AQLwBWU1AgJHAwuXCyErqARvNjS178fVIL1kyAg/GwBXAMXFJ4gwLmwS3hd7goB7YEARN5MVVfwWmdV+QbBSBeFj1IOzzi8ScjUQEKEO+ZmZcnGexO8KyQAkgzRg1GuoPUsvyrCqZS0DBSCpIDAFRBXIL1SsrBZkKsgSMdWSLSFgZwZl3hlYGYBSBpm0MqKi48KrYprDlD5eMkpyyqGIL2umRAsmbej8XIXAKNKyF7KJiAYWGmpT8FMgC4chVQEq

1UgcWZjq2iI5SfAtwsKeot0ACTz8mGTzjQKXkxnOaDUADTyU+XfgGeR3zmedkLrwZIKOAtkLueWbMNqkyAxTkntvxkLygqLbRteP8sxeeQdB8VLyTjpgj4NvLz/DOHzQhCrzn6mrzv8hrz+8FrzqxDrzKWqvh9eQlJOLBUt8SibyzeYZQLeb74reRDwbeQJkMHA7y5Omai2Oi7zMQm7z7CR7ypLt7y/TL7ymMQHzm7nkKQ+bnzeheut7mtHyIgHH

y+wKJBE+aPyMZrTzdQslI0+Zni1odLVZylnzJ7jnyQynny5ZIXz3GMHNy4p70y+WdENkRJUcBffJa+WT96+XdNG+auZm+TvtW+YgD2+WkSeml3zg1KGV6kv3yIYlkTh+XKtsNmt1x+RskCFAAyZ+QkL24PPy5FG8yNBVvABQCvywSjdDj4NLSHSVvzGCtnkb+dgS64F/zTapL0WRgsxj+bWZT+Q3yVbBfzFDKwoEknaVvQfQh6mg/yn+dGdX+aPk

P+RjNBRcoAf+cXljEP/yiqnix8WrfgQBdkVkKlULIBUoZoBb2zvGU/oEBXQyDaMgLLCqgKdmUT9rrgvlsBdXz4RXgLPTIYKiBWqYY9J4KNqpCsUgpowMSlowaBWvA6BUAz70XY4mBZS0VIGwLjqBwL8CTHpZRXbBGgLwLh7uCxEAOcgpLs3oRBZrBAxYqZSha0DykDILnAnIKGbjrxfRD5chavIF0bFwMl+cyLZjBj5tBRhjJ4awB9BYWLDBXbBj

BesIzBTAkaSJYLcOCqFbBW6LfYro4nBTLAXBZkg3BUiCPBXILMht4LvekY4cMf4LkSIEKm6CELMJtGIIhX/krbNEL7mHmyruvELntqot9EFM5QLrNTwLjtpyhlBdt2bJZjtKjlDPmZ5RsLhx0hXGBMhSzycheZ8/2SwQChWFiihYSUkUXNUJBQUSyhcBK+4BULg4FUL+ebULVDvUKs1tli/qi0LEth3zpeZ0L12N0LErEryVIP0K4mIML6AMMLyA

KMKXTNvAJhUoVyNtMLp2sbzTeebyY1pbyVUdM9XAesLg4JsLlwc7zMynsLumu7yAZlXtjhcT4WCH7zcDOcKZmpcL+gdcLBXhHy3DFHyEADHzHhQnyx4BqLLlnkLQSHoB0+Z0hM+ahBOrgCKc8eYBgRQXyi+eCKZ8JKsoRVgYYRcdxPRdf57dHXy/NufzHqZZT0RS/C2+ZotsRSvj1CT3z77mkVCRYPyYsbydSRWfNyRTgZKRbmpqRT0lZ+Wwh6RR

I5F+cPBNBVKyT6qf5Tqbaj3qfNUh8rvyDrPvy8WofzRRe2k2OhKLkRVKKHJTKLYYtfy0pUtUrBfUlH+c/yogGqL4yspKBRRlLv+XMk/+YMgVbIAKjRe0KtaRT5QBXswyBb4yoBQxAYBeLC4BY2DEBQ6LCjs6KIjp6oMBe6LPwHCLrJfTMyioGK/RZoUsRWaK/TMGLKBWGKaISXRIxVD1oxbIVoZKsh4xawLbkl4cO0jGLUxSVKeBXwLsxYILZhcF

UCxa5L1pWBKOedIKDwb0TYhO+dmHIoLr8MoLR0qoLGNC9VqEpoL7jm2LPeYjDOxYcEDBdiK76Tkp+xTxBzBXQRhxXoxRxU0iPVPYLJxZwBYCVItobjVFvocXRnpSZIAYsuK8CWuLuLBuLnKEEKSsaEKQSOEKDUfuKhWXA1YhSeK0SLSK3HD7TBXH7SAdi6i5uegBMAMwBg6Zg03INgA1gAXzRLmwAi8kYAjAG6gyIFblCGlUAEfEJhcsha5eWEqw

cYIXZpgCF5IvJmBeYmDBwYCZp/gLrKNxCVwGsm3wZWKlNq7CXSe+LchAQK2hcoOjoCScPwiSTTsJ+M9zNInXSvXGV5ywkNkNcl9zy0a7ddcpzt/uSyTGpmySWwiRBhwRuA2RNUB4gLUBPEEIAKAPgAjALvdMAGRA2AGR4PIq38ZvO38Rwn8Bl7LBg/UdPR6gDCB+6BDylwIvSgKFeK9shLoJgNVx5dgaJM0BuBEcFsBSQGuE0eTP8MeTnd8VEllK

IhAAIYPQAZ4pgB9ALK5UMA6g2gPQATUCkBlAFAAAYvZBKQMCT8+MAU24LnJIyFnKlSUzAg8ACAWwPFB8eWNxtSZ8S+5UYAeAH5NX3pXBVucQ1fgHlBFgCuBLxKcB4dLtkooC/w4cB8AioMcBmsBq54prLhVOB8oKGuuAu+FihbXJEQnZaeIHuZzgSSdXSDmcV5nbu9zA5eVNaSdmj6SRioktMHKa0V3TAeWBJIAFHL6gDHK45QnKk5SnLGgGnKM5

QOE57MHdQeZMBweavZYCNR4Y0CCgKtNVlpdE2AtgCcBrgBncrGr7kq2Jrtc7hvKfgFvLLxLvKd6U95tSW/Z5LiQpf8VdIuqpqkFqdqklqY0FHxSjlNnAeyNqegAxFVB5osgHYpuRsoZuT9j+sGoq0uNsoviZoAyIMsADUG8gYAOiBSAN9hlANhB6gHJLnANUBJAE5AOAKhhlAL54lZXZwgpuvYGsicAy+OThwRNH8ioHDgetMjyzsizATZTbLIKO

bKYcDpwbufHwbZVCA7ZaFNNbiAq/NGAr5chAqqQLOzySZ/Qi0bAqfuU3TK/vFoPuZh4UFT7dmSYo0m/kbkI5eUBsFbgr45Y0BE5cnLU5enLM5chIY3Pzt+6aDyvIO2i0AIXLoANpoS5V0Ay5dQqGPAEQkgBq4KtNChpdsx5csNPSC6Uihp/tHlV6e1p9UIXK3fhAAnIN9ge3F+AVIFahJAOFQGInAggIOLBDCokA20fqgi5b5Bl5VQB9UGXLXfn3

LiAPUA4ANgAOAHcIYAOyJFuW0AYANihc6E5BJAOLAk3PcBF5dcrq+bcqnMBZB15fOjN5aHkBFaBRXGitMQBIfKNlRId0QKKBZgHcJjgBfKVZVDojgHMAqyO4RgoKBRIwucBYoK/KyQBxhVwjlAkvNDptOE1gpgFdzdsiXTUlWB4c0eAqnuZAqySdAq8lQ3S4FTSSPbi3SvbmXJGSfvxYGOgrWSc2FDcHUr6ALHKGlU0rCFcQq2lelwyFSDyuSWOF

elSvYitDbIpIi2AtRP1M4YGww5wuP8IUJ4RPkA7kllXKTuFVW5oVXwrYVTvL4VU/ZhFYTyPGsTywRlQJV2dugm4YATlSH1UkcmAS4LgZ8ELiornkG6qpmJzKCco9p/tnJpdFRdhQ1eIgDFa9onkBwAXkBGY2gNsr6gMwAsQBM81gPgBHUMQBwqCOAvwKSB3FXkhlZYGiqDtihmMHrox6QjAoeeuJzNBjByyHGEmGql5coGpwS7KbKolSF4YlWlNy

gNbLDgIkrElckrHZZmjCSdbcBGm7LOVS9ynbjyqEPEgrs0f7K6SSUq5GqKqmvIbllGjUqsFXABo5TKq8FY0qCFS0qSFVnKOSQvYuSX5Fu/nUR+lX8gnNKXKxdoLAI/kkBgoDMqIKIx4x/jVpEeWuBGFajyrsp3KuFVuFLvGsrLlW6jC7D8rsIDcIjAJMB9AF+BUMIBAnIAahqgNgBGgGsAosheRr1Z9AwVavLIVe0r+PDrtbVVGF7VXvKyKBv9kV

W6j6AKpABQFahvsG6gUCtiry1bJEMdgZpmsG+4EoI/LIQKZoU0ZfRsUNMAEdGiSEplsBDbsaIi2J8gLOEyrgFaOrnZeOriSRyqslVAresrOqBsvOr/XGWil1XyrfuagrpsrWj11QHd4uJHLt1Tgrd1XKqD1UQrWlaQqOlX3TXyAPTXgFQqitJxECyOFAGFYjgooijhZIgCAoKN+r1dq1p/1TwqbVRMr8NSLpCNXxIRFS25rhLiMZKB6r12dIr21v

eLO1lUNu1p3De1soqjPiFqJHly1w1b9tI1fFlbkDGrdSaFrawCRquxHoBcAPg04AI6hdCCkB0QI0BZgLUA2ALUAnIMoA7hOiAvwL54sipvIwgJfKi7KkAE0JXLiyLwwG1TlBeWNvKlgE1wFgLDzeNbLgBSbFBetKjpTgCLpeIiXTcoMkB4YFLsIUAVgu0bn8JNdmiJ1YTop1R7LclZST8la3TClcprEFcuqTIh3S0FQDyJVSyILyNKrZVfgrmlcZ

qj1dhrVGqqq85VJwNVVertNDwBb1X39BYF8AkoAjBFgM+qgUACBpdJugQvFt5X1TKSO5csqrVTNNeFb5rt5f5qhFceFnVWEB8tU8g3kADF8QN9h9AJgBfUQMqlXFLco6YDrUgHGgNwHVo1tQ2rOMIuIFbtFAKVYeExtdwB6wFpwLXCVBU0elMxNetrQFS7KC/tJqq2NkruVftreVQUrPuUUqh7KLrSlSKqUtH7cqlRuqpVXpr6lfdqFVSZrj1W38

ulVyS1shqqU3M2gRdMaI96Awq20NLpMwIOitvEvSYdZaqvNdaqg8jCq/NYIrXiJqTEVejyY8t/82tQ3CNUhp87xVuyYtTuy9PgGqICetSktTUAMYWlqSIsK5/aQJhsKYp4Q9RjqqgFiB9AG8gADjwB7IF38FZYFNy1dMA9ZR8AMYKw0PgOcA0dvxEgdC2QuMFdyE0HrdVyNFAyVf9q8yHMAuGmmjb6FlMs0fEQtteoqdtQVM9tfXS51SUqlNQgri

lapqGSe3TV1Q38tNeHKFdTuq7tfuqHtYqrTNSblyFVyT5Zct4bcoLpU3EvRbNKsBRSftlWYH2ixSS8BHlMigTgKrsOFc7qVlVrtseeKp+FQRqUdTlE0dTqSJAGj9XHPLBN7lOVBfqaNMWsUtHGeUst+pyMdQXlx3dVIrPdXdJvdQdpYtbuyNnPuy0cq+Lr4Pfqr9E/qr9B21O4Kglfhh/r6yl/qyWj/rQ9XzcdFQp49Fey8dqo/q9qnAbX9Ygbii

oL9P9bcKDSugbY9d2xNAADFEgKhh0VUvYA/ucpnAD3xeWBbJ17ElgrCPXKIwpCB96HDhlxAjgrgMypzZDzkl0U1wOYFMAiMNxhOddmFudWkredY9zJ1TJquVXJrhdV3r+9WLrjtX3rJdSuqZdQ1NLItprN1RABbtXur5VYeqlVXnIVVZyS85Vo1tdRR5IeVcANZHjtwoimhQdUaqatOTgDiPFA3DXyoLdSvS4dXOibdXhqkdfbrV0ev8gtZXdrhE

IiCSFahpiH/rW1pFrN2YtTfVaASZeKtTu4UHq3ATEa4jbZ4ftmHqichHrstUe5ojeEBYjQmrSck8h7IHcJsIDChaIMQAyIIQAnINUAeAFAA7hJgAjAG0AexL/qApugAWtXIBW9XDtwSW2hkgHQrG+CVAE0J2hH3E5oGwPLcLgDGEGVdXYS7LcBQUMVkUUIjhCMGpx5tSzqJgBMqwoAXc5dspFspgob2VUoaBdbJrXuTAqRdYdrNDb3qJddcapdYP

q9DXWjqlWPr9NRPqzDY9qLDb3S59XnL2ph9rDcGhrvtcMq71cTgUcNVkOMHqrE7kxqoot8pKcAXgLbpndVpn+r/cqsqnMOsq3UeiA3UCnKRwJgADOthBmAFdNAIJoAXkN9gAYvUA3UMAoF5VcrwwBhq7lVCqgjYjq4VQFqo8sLAqDegARwIm4yIPEBxYLGxaNYzkx8vlkGVWxhMwAsALgNwa+Ik5p5gIcAlgLLtXNAihGdZCJJIgvRWMLYQ0wtFB

j7GbcWVXmEMlfzq5AtOqS/moaFNd3qUPAKrvufcbdDb7d9Df7dR9TdrFdQZrldeYaZ9S9rrDa7hB6cLsL1fSpo7rvqU2CuBVTQwqxdA3LZlUXww/mDAAzXBgj9b+qT9d5qGTRfrkdQ7q10Tfq37NWBMZijNqgN4KyIGKJRQCOAf8cO5Lxf/iN2Zp9ZFSkb24X7r4tfBdjUkHrkzYAC0zb49MzdmaMDdzLo1dgbr4FWbEljWaMzWRAszegA2TRAB7

IIBBbUCcBwqFTo0NQGj+TbLcFOGZwLOPJxYogXgnCM8RFtVxgEdjlAzgATgmdUFAfgCxhvlOY1HctKSswvErG9WOrNtVJrTjXqbdtZ5xDTd7K1ciPYbjaaaA5ToaztUPrNNd3TMFcYa7Te8ajNdPq1dTnKNdXnLw7ovqe/h2iY7uDBL6Abq5wiDrITbm4PDYVAvCHXKtRIibOFVGbrdbhrGTZfr4zeEbEzcFqIAMJIC2kRtGbhSVxloFIKNhrNly

tQUfJIssf+obNf5OFqbxbUEvdcka24SAbSzXuyEtRAadnAuhsLV/TRlnSMCLeRspllRsuamRb6NkssHmlRbJuW8Tpuc78ijSWox4DhaFZsRsxlqRszAltjiLWLVSLUyByLT81KLVEByjWg0uxF+BJgD4lJgMwBVIADErpkYBSAIBAAYssBKWssAD7j55ssh4qh+CrL3NAkAfgCcASoM2BcwMSqU0LlA4gNFAuJBSrlzXigNxFKb9XK/LrlDnqUeT

Ia/lHlBdZfMA/gKCIe+JcA5DayqW9T1kLjfJrLzYpqTTaNk7zeaaHzU8aR9ZKrbTePrTDR+bVdc9rs5Z0qLNaDzU9f+bL1QCbBlT9reSWvZWFdtzt9ftlWyFFFi2HMAaoObqf1bDqrddZEHlRsrMTdibcTYBB8TYSbiTaSbyTZSbgVdSawsLSaIVcCr0TV2IDUOLA3IOiA7hOLE3kJIBytblcBQJoBxgBE4YABh0qTYTrQVfXFMNfSbkLbGbQjRq

SEze6IezQKBxYPOAsQG6h4gH8aejaOawSRWrIKHDg8cOFMlgM2AHrRKa4YEbLvhKl5s0EjzVboqbYtFRgQwrGjNRNww5tVzr2soebm9cebttcob9TZ7L+sllbjTSNl1+Cpr7zWUq6/hprxVWHLirbprSrYZqp9RVblVWZqfja6bFudZqq5KNNmwJuAzRGBbpjYdlM2MdlQoluB6GEVALVf4ahrTpre5RsqNrVtadrXAA9rQdahAEdaTrW3hzrQtb

LrTSbrrXSbKrQjr7rQ6r3sk6qs7mtMMGmpTzDmGr4jWp9wIs3CO1sAbfdf6qyzYGqKzZAbQIKbaUzvGqxLb7SJLcTkpLTUBXbRWcwtT2aDUAKJrAKhhxYvoAUnjqCrUEhA7Adnx3tT9bHLQiAVZVEr3gGOiVzYOiC9dVAkcIiS3+CjhN0FMAkvDFBliMVpElRmAWwOKbmVeWQJdsF5LXGuAfDffQjjZJrXZTjazjSoaMrReaSpj7L1cvArbzaTb8

reTapsmKrLtdTbrtbTa3jWVaGbU9qmbbPrXtazb8tP8bUNU1bgTb9ricKERs2GDb9VRiSoovrKhWKbd3NUbbkTZjyANWiagNV2J6tViA7hG0A4HuiAUgPgA76VAAUgG5AnIOLB0QJgBJAC3YnMItb0NVraIVbdb87nrbmTcRreZTsp0QBGpHUN9g3IKhgyPEwbL5f8A4gAcQNgDMa21RnbeABDAYoE1g0dKFENgJsAkwrywoQNihR0fDBL6MXT0b

ceIm9YORsbQMaW7XjaO9V7KO7VeaMPD3qe7SdqNDQ8a/uRdrQ5QYabTaPaldZPqVdZPbLDczaZ7fN54gHcJ2bc/wg8DvQILTqI4YHXap3DvrGYMBRPCNcAxbRrsJbZuq1rU8gtlTsq9lQcqjldgATlWcqLlR/aNbUtbv7Uvbj7cY7T7XcJz7ZfaeSDfa77Q/an7S/a37RdaRzTcqbrTrafNf/ar9VqSMLZEaIAFSDqLep9bxYAb6LQ+KVqf7q1qa

+RD2RIBAnR7auZV7bCjc2a49WisdLa6iuxPgBc2uxVHhEIBDlFiBsALUAKAPZBJAKpBqgCkAZ3r553HUFN5gMxhCoDGFEcNFBbCPvwooHwbGGp2gswHBQhSWuaoiLzEvCDHhG+OjpmsGjaqWLcAgwsHg1wE2AZxLOJSHZjbyHU3bKHaeb29eebO9UaaWHYw7crb3ahVWprylSHLKlfWjXjTw6PjZ+bKrSeqSPFyTodh6afyJ9rp6ECaXUeXLbQAn

TCstXL9Va2goom/K2yAuE97UibELfDqvHXaq4zWEbd6fvaCJj3KwAJuqwALogygCkB9UPbgwXZcqQdK5bUoEbLPCCbqunR/bZbnrLkvOsBJnca5xgDC6s5XR9TumQVuwC3pWFIVpDvg0T5pMzgr+IXFKXcKRyXeIR6nm3BWjE8gqnUmBC4ky6v8C0BNbSvLgQEEAhwKbAJuD2aB5UPKR5ZIAx5fQAJ5VPKZ5XPLKnRhqVZVnaMvPmQwYCuEvgDrK

pgC+5N0O5bH1RTgNxFq79XNMB89UsQISWbcF6MjgQ8pcA2yCigUrdqbK6ZkqqHWea8RG9yrjVs6jtbcbDIiw6LTRUrZdfs6SrWPb6bXw6vjcDyXTcI6mtfPa09bwBmrSPS9iDjBVWLrsKtDlAOrUGaQYAihN0HU6VHZ5qUTafr16Y2xvHWhagXUiaQXcNbwXZC6IXTC7gVeC79XRxhDXVvrWFSY1LlWa6gdQsBLXSzAUUPi7KrYS6P8moASXWXCG

XQxA6XbfgaXUyAB3dS7BdId9OXSy6eXVQA+XUyAJ3UqYp3fyB+XQFYhXUA6viWfaL7Vfb7HdIBHHc/bX7e/aQVQu6VZXQx3gAmEy7SJ4GtJF5W5YqwmNeLlkeZ+4JIt+4NXUVAlgJuA4TSEQodXubrkKTgVwklA1wKsApSZnr9+PdzjjTqaTzYLrVDSs7CbWs6crSTbmHWTbpdZabnjfLq/XYc7yrfw7vjUI6LcvEBGDZc7XQNc6N0FG77nSZBdZ

YBRoUEOjE7vsQYTalBsUPmhE0F86ELQEbFSX86/NclafHU7rf1UW7JbXC6P7aW7oXU5hYXeC6FbkcAuGK+7FiBDAP3WUBnAN+6EYL+7mwLG7oUPir23VPayKLfIiXd27UwKS6+QGO7xCCO63FX0rDcJ/lfIPiAq4DhhIAFkTcgM9AIACA7GgGA6IHVA7DcEKBRQcQBNPcZAS3e8AVgLNqG+OOjxIlC73PfarW0G2hx0ZTglPRLISILS7j8qO7NVe

O6wVZO7THby72XbO6YvfO64vdO72XfgABXc9bV3X3KtHZIBdlWwB9lYcq4yQY6KAOcq5XddaFXUDpVwPmg/gNjtSQJe6HCO8AQdEDrGwFxg9XZXqlgNV602HZoL3fXrjIFWq87RjB99ScAUcOmwZnRtqsbfM70rTOr27RaxsrcTakVHlbXXcgqEPd66rTXLrDDQc77Tbw7HTV+bqrc7g85UCrcPdwB8PZCBCPaMreAOcBkSbHhhSSVBFwgKTYHQi

aIzYNas3dGa7rf87WPfm7DbYW6zUaC6S3Qta+Pc9ruPU5gcwGg6C7l8oz3T16P7f16HCIN6tblq6WwCF6cKJ26kaup7nPb27tPf26IvXp7UAP0rDPVUBjPWoAF5RgBMKJZ7jFaYrzFZYrrFbYr7FY4rnFa4qifY56NPej6uPRC6rZL1pF0Y2AOveGFfPRcAAQC/wswDOIwlSF6RleF6qXdj7cfYoh8faQATPUT7zPVABLPdZ7bPZA6GfYQAnPS57

QXaz7tuVxhcdssaE6Yl5LlaTgtfdZocoLr6hIsL7VQIy6kvdy6UvYu7EvfXFYvV/b4vWF70vcu6kVVl6NlSBqacuBrINdBrYNfBrENchqyvU76/rUBQYoDpxaGJxJ89cg6zOLzFbNM0RJDexg9XZ4RF6GXqtgFza3Lc4a+1ZEQ3gAXg8sEcQ1wNFB61WN6edY3a+dWB7zjdN7IPXQ65ve7cNnXB6+7St7dnT66XjSh6tvUc7GbQI7p7SG6sPfZ6I

7g1aF7Tc6zvUVpUHYF6mwMDrsoIY0BbR4bqcLR6ioOwqp0ZGbGPdrs/7f86wbQiqy7sC6fvcW6Dff97y3fqhK3cn6e+PvrWYGn7s2Nz6wAM4Ac/dVlqyBLtC/UMq7cAS7VPV26JWWj6yXRj6oALp6h3cQBP/e/653db7Hfal6wvXb7mXcl6AA4u6XfYK63faF70nU8gnlS8q3lR8rsAF8qfleMA/lQCrDvRG7BMPK7y1and2+AXhLgJKwiyNjBo/

hq7C7jJFCoJt5y9Yio33HiqkXXnbUoH6bevS8Ba+L8BitCsAmYGFF67WQ6gVBQ6pvQaaq/bN6ibbX7YPdoaG/Y8bEPUVaR7bUq3zePbA3U6aqreZr9vazasskd79PQP6CPeY7o3TohQRGSBM9RVoioOKbZ6S8AViFOFhWBm6GJC96kLSv6WPS41HVajrN/WgZfvTv6DfXv6nMOC6aA6uFioCHkMYL1qP7UkB3PSubwUGOiLZIj7dwE/6UfS/71fe

/6f/SvZRffS61AyRA8fRIACfaZ7ifUxJSfSYqzFRYqrFTYq7FUIAHFU4qXFW4rgVSVM1fcz63PZZwRdHP7A8PfL3+L56Ywipwx0RDAiMB8BzfTKhLffb7QA1gHyvQl76jVb7WXdgHnfRl6V3dAG+ZRABnAMsAqWhwA3kMZa/sFag4wJWA3UKQBfftgAvwMQBAAwe6bfdU6F6JDAfgC8pVxM07C9XlAVtRq4GndcAqA4xg80IJEtbrjtT3Xt4mA+j

BeYi2AUcOF5NRAmhwlRjbxvXM6y/c3bFnbXSaHQTbq/UIGqwkw7RA0t7WHeprB7Rw7rTTTbpA3TaHTZ8b5A6c7WpqzbVIAXLGrYP7NA0R7tuasA1wPMByPf2isdlva7NM3LR/tDqBrZbrLA786Yzav79bY7qN/d97HA9v6ePbv7+PRW74XYsAYvB8Bbg8uAOMDmwygPFAng/8BiJDjhC7izBQg21hwg8S6mfW/6ovTp6sfV/6Yg4VxovZ0H//d0H

g/WZ7gA1y6Bgz0Ghg677RtIHbNrdtbdrftb0QIdbjrada1bT9a2XWOacvNDp3CFtlcwBCgpjbwBo8GZBxPaCItvNmAv5fCg80IvQQfQyrYoo06zbklAY0aLo87NrdTNDa6K6U5xdTeB627QIHSpr7Lu7XX6wQ2VNhVeIHVvUh6Nva373zRPag3VYbT1XnLRQOiH1A6d6sQ+d6QKL2jPkJPTebaq4Pg2+rDRLnh4YNFBV/r4aKQ+LaqQ4Ea3vTYGA

HREbygJx7N1X96XA2yH9/fC6/Q6GGJgDCggw3mB4XTmB3gOGG6GhLtEgBKGzkFKHUfVEG5Q5j6xfYqGFQwkHygEkGqItL7CfSUG5fZZ7Xre9bPrd9aLyIz7X/Vp6WfVJ7UgFVl4dEw0/gFfRuDQKHXLecHq3clAhtQjA2g46wOgyAG1QzaHygBy7+g4e60vcMGoA4Yq+5WNbvHhNaprbgAiTSSayTRSaBQEH6Ng4MaqDinT3PXjgEwhnYXQwZpv3

XMBPgAXS40cn8GyDWHYoJ16KAwZotZJqbYoHo0VSWuA2+BzquA7M6eA5N6i/jkrlnbQ7BA9B75vQZFN+J66CrRIHnzaC7XzfCHtvYiHdvYoHZRKzbAIKWGI3bc7oA9iHngw1wQdMKS3NY2Hf+CVwwYAXT25R2HVHV2GmPTSHew2x6GQ5wrBw2OGWQyOHAfe4HmwDRGQRHRGRdILlLlXEA4oBCgFtQw1cSauGuYOuHIg8z7lQ/KGdw/uHIAIeGkIj

L7TwyT6nkPpbDLcZbTLbgBzLZZbrLRMG7LSr6yg7KHBPakBvI1uBddksA3CHR6ePbH8zVbfLOMHpxooABGwI8O69w1uHJLBBGtg70G//TqGNQ8T7oIwaH3fW6jZgJIANwFiBJgOiBlZJgHQSdhGTfWZADI9igOYP/x9fUnSEdg8oJ6SDb+tKiT4bYxgFtSe616J1A7NJmAzbsVpow+kq7XXGGK/fwH+I0mGu7fyrUw3cbwQ166m/Wt7fXdw62/Wh

6Cw4I7u/aHd4gBLc7DYFEXgGm7ooMjypHZ1aSsu4amwycB5dPiHD9Qv7nvYfbXvdYGQjXSGnrUib8ovfqrfH3dk5hT5Z7lb5ZXkBcrfLqUrfAa8b7l+UIqgVRX7txd70OjGsvhG8cvhIALbdeLgnbRbQnUWaGLXba0jZE6Mjc7a79bgasgEjH4flb40Y/X4MYwhyKfNjH6/LjG+Kvcdvyj+KSaWJJAKkL5eY2THrShTGczYREHfuJbtFZJbknWzG

bPlfhOY5U8UYzstsgOjHdvvzHANrVR43rF88Y6LGCY+npUvlLG4jKs9ZY6Mh5Y2k6xg/sqIqDgrgIHyaQ/QvR8Q3LcqcN2ikoJF4E0LmRFgHnYYfeF4Lg9/LmsJ1qKGk1w2daBQS6buI9oyB6Do+X7W7ZX6To53brzQurxdR674PZmGbo9mGuHXCH/XQiHjncp6FAyzbhHQTqOUMqJPowx4kgPhhe0QwqS7oDHM0CF4jvN4R+rR5qLA5DGrA7m7a

Q32G/HfM4qgCp09bBT5O9tzHhiDjGrSl19dAvX4malb4M3kaAgnVbbvVWLxtPiASSzfbbmLeWbICUHrh4yMZR4xpdx43rGhY1PGdglb454/X4F45Q7vtvjl0taREsDa+E6XhK5oDPvHPjlw8j48bGaKRQ8z47PG59vPGNAoN8kAD2ajAPQB8ABZRiTf78frSNGg/qaAAbRMAy+NHHJnZGi6wAqwOYKlA7Q4N6TXN07zgLU62dbiSdo2XSqdonHYw

8nHqHXxHAQwJHJdes6RA5dH0w9s6KbVCG9nS377o3mG5A3JHy41h7agGI7o0FvqFPcF5G48+rjsi2HWYNxqjI53Gbst3LqQz2GYY/3H97flFh9ljGVvtUAP48L4+Y0WArfH28NAlb5zHlomUNdTG8zR7qQneO4gCcWbGLZvGwDSxaXxWxb+sAon6/HI9lE/X4eY6JcDY+on6/Jonj4xT4dE8fGGzYk62xD7bbEwLGlEyomXAGonOABonDvhY9tEy

O9dE47GdlOCBsAPQAeAGoAdfI0ArUC8hT7kYBAICPLcAMsBDPCOaE7ZQ6ZboHg0HbAJkUAIr6wNH8IYMErqvaSBWI5M7zZNF4zOHmh7ZSkqdozGgEgBDB2I4cbuA43Yfgws74w6nGKE6dGM49QmFvZs66EwPq2HZTah7Zw7YQ1urpI+370PcG6iw6za3IMpHNg3DAh/V1MQKDsb1wFm5ZHTXL00B4aRScaI2FeYGJE3P8zI9ImmTZZGCeZl7Rgzs

pAIJWBnhNhAQcMUGoE8TrRgIOiYoFrKlxAjB4KA2HRWCg780I17coJThOGhPTxTSn9jOAShStI2BwfSGGDzV8GuI30m+A/jbippQn7jaMnhI9X8xA1MnGE837kPSwnZAzt6TnerqarVyT0MB9GvTVyprgGbq/oxLoNTc3HcsEsB+SfnYwY8vSTI93HhrRo6qgH0VbFYOJHUPEBHUM8JlgG0AKAAahsIFyJsIOirXHf6j3HdrbS47ra+47cn95QPH

jbYug2+uW1aqNW0R4PEgl416qZFSYmGYwor0jYlrWYx+gtU6k55hHSM9U5bAfEyrHvbWrHLU+mNtUzamKSnamSwz2aoAO5ArUCkBsAHcJSAE5B13NgBKwLMA2AGRN4gF+B13CWqg4GWrbQ4sRF6MRJGshMqoeZF4sYLFBgUJxJa1Xih73TnSK9Rrd2MHXHBQ2Y0zblsBenTXJQbbcBxjWpxgPaX7FDb8GBk8dGhk+nGGHTB6xk/X6ro2JGsw5IGX

zSYaA3aSnS48iHc5azb55eG6Nk/f61I5WGmwOwGlgGi7AzRBQfgFFFWYNjserecmpprOj1HSfankBMGpgzMHVIHMGFg0sGVg2sGsIxsn1Q+CqsQyNa3Ufyn6gIKnhU6KnxU5KnpU7Kn1bW47lrVeneU1KBb4C5AnIJWBmAE5BKcsnrAIHAADUADEUgI0AcjcY7302Y73sL/be43brYY+hb7k7BGNlZoBvsLsg5ThQAE7B8nI6aMBkUEErQYCcnRT

e8Ho/qM6rgDHgBfRm4w4y8BNOM2AaPZxJCHeuBRNbIbPgyX6jzdxH80bxGnXZcb1DVQn20zimg3HinIQ2uqJI8NapI0XGZIyXHO/c6aVk8I7cANwn6uGDBWw14QHNa7kp/U2GZPegmSoHBanvZSHuU92HoYzcnPvfYH4Ywug8UQTYDU/NSotUAbdUqka5LPO4lFaxaVeOZmUUZZn4nRGr746rHH46rwLMy4ZYk18TIKHAAX7W6hsAO6bho58nqGq

5oPlKg6BSax45zZuheWDDbLgKardXd06BnZjgpw3CIi6XiTCEw3aOM6imeI0LrEw62notEJHPbhMnlvbnH2HUwmiU4XHUPfmGkQ+SmlA8I7QcNSmQTU5pGuK+6ebS4bOs0m6jk02Gaw6zBhWPP7OU5m6DM1cmjM6hbAXV97OFflFz40fzByshUrfIjYDFhOYzDAWZSZA6QDALEIGxZJU3GFZmCzXRb6Y+E7Z3GannMyWJ0AAtnRRUtmWCCtnjDKY

YFrKrZimDtnFBHtnKqAdmPM3fHw9X4nnUxABrs4RaeqT3h7s2OZHs2Xlns9tnWmslJ3s0vgDsz2bb0/emRU99gxUxKmpU+LAZU90bMA6BHsI3rpDZDbJHDSThdbvV6FzdChrXB3xE6V+5VoyuAzIHMBbgAwxQprOaHg42QU7bjgXI2qSY8FqI60wVmG0/0mjo+imy/oJHhAx2m0w8mGMw/inRMxgrJI/2ni4x36MPS9Gq2PEAVueOnP7apGRlaqI

olWKah/nWGmwASH5HZLoioLwnRveSHxExumFScv6EM0jqPvTNnTM9ZGt/Vx7hwzx7XA10BwXcd4ac7cAoefbLGc+i6t6EfY2c58AOc/5GVPSiA1PUFHZQyFHtw/EGcfQZ7JfckHjw6kGzw08gnky8m3k5lGZQ/eG3PRXYC8KzBdxBmF73b56EYCunE3fMBqcDmANwNVHNQ9/66o+HmGo6qHWo1hGMAFqGHfRenwAx1GWtD2bsIEYA7hPZBGgC8gh

zfoABQIkAlIPQAknuFQmAC8rY0y9QnLTgGZTUcBbgLyGVwNvLWNdVBbNIuJniNWqkoLub809QG/gFpwL6LmAQdKFFTXaThEcMd4dMwbtswJzmiE/WmTjY2m+c0kQdYA/mMIwdrwQ9imKs6Ln6EwPaJc1dq+0zIGB07JGyU9+aKU3nLjIit5jvRiG0IJOm1c11MU6a+7CoDt4VgIImPDaMbA8ERh10zOiFSYBrLHTumjLbMAvwH6meAH9hVIPQB0Q

EYAgDtYB9AAywVA9Bn5Ux+m7ndemuxMwAf065B/04BmAYsBnQM+BnIM3Kml5bQW15Z47zIzInVU0RqD5V1GuxPoA2gNUArULgAsQKpBjVG9aJgOLAoAFiAUgOLBrUPu7P7cT6iAP0bL5YvT9XHjhu+IOjq+P/xbkFvrkcPF480yXZSMIbcwYE1gRvUjy8SazBeYgKTDvJcBhIpcBL8/lmJvYVmuM8Vm04/Q6ys0LnBM/OrrozVnCUzmHiU3/npM3

Lm5M1h7KHVXHosCd7qoFsnNsjmB4vJ5b9k4YGNMy3GSuCNr63e2GTc+gW16Q9kpswC7HrchmRg6hm3UWIBxvIkAz3MOaQSZFnwSV8p3PTDhxjcXnsHeZoLXFvRoLQHH4wpTg9XQvQz6Gmxhs1xI4ldcgLblzmvCzzm0UwCGMU8Mm20+VnBVZVmIQzs7Qi7dHmE/VmHo41n2E5h7Xo0/nVA5qqObfGhYot7k6w9CbmU+lg8cHvQOU34auU5InDMxb

njM9bnr9XImF0F0EJ2vL42ough8DfBkcOlrqJFcJZLbYambM2E6fdaanmY+anrE4f4bgh8XfQZCkfi7MkROA6md3Cg0fbe8XlBJ8WbEmUgES0jEkSz2bGCzpBf0ywWgM32aOCxBmoM+ensczAmx8sLaB1c5rZtSua800/KlgIbdefRsAOvbWGVo+NqS+DihmFdW7ccN/wmc38AQ0bBb8sCcAQzQnHr86B7b8ynHm03MXSs++JX80sX385MmRM8Pq

xM1x6JMw1m2EwAW9vQpHhHe8m+/QkXwC+WG7nZWHjgE27W3foHOSwunx/uGifhNmA0C13LLk+bnB5LGarc2UWC3bbmmQ/bnnA47nRw24HLla2QtOKl4tboArBS+i7hS5lngLWKWR/ojhA81YhAoz26w8yqIKXXVGJffvApfdFHDcAnm+gL6n/U4Gng0xQBQ0+GnI09Gmz06UG08656Dfc2QOnWmwEKDDoi/U5hDfTlBayEi640MVly8w3nK82FH6

oy1HII0AG+g7XmBy2BGIAyhnE1VUAWDcwBcC/gXCC8QXSC/YCU5pQXMI5fLPlAkBcdoHgW0KwbIvMlAU/Q2AYdMyowzVvnVo2n8ZwmH917MJ5y7WcJU6TJ78VdOHngx4Wek+B5vC+7KlnTxnMrUCHBcyCGLo9nHhM6sXpk9CH1vQXH5k5JnFk09Gu/TEXXo3TkDi4kXI3RWHh/Z5bIYPOm5Hftl1wDPSsi7lhtuamxbgDcXjI+Nn7i5NnHi9NnPS

7NnndTZHAy3ZH/Sw5HLlb5bDbvMBzy8b6Iy05gqDjeWxpq/x+SfmgEy8j7pQ3eHjINXmlQ4/w4g4O7f/Y1GwAzO6hy8BG68y3n9Q23nRC08h4gFag/sBwBvMapokgHABwqKgGEAOMARwHcJVIDyAJ8/Gm/rVR5eYqXbvlMMaHCOKaooMjgmI/H9NRIjh/gDRn2VLzF3NN6HCsHFBY49n8YoBuAQdBmim+DvRJS9zmb87znZS0SBH8zrAqSS/mBM2

/mzox/nq0QBXas+EXNi6wnB0zJmy47sWFc4QB1k5/bICx1nS7FrJCsnmn9VbLtMi5BbDRCzBWc3LoxE/vafnQ8W3SyqmTMy8WKixOWJAGsBo05OhagFmdxgIQADUKQBJgOpAeAMapKwN9g42A5bS1Z4q6NWdl+DVVwvlDUmXQ/5bTIBCh99SigQiNSrunVKTd87R6VSbR6wbSXTlwPlBtbgjhcdhmBQKJMXvg9MWis2Pwwq/sW+M1imoq8qWYq6q

X/ywSn1i3VmQK9qWUq9EWznXnL7LTBWTSzer4K6m4Wgy17+s8wHJ/aVWW4w4R1gEvQnSwfaCK66Xz9fVXni747xyxUaqgIQBo0714NNENHz09Amo6WEQtOFzbSMHOmx6emmFWO4R4Ewfr4YNtHuna2HUgA7K/3XJ7ruWbdyyJ+r9ZSN6E0CBQAq1MWgqzMXyE/KX/C4qW7q2aau0/3a4q89X843MmtS1sWdS0Onms/qWsPScoDizrrBYI2Ai3Ef6

Mi05qhtQbJHS/R7j9Uv6z9XuE83UjX2PfvS3iwGCl2oO4zXCHkDgMih6dag61OJ6rrM0kaTs2CWInQ7aA9dE7g1UbCLa19n8jVGqE+GiXza6lqezXSBlAAKBUMPUBKwDh6Is3hmmcpuWrZLD7f3Z7k1OIRAZjSMbXNLnaU6ecGRDTChf3JsAtwGzA69dFawyFqaYw5JhpMLJhMc23r/g3zWBc/xnFi8LXliyEX4q2EXgK1LXkq//nZa4AWWs1h6K

S8gql9YcXKPKSBRpm/KN9RLps2N1bUHdjsFgDDWaq4RW6q4hnZE2Zn+sIn1nAAAA+PgKg59bNj5Det+Z0ZDr1gxZHTVSGUM3YUIbEQmCEEOBzdPJCouTCzTRQ6gsEO0lwaK/YTqRmp5wvFJzxKIkTQDdQvsLpnGwBa4Xmcbk0aIFE6MFWBe7e/5qrSkyhUehyjITTKvFReIA8BxkQxdPwbxOBurmOPzd+aObQ8cKWv12g5L6Xwwx7C0CXHQuD0Q5

ZR/Z1esb11bPI2WIw719+taZReIH1qda3JY+uLBU+uz7c+vLmPobzdTWC31ryjGFLNYA1DBQW0Xagv1g4pv1zTKf13IDf1/eLSGf+tDkwBuZIVgz/mPKE3tZ7PhBRFxOmWBsKwhBtxUIgKaN7WHoNtECYN3+Ll8nBv26TfD4Nhi5EN0BG3JUhu5mv/GGJ2mPGJn1Umpt2tbxx207xi1MBO+RIH1yhumGGht71mhuMNsODMNjsxcSs+sEY0MB0yTh

vX1neA8N6DT2HffKCN5+v8c6UJiNjeISN8zKLwPZxkpGRuxnABuHybeuKN0oxgNtraYi2vHQN2huoNo8zF0RBu6NlBsKwgxungY0BDxbBuiN3BvmNuGIENxi4LMWgEFqZEt/bTLUB0r4kRmdED2QfACzAR1At9NMFuoGADOAQgCJAXqOYAZQDvRn603xVMAAQFiK3y4/ON8AEB0p+enIOqg7peGnOWuz3KkgacMbiSlAl00nC05ihoT0gUlh/R8u

cR3pP46KTAyYOTAXVhMN+Fmv3flmhO/lkWuN+tYsS1oHmFhr6us2/QCKZgaa9aMY382/tEMpgbMtxjssT02esBGyhi8KriLqmkTVL170uom53N+l5stO5soAO55suz507J3Nl926ygltA+roC8eklu3N2zTktrMAJltK6qgq+xcU+gYoFY0DaMKTSWQcAA0wQTA++ShFgF6ijwuXyBIVIYAMARoVTrMhMSYK6vitwK4cvJxnPll5sV195vFAP9Sk

XDl5Stx12eufrLqtvY4Weq/BSzBUtwqPVt8oBVtC116Qatq/CKtoTOuu01sTQDl6Tg0Wsd0+1sGtrIDcg9Us7AV1vy+w1sRaiC7etjl7iUKbD2NtVvytq/BXwG22Wt/Vs+trIDzjBiD9lwsQBtq/CydBNtiV0NtWtrICcumd758DlBytjNv6AOB7DEScHagOYjJgGZAigNZNBQLHDueltCFYCjP1ysttogEUDoYZfMY4MP4S7F4MGuL1u+UAwBCt

zMT5PClB8sPFDxAfdxJtrIBOtgeulKpkDitvmbEAAxNqt2dvigWqSvqKpUkAZl6YFZEHWNVdu8RmMjnTNNSKy5QDUgQyglQHcAXe09snt2KiuEO34QAV6X6ViQAO4o9tJYWKhPtg7JIgfsiL0IHijtwK42t3hkhIUbgKB9SBw3cX0JezdvKx0oRaFsDtYK4YiQdk7DKQc5Awd8JxMAewHZABDvSmJgAbtj/CCwE5CjtkA7SBUUCiQOADrtjvFbto

SbQGXEB9tjQtrNzwD15s3H8abNtoQUiu/qrQwC+b4X7ZaPJ0fDuLcLMjvTEH3Cjt+JjIgjUp3gAQpu+qMgd2yUh9KyFUWQIAA===
```
%%