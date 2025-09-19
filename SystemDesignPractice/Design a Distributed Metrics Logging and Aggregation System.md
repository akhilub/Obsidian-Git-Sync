---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
Let's assume the logs are generated from a distributed system, e.g, an Compute Engine compute cluster.

How to collect logs efficiently from distributed systems?

How to store and index logs for fast retrieval?

When you search for logs, what happens?

How about live streaming logs? How would that work? ^xWsX7Ehp

Design a Distributed Metrics Logging and Aggregation System ^bRDwdEDQ

1. Requirements

    • Functional:
        - Efficient Log Collection
        - Reliable Log Storage
        - Indexing and Search
        - Live Streaming
        - Aggregation and Analytics
    
    • Non-Functional:
        - High Availability
        - Scalability
        - Durability
        - Security
        - Cost-effectiveness

         ^NxsNeqse

2. Core Entities:
    
    • Log Agent : A single log entry with fields like
                { - timestamp,
                  - host
                  - service
                  - message
                  - level
                }
    • Log Source : The application or instance generating log events (e.g ans EC2 instance).
    
    • Log Stream : A continuous flow of log events from a specific source or group of sources.
    
    • Aggregated Metric: A derived value from multiple log events (e.g count of errors per minute) ^PsBocsPo

3. High Level Components:
    
    • Log Agent : Installed on each GCE (Google Compute Engine) instance to collect, filter and forward logs
    • Log Ingestion Service : A highly scalable service (e.g Apache Kafka/Google DataFlow) to recieve and buffer incoming log data
    • Log Processing/Transformation : Workers that consume from the ingestion service, parse, enrich and potentially filter logs (e.g Google Cloud Functions)
    • Log Storage : A persistant and scalable storage solution for raw and processed logs (e.g Google Cloud Storage for raw, Elasticsearch for imdexed)
    • Indexing & Search Engine : A system optimized for quering and analyzing structured and unstructured log data (e.g Elasticsearch).
    • Metrics Aggregation Service: Processes logs to extract and aggregate metrics (Prometheus, custom service)
    • Monitoring & Alerting : Visualizes aggregated metrics and triggers alert (e.g Grafana or Google Cloud Monitoring, PagerDuty)
    • Live Streaming : Provides a real-time view of log data (e.g WebSocket service consuming from Google DataFlow). ^2XW2dItF

4. API Endpoints or System Interface

    • Agent to Ingestion:
        -  POST / logs : Agents send batches of log events to the ingestion service

    • Search Engine:
        - GET /logs?query={queryString}&startTime={timestamp}&endTime={timestamp}: Retrieve logs based on search criteria 
        - GET/logs/{id} : Retrieve a specific log event

    •Metrics Service:
        - GET /metrics?name={metricName}& interval={duration} : Retrieve aggregated metric data.

    • Live Streaming Service:
        - WS/ stream/logs?service={serviceName} : Websocket endpoint for subscribing to live log steams. ^TQRfSfMg

5. Data FLows ^38Wm3URi

1. Log Agent -> Log Ingestion Service

2. Log Ingestion Service -> Log Processing/Transformation -> Log Storage

3. Log Processing/Transformation -> Log Storage(Cloud Storage)

4. Log Processing/Transformation -> Indexing & Search Engine (Elasticsearch)

5. Log Processing/Transformation -> Metrics Aggregation Service

6. Indexing & Search Engine <- Monitoring & Alerting : Monitoring tools query Elasticsearch for specific logs or patterns

7. Metrics Aggregation Service -> Monitoring & Alerting : Dashboards and alerting systems consume aggregated metrics

8. Log Ingestion Service -> Live Streaming Service (optional direct path): For live streaming, the Live Streaming Service can directly consume from a dedicated DataFlow topic or
    use a separate stream processing layer before pushing to clients via WebSockets.


 ^aly6g8qS

6. Scalability & Optimization or BottleNecks:

    • Log Agents : 
        - Batching & Compressions
        - Buffering
    
    • Log Ingestion Service:
        - Partioning
        - Replication
     
    • Log Processing/Transformation:
        - Horizontal Scaling
       - Statess workers

    • Log Storage(Data Flow)
        - Tiered Based Storage
        - Partioning

    • Indexing & Search Engine:
        - Clustering & Sharding
        - Index LifeCycle Management
    
    • Metrics Aggregation Service
           - Pre-aggregation
           - Time-series Database
    
    • Live Streaming:
            - WebSocket Load Balancing
            - Fan-Out 
      
     ^TvZqtumJ

## Embedded Files
11f9cb7ab472370184fdb9157dbc943277052725: [[Pasted Image 20250917035206_339.png]]

%%
## Drawing
```compressed-json
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
```
%%