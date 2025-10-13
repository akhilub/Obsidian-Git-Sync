---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
1. Define Scope

Functional Requirements
    • Ad Selection & Serving : Display relevant ads when a user visit a webpage
    • Targeting : Ad selection based on user demographics, browsing history and page content 
    • Event Tracking : Track ad impressions, clicks and conversion for analytics and billing
    • Campaign Management: Advertisers can create, update and manage campaigns (budget, targeting criteria, creatives)
    • Real-time Bidding (RTB: Facilitate ad auctions in real-time between advertisers
    • Fraud Prevention: Detect and combat click fraud

Non-Functional Requirements
    •Scalability:
        - Server 100 billion ad impressions per month (app 38,500 impressions/second average)
        - Handle 500,000 ad requests per second peak

    • Latency : Ad selection and serving latency under 100ms
    • Availability : High availability (e.g 99 % ) and fault tolerance
    • Consistency : Eventual consistency for analytics and campaign updates
                       Strong consistency for billing/financial transactions
---------------------------------------------out of scope -------------------------------------------------------
    • Durability : No loss of impression, click, or conversion data
    • Security : Protect user data and advertiser campaign information
      ^nuGtklOH

2. High Level Design & Data Flow

    Main Architecture Components:
    
    • Client-Side Integration (SDK/Ad Tag) : Initiates ad requests and tracks events.
    • Ad Gateway (API Load Balancer) : Entry point for all ad-related requests, handling routing and initial validation.
    • Ad Serving Engine : Core logic for ad selection, targeting, ranking, and real-time bidding.
    • Ad Campaign Management Service : Provides APIs for advertisers to manage campaigns, budgets and creatives.
    • Ad Creative Store: Stores ad creatives (images, videos, HTML snippets) and delivers them efficiently.
    • Event Ingestion Pipeline : High-throughput pipeline for collecting raw impression, click, and conversion events
    • Real-time Analytics & Fraud Detection Service: Processes events in real-time for fraud detection, aggregation and immediate reporting
    
    —------------------------------------------------
    • User Profile Service: Stores and retrieves user demographic data, browsing history, and interaction patterns
    • Data Warehouse/Batch Analytics : Stores historical event data for complex reporting, billing and ML model training


    Data Flows
    
    Ad Request Flow
    
        • User visits webpage (Client)
        • Ad Tag/SDK sends GET/ad/serve request to Ad Gateway
        • Ad Gateway forwards to Ad Serving Engine
        • Ad Serving Engine Queries User Profile Service (for user data) and Ad Campaign Management Service (for active campaigns and targeting rules).
        • Ad Serving Engine performs filtering, real-time bidding (communicating with external DSPs if applicable, or internal bidders), and ranking
        • Winning ad ID is returned to Ad Gateway
        • Ad Gateway requests and ad creative from Ad Creative Store
        • Ad Gateway return ad creative (e.g URL) to Client
        • Client renders ad and sends asynchronous impression event to Event Ingestion Pipeline
    
 
    Event Tracking Flow (Click/Conversion)
    
        • User clicks ad or completes conversion (Client)
        • Ad Tag/SDK sends POST/ad/click  or `POST/ad/conversion` to Ad Gateway.
        • Ad Gateway forwards events to Event Ingestion Pipeline.
        • Event Ingestion Pipeline streams data to Real-time Analytics & Fraud Detection Service
        • Real-time Analytics & Fraud Detection Service analyzes event for fraud updates real-time dashboard, and send validated events to Data Warehouse
        
    Campaign Management Flow
        
        • Advertiser interacts with Ad Campaign Management Service via its APIs (e.g POST/campaigns, PUT/creatives).
        • Updates are stored in the Campaign Database and propagated to caches for Ad Serving Engine
 ^QrPcxi0M

How would you design the ad selection (targeting) logic? ^w8HkO2O7

For ad selection, I'd design a multi-faceted targeting logic combining user-based, contextual and behavioral targeting.

• User Profile Matching : Leverage stored user demographics (age, gender, location, income) and historical data (past clicks,
impressions, conversions, interests). This data would be stored in a low-latency database and retrieved for each ad request.

• Contextual Analysis : In real time analyse the content of the current webpage using NLP to identify keywords, topics, and
sentiment. This ensures ads are relevant to what user is currently viewing.

• Behavioral Signals : Incorporate real-time user actions like recent searches or interactions on the site to capture immediate
intent

• Campaign Rules Engine : A rules engine would filter ads based on advertiser-defined criteria, such as budgets, frequency caps, negative keywords
and targeting parameters, and then combine these with the user and contextual data

• ML Ranking :Finally a machine learning model would rank eligible ads based on predicted click-through rate (CTR) and conversion rate, optimizing for both
relevance and advertiser ROI all within <100 ms requirement ^k3G4GWQi

How would you build the data pipeline for collecting and processing user data for targeting? ^zR6xC0a1

3. Technology & Data Design 

Ad Gateway:
Technology : Nginx/HAProxy (Load Balancing), API Gateway (e.g AWS API Gateway, Kong)
Reasoning: High-performance, low-latency entry point handles traffic distribution, SSL termination and initial request validation

Ad Serving Engine
Technology : Microservices based on Go/Java, In memory data grid (e.g Apache Ignite, Aerospike) for fast access to campaign data, user segments and creatives. 
ML inference service (e.g TensorFlow Serving) for CTR/conversion prediction
Reasoning: Go/Java for performance and concurrency. In-memory stores for sub-100ms latency. ML service for real-time ranking

Ad Campaign Management Service
Technology : Relational DB (e,g POSTgreSQL, Aurora) for campaign configurations, budgets and advertiser details
Reasoning : Strong consistency for financial data, complex querying for campaign management

Ad Creative Store
Technology : Object Storage (e.g AWS S3, GCS) for creative assets. CDN (e.g Cloudfare, Akamai) for global distribution and low latency delivery.
Reasoning: Cost-effective, scalable storage for large media file, CDN ensures fast delivery to users worldwide

Event Ingestion Pipeline
Technology : Distributed Messaging Queue (e.g Kafka, Kinesis, Google Cloud Pub/Sub).
Reasoning : Handles massive event volume, provide fault tolerance, decoupling producers from consumers and supports real-time stream processing

Real-time Analytics & Fraud Detection Service
Technology: Stream Processing Framework (e.g Apache Flink, Spark Streaming) for event aggregation and anomaly detection. In-memory DB(e,g Redis, Druid) for real-time dashboards
Reasoning : Processes events with minimal delay for immediate insights and fraud identification


—-----------------------
User Profile Service : 
Technology : NoSQL DB(Cassandra, DynamoDB) for user profiles and browsing history.Caching (Redis) for hot user data
Reasoning : High write/read throughput for large user bases, low latency for profile lookups

Data Warehouse/ Batch Analytics:
Technology : Columnar Data Warehouse (e.g Google BigQuery, SnowFlake, Amazon Redshift) 
Reasoning :  Optimized for complex analytical queries over large historical datasets, crucial for billing, reporting and ML model training
 ^z89l4v36

4. Low level Design

Ad Selection & Targeting Logic (within Ad Serving Engine)

1. Filtering (Candidate Generation)
• Receive user_id, page_context (URL, keywords), device_info.
• Fetches user_profile from User Profile Service cache (e.g interests, demographics).
• Queries active campaigns from in-memory cache, filtering by targering_criteria (e.g geo_location, demographics, keywords, ad type).Uses an inverted index for fast keyword matching with page_context.

2. Real-time Bidding (RTB):
• If the ad slot support RTB, the Ad Serving Engine acts as an Ad Exchange
• It sends bid requests to registered Demand-Side Platforms (DSPs) or internal bidding services with relevant user and context data
• DSPs responds with bids within strict latency limits (e.g 50-80 ms)
• A Second-Price Auction is typically used: the highest bidder wins but pays the second-highest bid price
• Trade-off : Faster responses times for external DSPs vs more control and optimized bidding for internal DSPs

3. Ranking : 
• For all qualified ads (including RTB winners and directly matched ads), an ML model predicts CTR and Conversion Rate (CVR)
• The model considers user_features, ad_features and contextual_features
• Ads are ranked based on (predicted CTR*bid_price) or (predicted CVR*value_per_conversion) to maximize revenue or advertiser ROI.
• Trade-off : More complex ML models improve relevance but increases latency.A lightweight, highly optimized inference model is crucial for real-time serving.

4. Fraud Prevention (pre-bid/pre-serve):
• Before serving quick checks for known fraudulent IPs, device_patterns or bot signatures.


Event Tracking APIs
• POST /ad/impression, POST /ad/click, POST /ad/conversion
• Endpoints are lightweight, focused on quickly accepting event data.
• Data is immediately pushed to Kafka for asynchronous processing.
• Idempotency : For click events, a unique ID (e.g hash of user_id, ad_id, timestamp) can prevent duplicate processing if retires occur.


Fraud Detection Logic (within Real-time Analytics Service):

1. Real-time Stream Analysis : Flink/Spark Streaming jobs consume events from Kafka/Kinesis
2. Rules Engine : Implements rules to flag suspicious patterns
    - Rapid clicks from the same IP/device
    - Unusually high CTRs with no conversions
     CLicks from known botnets or data centers
    - Geographic anomalies (clicks from unexpected regions)

3. ML Based Anomaly Detection : Train models (e.g. Isolation Forest, neural networks) on historical data to identify deviations from normal user behavior

4. Action : Flagged events are either discarded, marked as fraudulent or trigger alerts. Frequent review and updated of fraud detection models are essential. ^6QVxtQKC

5. Object Model Design

User
    - userId (UUID, PK)
    - demographics (JSONB : age, gender, location, income_range)
    - interests (Array of strings)
    - browsingHistory( List of {url timestamps})
    - lastSeen (timestamp)
    - ipAddresses (Array of Strings)
    - deviceFingerprint (String)
    


AdCampaign
    campaignId (UUID, PK)
    advertiserId (UUID, FK)
    name (String)
    budget (Decimal)
    budgetSpent (Decimal)
    startDate, endDate (DateTime)
    status (Enum : Active, Paused, Ended)
    targeting_criteria (JSONB: geo_target, demographics_target, keyword_target, device_target, etc)
    bidStrategy (Enum : MaxClicks,MaxConversions, ManualCPM)

AdCreative:
    creativeId (UUID, PK)
    campaignId (UUID, FK)
    type (Enum : Image, Video, Text, HTML)
    contextUrl (String e,g S3 url for image/video)
    textContent (String for text ads)
    landingPageUrl (string)
    size (string e.g “300x250”)
    attributes (JSONB: keywords, categories, brand_safe_score)

AdEvent
    eventId (UUID, PK)
    userId (UUID, FK)
    creativeId (UUID, FK)
    campaignId (UUID, FK)
    eventType (Enum : impression, Click, Conversion)
    timestamp (Datetime)
    ipAddress (String)
    deviceInfo (String : User-Agent, OS)
    pageContext (String : URL, keywords from page)
    bidPrice (Decimal for RTB)
    paidPrice (Decimal, actual price paid in auction)
    isFraud (Boolean, set by Fraud Detection Service)
 ^SMtekhAC

6. Address Non-Functional Requirements

• Scalability:

    Horizontal Scaling : All stateless service (Ad Gateway, Ad Serving Engine, User Profile Service, Campaign Management) are designed as microservices and can be scalability
    Distributed Data Stores: Cassandra/DynamoDB for user profiles and event logs, columnar data warehouses for analytics, support sharding/partitioning for massive data volumes and throughput
    Message Queues: Kafka/Kinesis handle peak event ingestion rates, buffer data, and allow customers to process at their own pace, preventing back pressure
    CDN : Distributes static ad creatives globally, reducing load on origin servers and improving delivery speed.
    Trade off : Increased operational complexity with distributed systems.

• Latency: (under 100 ms):

    In-memory Caching: Extensive use of Redis for frequently accessed data like active campaigns, user segments and hot creatives to avoid database hits
    Optimized Algorithms: Ad selection and ranking algorithms are highly optimized for speed. ML models are pre-trained offline with fast online inference.
    Proximity: Deploy services in multiple regions (CDNs, multi-region  DBs) to serve users from the closest data center
    Asynchronous Processing: Event tracking (impressions, clicks) is decoupled from the ad serving path to ensure ad delivery is not blocked by analytics processing 
    Lightweight APIs : Ad serving APIs are designed to be extremely fast and return minimal data

• Availability & Fault Tolerance
    Redundancy: Deploy multiple instances of each service across different availability zones/regions.
    Load Balancers : Distribute traffic and handle failovers.
    Data Replication: Databases are replicated across multiple node/regions
    Circuit Breakers/Timeouts: Implements in microservices to prevent cascading failures
    Health Checks : Automated health checks and self-healing mechanism for services
    
• Consistency: 
    Eventual Consistency: Acceptable for analytics reporting and campaign updates (advertisers can tolerate slight delays in seeing impressions counts). Achieved via asynchronous processing and data pipelines.
    Strong Consistency: Required for billing/financial transactions (e.g budget deduction). This would be handled by the Campaign Management Service using a transaction database
    Trade-off : Strong Consistency adds latency and reduces availability compared to eventual consistency
    
• Durability:
    Persistent Storage: All event data is durably stored in Kafka/Kinesis logs before being processed and written to DBs and DWs.
    Database Snapshots/Backups: Regular backups and replication of critical data stores.
     ^nCc7wxqz

7. Additional Features (Evolution of Design)

Advanced ML Algorithms
Contextual Targeting
Cross-Device Targeting
Multi-Objective Optimization
A/B Testing Framework
 ^SU1hIx13

System Assessment

Security 
Data Encryption
Access Control :RBAC
API Security : OAuth/JWT
Fraud Prevention: Continuous monitoring and updating of fraud detection rules and models to counter evolving threats
Data Privacy : (GDPR, CCPA)

Monitoring & Testing : 
Real-time Monitoring : Deploy a comprehensive monitoring solution (e.g Prometheus & Grafana) to collect metrics (QPS, latency, error rates, resource utilization) from all services and databases
Distributed Tracing: Implement distributed tracing to track request across microservices, crucial for debugging latency issues
Logging: Centralized logging (e.g ELK)
Alerting:
Automated Testing:


  ^H0FzVYAg

Design Ad-Serving System ^pSwdyAKG

%%
## Drawing
```compressed-json
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
```
%%