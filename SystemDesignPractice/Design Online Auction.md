---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
Design Online Auction ^0ff315jy

🛍️ What is an online auction?

An online auction service lets users list items for sale while others compete to purchase them by
placing increasingly higher bids until the auction ends, with the highest bidder winning the item. ^VZ8GTMX9

1. Requirements:

    • Functional :
        - Item Listing : Sellers can list items for auction with details (description, images, starting bid, duration)
        - Bidding : Users can place bids on active auctions
        - Auction Management : Start, end and cancel auctions
        - Notifications: Notify bidders of outbid status, auction end, winning bids.     
    
    --- out of scope ---
        - User Management : Register, log in, view/edit profile
        - Payment & Shipping(basic) : Record winning bid, mark items as sold (integration with payment/shipping
                                             not deeply explored in this design)
        - Search & Browse : Search for items, browse categories

    • Non-Functional :
        - High Availability : System should always be accessible
        - Low Latency : Bid should be processed quickly
        - Scalability : Handles a large number (~10 M) of concurrent users and auctions
        - Reliability + Fault Tolerant: Bid & auction data must be accurate and persistent. Can't drop bids
        - Data Consistency : Ensure bid order and auction state are consitent
    -------------------
        - Security : Secure user data and transactions    
     ^Ue4Gjcgp

2. Core Entities

    • User
        - user _id (PK)
        - username
        - email
        - created_at
    --------------
        - password_hash
        

    • Item
        - item_id(PK)
        - seller_id (FK to User)
        - current_price
        - start_time
        - end_time
        - status (e.g active, ended ,canceled)
        - winning_bid_id (FK to Bid, nullable)
     ------------------
        - title
        - description

    • Bid
        - bid_id(PK)
        - item_id (FK to Item)
        - bidder_id(FK to User)
        - amount
        - bid_time

 ^UNnxM1M3

3. High-Level Components

    • API Gateway : Entry points for all client requests
    • Auction Service : Manage item listings, auction states and winning bids
    • Bidding Service : Handles bid placements, validation and real-time updates
    • Notification Service : Send email, SMS or in-app notifications
    • Database (Relational e.g POSTGRESQL) : Store User, Item, Bid data.
    • Caching Layer (e.g Redis) : Caches frequently accessed data (e.g active auction, items, current bids)
    • Message Queue (e.g Kafka) : Decouple service, handles asynchronous tasks (e.g notifications, search indexing)
-------------
    • User Service : Manage user authentication and profiles
    • Search Service : Indexes items for efficient search and browsing ^aDtLpF1o

4. System Interface (API Endpoints)
    • Auction Service :
        POST /items (Create a new auction)
        GET /items/{item_id}
        PUT /items/{item_id}/cancel (Seller cancels auction)
    ---------------
        GET /items?status={status}&category={} (Browse/Search)
    
    • Bidding Service:
        POST /items/{item_id}/bids (Place a bid)
        GET /items/{item_id}/bids (Get all bids for an item)
        
        WS /bids  ( Websocket for live bid updates)

    • Notification Service :
        POST /notifications (Internal endpoint for sending notifications)
        

------------------
    • User Service
        POST /register
        POST /login
        GET /users/{user_id}
        PUT /users/{user_id} ^kciS4dwz

5. Data Flows
        
        2. Seller List Items : Client -- > API Gateway --> Auction Service --> Database (write)
                                  Auction Service also publishes a message to MQ for Search Service to index
        
        3. User place bid :
              • Client --> API Gateway --> Bidding Service
              • Bidding Service validates bid, updates current price in DB via Auction Service 
                (or directly if Bidding Service has write access to `Item` table for `current_price`
              • Bidding Service publishes a "Bid Placed" event to MQ.
              • Notification Service consumes event from MQ and sends outbid notification to previous
                highest bidder
              • Bidding Service update a real-time stream (e.g WebSocket) for live bid updates.

        4. Auction Ends:
            • Scheduler triggers Auction Service to check for ended auctions.
            • Auction Service determines winner, updates `item_status` and `winning_bid_id` in DB
            • Auction Service publishes "Auction Ended" event to MQ
            • Notification Service consumes event, notifies winner and seller.
        
                
    ---------
        1. User Logs in/register : Client --> API Gateway --> User Service --> Database (read/write) ^NvsLSYty

6. Scalability & Optimization 

    • DataBase Scaling
        - Sharding : Split `Item` and `Bid` tables by `item_id` or `seller_id` to distribute load.
        - Read Replicas : Use read replicas for heavy read operations (e.g browsing items).

    • Caching: 
        - Redis Cache : Store active auction items, current bids, user sessions to reduce database load and
          improve response times.
    
    • Message Queues:
        - Asynchronous Processing : Decouple services and handle tasks like notifications, search indexing and
          audit logging simultaneously
        - Throttling/Rate Limiting : Protect services from overload, especially the Bidding Service

    • Microservices Architecture:
        - Allows independent scaling and deployment of individual services (User, Auction, Bidding, Notification)
    
    • Load Balancing :
        - Distribute incoming traffic across multiple instance of each service    
    
    • Real-time Bidding:
        - Use WebSockets for live bid update to provide a real-time user experience without constant polling

    • Monitoring & Alerting:
        - Implement robust monitoring to track system performance and set up alerts for anomalies
     ^kSTwZGog

%%
## Drawing
```compressed-json
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=
```
%%