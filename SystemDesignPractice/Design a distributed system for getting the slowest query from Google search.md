---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
Design a distributed system for getting the slowest query from Google search

Design a distributed system that can identify and retrieve the slowest performing query from Google's search infrastructure, 
with capabilities for both historical analysis (previous day) and real-time monitoring. ^hm10CQb3

1. Requirements ^XsJOEdDx

Functional Requirements:
    - Capture Query Performance Data :Accurately record the latency of every Google search query, including start and end times, query parameters and geographical region
    - Identify Slowest Queries : Systematically determine the single slowest performing query defined over historical periods (e.g 24 hrs)
    - Historical Analysis :Store query performance data for a configurable action duration (e.g 30 days) to enable analysis of trends and regressions.
    - Real-Time Monitoring : Provide immediate alerts and visibility into currently slow queries or performance degradation.
    - Query Details Retrieval : Allow users to retrieve detailed information about identified slow queries, including execution plan, associated services and relevant logs ^MKPP6FPg

Non-Functional Requirements:
    - Scalability : Handles billions of queries per day with minimal overhead on the core search infrastructure
    - Low Latency (< 200 ms) for monitoring system : Ingest and process performance data with low latency to enable near real -time insights
    - High Availability : The monitoring system itself must be highly available and fault tolerant to ensure continuous performance tracking
    - Data Accuracy : Ensure the collected latency data is precise and reliable
    - Data Durability : Persist historical performance data against data loss
    
    -- out of scope --
    - Security : Protect sensitive query data and system configurations.
 ^uEaakamL

2. Core Entities:
    
    - QueryEvent : Represent a single search query execution, including:
        { - query_id
          - query_string
          - user_id
          - timestamp_start
          - timestamp_end
          - latency_ms
          - region
          - datacenter_id
          - service_components_involved (lists of services e.g indexing, ranking, ad-serving)
          - status_code (success/failure)
        }

    - ShowQueryRecord : A record of an identified slow query
        {
          - record_id
          - query_id
          - detection_timestamp
          - threshold_violated_ms
          - historical_period
        }

     - Performance Metric : Aggregated metrics (e.g average latency) for specific periods or dimensions. ^gxExRkjC

3. High Level Components

    - Query Interceptor / Logger  : Injects into the Google search infrastructure to capture `QueryEvent` data for every query
                                            This component should be extremely lightweight and fault - tolerant
    - Data Ingestion Pipiline (e.g Kafka / PubSub) : A high throughput, low latency messaging system to transport `QueryEvent` data from interceptors
                                                                to processing units.
    - Real-Time Stream Processor (e.g Flint/Spark Streaming) : Consumes `QueryEvent` data calculates real-time metrics, detects anomalies, and identifies
                                                                            immediate slow queries. It can also perform windowed aggregations. 
    - Historical Data Store (e.g BigTable / HBase) : A distributed NoSQL BD to store raw `QueryData` data for historical analysis, optimized for time series access
    
    - Aggregated Metric Store (e.g TimeScaleDB/Prometheus): Stores pre-aggregated performance metrics for faster dashboarding and historical trend analysis 
    
    - Slow Query Detector Service : Analyzed historical data and real-time streams to identify the slowest queries over various periods and generate `SlowQueryRecords`
    
    - Alerting Service (e.g PagerDuty/Slack): Triggers notification when slow query thresholds are breached
    
    - Dashboard & Visualization Service : Provides a UI for monitoring real-time performance, viewing historical trends and drilling down into slow query details.
    
    - API Gateway : Provides a unified interface for external systems and internal UI components to interact with monitoring systems. ^RYyl2N4E

4. API Endpoints

    - POST /v1/query_events :  Endpoint for `Query Interceptor/Logger` to send `QueryEvent` data(internal).
    - GET /v1/metrics?type=<metric>&interval=<time>&region=<region> : Retrieve aggregated performance metrics.
    - GET /v1/slow_queries?period=<time_period>&limit=<num> : Retrieve a list of the slowest queries for a given period. 
    - GET /v1/query_details/{query_id} : Retrieves all `QueryEvent` data and associated logs for a specific query
    - GET /v1/alerts: Retrieve active and historical alerts. ^ZUDngsar

5. Data Flows

    1. Query Execution : A user submits a query to Google Search
    
    2. Telemetry Capture : The `Query Interceptor/Logger` captures `QueryEvent` data during query execution.

    3. Data Ingestion : `QueryEvent` data is sent to the `Data Ingestion Pipleline`

    4. Real-time Processing : The `Real-time Stream Processor` consumes `QueryEvent`s --> Updates real time dashboards -->
        Checks for immediate slow query thresholds and sends alerts via `Alerting Service` --> Performs windowed aggregrations and pushes to `Aggregated Metrics Stores`

    5. Historical Storage: Raw `QueryEvent` data is written to the `Historical Data Store`

    6. Slow Query Detection (Batch/Scheduled) : The `Slow Query Detector Service` periodically queries the `Historical data Store` (and potentially the `Aggregated Metrics Store`)
    to identify the slowest queries over historical periods, storing results in `SlowQueryRecord`s.

    7. User Access : `Dashboard & Visualization Service` and external tools query the `API Gateway` to access metrics, slow query lists and detailed
    query information.  ^Q770xFHi

6. Scalability & Optimization

    • Data Sharding/Partitioning:
          - Query Events : Shard `Query Event` data in `Historical Data Store` by `timestamp` and `region` to distribute load and optimize time-series queries.
          - Index Sharding : Leveraged by the underlying Google Search Infrastructure, where the index itself is sharded across multiple servers.
     
    • Load Balancing : Use global and regional LB to distribute incoming query traffic and internal service requests across multiple instances.

    • Caching :
        - Result Caching : Cache frequently accessed aggregated metrics and slow query lists in the `Aggregated Metrics Store` and the `API Gateway` level.
        - Query Suggested Caching : While not directly part of this system, the core Google Search uses this, which reduces load on the underlying search infrastructure.

    • Asynchronous Processing : The `Data Ingestion Pipeline` and `Real-time System Processor` operates asynchronously to minimize impact on the core search latency.
    
    • Downsampling/Aggregation: 

    • Predicate Pushdown
    
    • Efficient Data Formats

    • Distribute Tracing Integration

    • Resource Management

    • Failure Detection & Retry Mechanism ^2hkZOFWI

Detailed Solution 1 ^lU2CIGyc

Data Ingestion 
    Pipleline ^9FMvO4C4

Real-time Processing 
    Engine ^sF3Xc2Oy

QueryEvent ^nmTDi8mK

QueryEvent ^x2VgGQNy

QueryEvent ^lnYyox3q

Identify slow
Query Event ^KeMKDtTI

     3Stores
SlowQueryRecord ^ZcozEa3m

QueryEvent ^tnuHo2rK

SlowQueryRecords22 ^BvBm277h

Dashboard Monitoring UI ^VUwuoHqa

API Services ^L0fCGd7m

1. Requirements 
    
    • Functional Requirements :
          - Identify Slowest Queries
          - Historical Analysis
          - Real-time monitoring
          - Query Details : Capture relevant metadata for each slow query (e.g timestamp, query_string, latency, region)
          - Alerting

    • Non Functional Requirements :
          - Scalability
          - Low Latency (for monitoring) 
          - High Availability
          - Durability
          - Accuracy ^uDOP500T

2. Core Entities 
    
    • QueryEvent : Reperesent a single search query, including
        {
            - query_string
            - timestamp
            - latency_ms
            - user_id
            - region
            - dataCenter_id
        }
    
    • SlowQueryRecord : Stores identified slow queries with fields like
        {
            - query_string
            - average_latency_ms
            - max_latency_ms
            - count
            - time_start
            - time_end
        } ^yKVVIZa3

3. High Level Components 
    
    • Search Frontend/Load Balancer : Ingests all search queries
    
    • Latency Measurement Agent : Embedded within or alongside the search backend, measures query latency for each request
    
    • Data Ingestion Pipeline (e.g Kafka/PubSub) : Collects `QueryEvent` data steams.

    • Real-time Processing Engine (e.g Flink/Spark Streaming) : Processes `QueryEvent`s to identify slow queries in near real-time. 

    • Historical DB (e.g BigTable, Cassandra) : Stores raw `QueryEvent` data and aggregated `SlowQueryRecord`s

    •Analytical Processing Engine (e.g Spark Batch/BigQuery) : Processes historical data for daily/weekly slowest query reports

    • API Service : Provides endpoints for retrieving slow queries (real-time and historical)

    • Dashboard/Monitoring UI : Visualizes slow query trend and details ^P8OyYSJ3

4 API Endpoints

    - POST /query_events (Internal) :
                                : Ingests `QueryEvent` data from latency management agents

    - GET /slow_queries/realtime:
                                : Retrieves currently identified slowest queries (e.g top N over the last 5 minutes)
    
    - GET /slow_queries/historical?date=YYYY-MM-DD&top=N 
                                : Retrieves top N slowest queries for a specific date

    
    - GET /query_details?query_id={id}
                                : Retrieves detailed information for a specific slow query instance
    

    - GET /slow_queries/trends?start_date=...&end_date=.... 
                                : Retrieves trends of slow queries over a time range
     ^lPdNuthL

5. Data Flows

    1. Search Frontend routes queries to Search Backend

    2. Latency Measurement Agent (in/with Backend) measures latency for each query, creates a `QueryEvent` and sends its to the Data Ingestion Pipeline

    3. Data Ingestion Pipeline streams `QueryEvent`s to the Real-time Processing Engine

    4. Real-time Processing Engine analyzes `QueryEvent`s, identifies slow ones and stores `SlowQueryRecord` in Historical Data Storage (for both real-time access
    and later batch processing). It may also push identified slow queries to the Monitoring UI or triggers alerts.

    5. Historical Data Storage also recieves raw `QueryEvents` from the Data Ingestion Pipeline for long-term retention.

    6. Analytical Processing Engine periodically reads raw `QueryEvent`s or aggreated data from Historical Data Storage to generate daily/weekly slow query reports
    and stores these back into Historical Data Storage as aggregated `SlowQueryRecord`s.

    7. API Service queries Historical DB to fulfill requests from the Dashboard Monitoring UI for both real-time and historical slow query data. ^KmsDtSLU

6. Scalability & Optimization

    • Horizontal Scaling : All stateless components (Processing Engines, API Service) can be scaled horizontally by adding more instances.

    • Distributed Data Storage : Use a distributed NoSQL DB(e.g Bigtable for high-throughput, low-latency read/writes or BigQuery for analytical queries) to handle massive
       data volumes.

    • Partitioning : Data in the ingestion pipeline and storage should be partitioned (e.g by `timestamp`, `region`) to distribute load and enable efficient querying

    • Stream Processing : Leverage distributed stream processing framework (Flink/Spark Streaming) for real-time analysis, enabling high throughput
        and low latency processing.

    • Batch Processing : Utilize distributed batch processing framework (Spark Batch) for cost-effective historical analysis.
    
    • Caching : Cache frequently accessed slow query reports (e.g "top 10 slowest queries today") at the API layer to reduce database load

    • Indexing : Proper indexing on `timestamp`, `query_string`, and `latency_ms` in the data store will optimize retrieval performance

    • Sampling : For extremely high-volume potentially sample `QueryEvent`s that are not "slow" to reduce data volume for historical analysis
                   while ensuring all "slow events" are captured.
    
    • Aggregations : Pre-aggregate data (e.g hours/daily slowest query summaries) to speed up historical reporting queries.

    • Load Balancing : Distribute incoming `QueryEvent` and API requests across multiple instances of processing engines and API services 

    • Message Queue (Kafka/PubSub) : Decouple data producers from consumers, buffer spikes in traffic and ensures data durability ^MOFVPlFk

Real-time Processing Engine ^uCaS8zMc

Analytical Processing Engine ^edg5zIns

Analytical Processing Engine ^NEoKoZRg

Historical DB ^CG8zAyIC

L ^9K4cWNUh

Latency Management Agent ^Hufk2GP3

Search Backend ^D6QPoAZt

Search Frontend ^58lUKDDK

Query ^Hyb93rVk

Search Queries ^ylwlxVz3

Short Solution 2 ^hmUCsEug

Problem:
Google Maps needs to show real-time restaurant wait times by integrating data from:

- Restaurant POS systems (3rd-party APIs).

- Google Search/Maps internal service.

- User-reported data via the mobile app.


How would you integrate external POS APIs with different formats/rates?

Sync vs async ingestion?

How do you resolve conflicts (POS vs user report)?

How would you handle stale or missing data? ^gqxIb5eT

%%
## Drawing
```compressed-json
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
```
%%