---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
Design a Recommendation Engine

 ^RAqZudfF

Clarifying Questions 

What is being recommended?

Is real-time recommendation required?

What data sources are available?

What is the expected user scale?

What is the read/write ratio?
 ^4GlWVQ0e

1. Requirements:

        • Functional
            - Personalized Recommendations : Users should receive relevant recommendations (e.g products, videos, articles) based on their past interactions and preferences. 
            - Real-time Recommendations : Recommendations should update quickly to reflect recent user activity.
            - Content Discovery : Introduce users to new items they might like but haven't explicitly shown interest in.
            - Cold Start Problem Handling : Provide reasonable recommendations for new users or new items with limited interaction data
            - User Interaction Logging : Track user views, clicks, purchases, ratings and other relevant actions 
            - Admin Tools: Ability to blacklist items, promote specific content and analyze recommendation performance.
        
        • NonFunctional
            - Availability (high) >> Consistency (eventual) : High availability (e.g 99.99 %) to ensure recommendations are always available.
            - Latency : Low latency for serving recommendations (e.g < 100 ms for real-time requests)
            - Scalability :
                    Users : Support millions to billions of users (e.g 1 billion DAU)
                    Items : Handle millions to billions items (e.g 500 million items)
                    Recommendation Requests : Process high QPS (e.g 100K QPS)
           - Data Freshness : Recommendations should reflect recent user activity within minutes (e.g 5 -10 minutes)
           - Accuracy/Relevance : High accuracy in recommendations (measured by metrics like click through rate, conversion rate) ^ACvg4Tpz

2. Core Entities

    • User : 
        { - user_id (PK)
          - preferences : (e.g categories, genres, explicit ratings)
          - interaction_history:
          - user_embedding : vector
         }

    • Item :
        { - item_id (PK)
          - title
          - description
          - category 
          - metadata : (e.g genre, tags, price)
          - item_embedding : vector
        }

    • Interaction : 
        { - interaction_id: UUID (PK) 
          - user_id (FK)
          - item_id (FK)
          - event_type : (e.g view, click, purchase, like, dislike)
          - timestamp : datatime
          - rating : int(optional, 1-5) 
            } ^szTrORGz

3. High Level Components

    • Data Ingestion & Processing
        - Event Tracker : Collect user interaction data
        - Real time Stream Processor : Processes events for immediate updates (e.g Kafka) 
        - Batch Processor : Processor historical data for model training and feature generation (e.g Spark or Hadoop)
    
    • Feature Store: Stores raw and engineered features for users and items.
    
    • Model Training & Management :
        - Model Training Service : Trains recommendation models (e.g collaborative filtering, content-based, hybrid) using processing data
        - Model Registry : Stores and and version controls trained models

        - Model Deployment Service : Deploys trained models to inference servers.

    • Recommendation Service (Inference) :
        - Recommendation API Gateway : Entry point for recommendation requests
        - Candidate Generation : Generate a large pool of potential items (using embeddings, ,matrix factorization).
        - Ranking: Ranks candidate items based on predicted relevance.
        - Filtering & Business Rules: Applies rules (e.g blacklist, already seen, promotions).

    • DataBase:
        - User/Item MetaData Store : Low latency NOSQL DB for metadata (e.g Cassandra, DynamoDB)
        - User Interaction History : Time-series DB or OLAP store for interactions (e.g ClickHouse, Pinot)
        - Vector DB : Stores user and item embeddings for similarity search (e.g Milvus, Pinecone)
        - Offline Data Warehouse : Stores raw and aggregated data for analytics and model training (e.g S3, BigQuery).
    
    • Caching Layer :
        - Distributed cache for frequently accessed recommendations and user profiles ( e.g Redis, Memcached).
        
         ^eehJzHMB

4. System Interface (API Endpoints) 

    - GET /recommendations/{user_id}:
        • Request : { user_id,
                         limit,
                         context : (e.g current_time, device_type)
                       }
        • Response : [{
                            item_id,
                            score, 
                            rank 
                        }]         


    - POST /events
        • Request : {
                       user_id: string,
                       item_id : string,
                       event_type: string,
                       timestamp : datatime,
                       details : JSONB
                       }
        • Response : {status: (e.g "success")}
    
    - GET /item/{item_id} : (Internal service call to fetch item metadata)
        • Request : {item_id}
        • Response : {
                       item_id,
                       title,
                       description,
                       category
                       metadata : JSONB                       
                       }
                        
         ^dK7uujQX

5. Data Flows
    
   1. User Interaction : User interacts with the application
   2. Event Ingestion : User actions (views, clicks, purchases) are captured by the Event Tracker and sent to the
                            Real-time Stream Processor (e.g Kafka)

   3. Real-time Feature Update : The Real-time Stream Processor updates user profiles and item popularity in the Feature Store
       and User/Item Metadata Store and potentially updated embeddings in the Vector DB.

   4. Recommendation Request : Application calls the Recommendation API Gateway with `user_id`.

   5. Recommendation Generation: 
        • The API Gateway forwards the request to the Recommendation Service.
        • Recommendation Service fetches the user profile and history from the Feature Store and User/Item Metadata Store
        • Candidate Generation component uses user emebeddings or item-item similarity from the Vector DB and User Interaction
           History to generate a diverse set of candidates.
        •Ranking components uses the deployed ML model (from Model Registry) and features from the Feature Store to score candidates
        • Filtering & Business Rules are applied
   
    6. Recommendation Served : Ranked and filtered items are returned to the user via the API Gateway. 
    7. Offline Processing for Training: Historical interaction data from User Interaction History is periodically processed by the Batch Processor (e.g daily) 
                                              and stored in the Offline Data Warehouse.

    8. Model Training : The Model Training Service pulls data from the Offline Data Warehouse and Feature Store to train new models.
    9. Model Deployment : New trained models are stored in the Model Registry and deployed to Recommendation Services instances by the Model Deployment Service.
  ^DjDYOkdY

6. Scalability & Optimization
    
    • Sharding :
        - User Data : Shard user data (profile, history) by `user_id` across databases.
        - Item Data: Shard item data by `item_id`.

    • Caching : 
        - Recommendation Cache : Cache top-N recommendations for active users in Redis/Memcached.
        - User/Item Profile Cache : Cache frequently accessed user and item features.

    • Asynchronous Processing:
        - Event Ingestion: Use MQ (Kafka) for decoupling event producers from consumers.
        - Model Training : Long-running model training jobs run asynchronously.

   • Data Structures for Candidate Generation:
        - Approximate Nearest Neighbors (ANN) : Use libraries like Faiss or specialized vector db for efficient similarity search with embeddings
        - Inverted Index : For context-based filtering, index items by features to quickly retrieve similar items.
    
   • Load Balancing: Distribute incoming requests across multiple Recommendation Service instances.

   • Microservice Architecture : Decompose the system into independent service for better scalability & maintainability

   • CDN : If recommending media, use a CDN for fast delivery of content

  • Model Optimization:
        - Online Learning : Incrementally update models with new data in real-time
        - Model Quantization/Pruning : Reduce model size and inference time.
        - GPU/TPU Acceleration: Use specialized hardware for faster model inference, especially for deep learning models.

 
         ^mOxcqCS7

Recommendation Service Cluster ^rak5vKgW

Recommendation API Gateway ^e8Suz6UM

(5a) Candidate Generation\n(embeddings, ANN, MF) ^ecQrpCWQ

(5b) Ranking\n(ML model scoring) ^LLtgHHAG

(5c) Filtering & Business Rules\n(blacklist, seen, promos) ^0KBqIpKH

5.2 fetch candidates ^IWl4qELk

Model Training & Management ^JdGpYL5E

Model Training Service\n(collab/content/hybrid) ^RPC32qOL

Model Registry\n(model versions) ^zQ2LsplJ

Model Deployment Service ^T1ASkXiJ

8.1 store ^upvtio6t

9. Deploy ^yHVLk5D3

Databases & Storage ^o3C2hJGn

Feature Store\n(user/item features) ^TYSi7kuP

User/Item Metadata Store\n(Cassandra/DynamoDB) ^XZZhM4Jl

User Interaction History\n(ClickHouse/Pinot) ^BmobKmzF

Vector DB\n(Milvus/Pinecone/Faiss) ^atIDZMJc

Offline Data Warehouse\n(S3/BigQuery) ^aTLCbbVB

Distributed Cache\n(Redis/Memcached) ^0CrVwAhL

Data Ingestion & Processing ^OvtyYqC8

Event Tracker\n(SDK/Gateway) ^4B2hW6VA

Real-time Stream\n(Kafka/Pulsar) ^NP1BHeEC

Real-time Processor\n(Flink/Spark Streaming) ^G1os5jzN

Batch Processor\n(Spark/Hadoop) ^k8Dh54is

2. Event Ingestion ^Adsk9njc

Core Entities ^p3k8FLFK

User\n(user_id, preferences, embedding) ^PpvVnCmR

Item\n(item_id, title, metadata, embedding) ^MyJLtNKH

Interaction\n(interaction_id, user_id, item_id, event_type, timestamp, rating opt) ^k0cnsPo9

has ^yRGHFdpH

targeted by ^pX1VOimQ

Admin Tools\n(blacklist, promote, analytics) ^z091T5Zs

CDN (if media delivery) ^X3zJIbsc

User / Client App ^8c1VaWVv

High Availability 99.99% ^AGVBmbut

Latency <100ms ^qs2B5BXA

Data Freshness 5-10 min ^nqP2Ijkw

Scale: 1B users, 500M items, 100k QPS ^MfBfLbFX

1. User Interaction ^OJmdzePo

3a ^6nbgue0d

3b ^YC2JlIvY

3c ^UpHgVKYt

3d invalidate ^pTzSDrl2

4. GET /recommendations ^jqdz0R8t

5.1 check cache ^mnZm6kxI

5.2.1 ANN search ^RHhFz9HE

5.2.2 history ^3CSOdJm2

5.2.3 features ^r4KEaxVC

5.3 model fetch ^O1n0ns3z

5.4 features ^hChVpKc7

5.5 rules/meta ^suthF39D

5.6 cache top-N ^OUIlqg6d

6. Return Recommendations ^8Q72EKIZ

7. Batch extract ^uYrV6Xbv

8. Train model ^0gT3FiMe

Performance & Bottleneck

• Bottleneck: The Recommendation Service (inference) is likely to be the primary bottleneck due to high QPS and complex ML computations.

• Performance :
       - Candidate Generation : Crucial for latency. 
                                      ANN search and pre-computed similarity matrices are key

       - Ranking : Model complexity directly impacts latency. Simpler models for real-time, complex models for offline

       - Data Retrievel : Fast access to user profiles, item metadata and interaction history is essantial.
                             Use in-memory caches and optimized databases

        - Event Processing : Ensure real-time stream processors can handle peak event rates to maintain data freshness. ^KLecAX9y

System Assessment

    • Security:
        - Authentication & Authorization: Secure API endpoints, ensure only authorized services/users can access data

        - Data Encryption : Encrypt data at rest and in transit (TLS)

        - Privacy : Anonymize user data where possible, comply with privacy regulations (e.g GDPR, CCPA)

        - Input Validation: Santize user input to prevent injection attacks

    • Monitoring & Metrics:

        - Metrics : Monitor key performance indicators (QPS, latency, error rate, cache hit ratio, model accuracy)

        - Logs : Centralized logging for all services

        - Alerts : Set up alerts for anomalies or threshold breaches (e.g high latency, low model accuracy).


    • Testing: 
        
        - Unit & Integration Tests : For individual components and service integrations
        
        - Load Testing: Simulate high traffic to identify bottlenecks

        - A/B Testing : Evaluate different recommendation algorithms and models in production.

        - Offline Evaluation: Evaluate model accuracy using historical data  ^An9QEZKe

Trade-offs & Evolution


• Real-time vs Batch Processing : Real-time recommendations offer freshness but are more complex and expensive
                                          Batch processing is simpler but has higher latency.
                                          A hybrid approach is optimal


• Model Complexity vs Latency : More complex ML models might offer better accuracy but increase inference latency.
                                        Choose models appropriate for latency requirements.


• Accuracy vs Diversity : Overly accurate models can lead to filter bubbles. Introduce diversity mechanism (e.g exploration alogorithms,
                                item categories) to expose users to new content.


• Cold Start Handling : 

            - For New Users : Recommend popular items, content-based recommendations based on initial preferences, or demographic-based recommendations

            - For New Items : Content-based filtering, item-items similarity based on metadata, or manual promotion.


• Data Consistency vs Availability : Prioritize availability for recommendations; eventual consistency for user profiles is often acceptable.

• Infrastructure Costs : Highly scalable and real-time systems can be expensive.
                               Optimize resource utilization (e.g spot instances, auto scaling)


• Future Evolution :

          - Deep Learning Models : Incorporate more sophisticated deep learning models for better embedinggs and ranking

          - Reinforcement Learning : Use RL to optimize recommendations based on user feedback loops

          - Contextual Awareness : Integrate real-time contextual signals (e.g time of day, location, device) into recommendations

          - Multi-Objective Optimization: Optimize for multiple goals beyond clicks (e.g long-term engagement, revenue) ^LV8dd777

%%
## Drawing
```compressed-json
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=
```
%%