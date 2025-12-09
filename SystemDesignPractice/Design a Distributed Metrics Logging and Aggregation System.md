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

Hello World ^GEncZNTK

## Embedded Files
11f9cb7ab472370184fdb9157dbc943277052725: [[Pasted Image 20250917035206_339.png]]

%%
## Drawing
```compressed-json
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
```
%%