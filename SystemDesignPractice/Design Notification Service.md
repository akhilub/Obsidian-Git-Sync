---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
Design a notification system that can send notifications via multiple channels.

Requirements:

Support multiple channels (email, SMS, push, in-app)
Handle notification preferences per user
Support notification templates
Handle 10M notifications per day
Track delivery status and analytics
Support notification scheduling

Follow-up Questions:

How would you handle notification failures and retries?
How would you implement notification batching?
How would you handle rate limiting for different providers?
How would you implement A/B testing for notifications? ^QVjhTGPE

1. Requirements

    • Functional
        - Multi Channel delivery : Support sending notifications via email, SMS, push notifications and in-app messages
        - User preference management : Allow users to opt-in/out of specific notification types or channels and
                                               define preferred delivery times
        - Notification Scheduling : Enable sending notifications at specific future times or based on predefined conditions
  

        -- out of scope--
        - Notification templates : Support customizable templates for different notification types and channels, including
                                        dynamic content
        - Tracking delivery status and analytics : Record the status of each notification (sent, delivered, failed, opened)
                                                            and collect engagement metrics
        

    
    • Non-Functional
        - High Scalability : Handles 10M notifications/day
        - Reliability : Ensure at-least-once delivery for critical notifications and minimize message loss
        - High Availability : (99 % uptime)
        - Low Latency : (<500ms)
        - Fault Tolerant

   -- out of scope--
        - Extensibility : Easy to add new notifications channels, templates and features in the future
        - Cost Efficient
        - Security ^2pSFO7bh

2. Core Entities
    
    • User 
        { - userId
          - email
          - phone_number
        }

    •  NotificationPreference 
        { -preferenceId (PK)
          - userId (FK)
          - notification_type (e.g. 'order_update', 'promotional', 'security_alert'
          - channel_preference (JSON :{email : true, sms : false, push : true, in_app : true}
          - timestamps
            
    }

    • Notification
        { - notificationId (PK)
          - userId (FK)
          - eventId( Reference to the event that triggered the notification)
          - timestamp
          - priority (e.g 'high', 'medium', 'low')
          - status(e.g 'pending', 'sent', 'delivered', 'failed')
          - metadata (JSON)


          - templateId (FK to NotificationTemplate.templateId)        
    }

  • NotificationTemplate
        { - template_id
          - notification_type
          - channel
          - metadata (JSON)
    }
    
    • NotificationAnalytics 
        { - analyticsId (PK)
          - notificationId(FK)
          - event_type
          - timestamp 
          - metadata (JSON)
        }
        
     ^JxsNtD9o

3. High Level Components

    • Notification Service API Gateway :
    
    • Notification Ingestor Service : 

    • User Preference Service : 
    
    • Notification Queue (Message Broker): 
    
    • Notification Dispatcher Service 

    • Channel-Specific Worker Service
    
    • External Notification Providers : Third Party APIs for sending (email, sms, push)
    
    • DataBase (NoSQL) : Cassandra well-suited for high-volume
    
    • Cache 

    • Scheduler Service: 
    
    • Rate Limiter Service :
    
    • Monitoring & Alerting System :

----------------------------------------------
    • Analytics Service
    • Template Service: ^Qmw5KL5h

4. System Interface (API Endpoints)

    • Internal API (for microservices to trigger notifications):
        
        - POST /notifications/send :
                 body: JSON ->
                 Response : notificationId <-

        - GET /notifications/status/{notificationId}
                Response : Current status of a notification, delivery details <-
    
    • External API (for user preference management) :
        
        - GET /users/{user_id}/preferences

        - PUT /users/{user_id}/preferences
                body : JSON      ->
                Response : 200 ok <-

        
       ------------------------------------
        - GET/notifications/templates
            

     ^oNQETYqe

5. DATA FLOWS

    1. Client Or Internal Service : Send Notification Request to API Gateway

    2. Ingestion & Validation

    3. Dipatching

    4. Channel Specific Delivery :

    ---------------------------------
    5. Status & Analytics ^2Dr3zd4L

Client or Internal 
Service ^eAdRZwtJ

API Gateway ^UyJYbcrg

Notification Ingestor Service ^uwjxsiwy

Notification
    Queue5 ^pTbgpxT0

message ^VLrEycRM

Notification Dispatcher Service ^dU1ILSRf

-user_preference ^k8zEH5xB

DataBase ^aEpV5n2G

Scheduler Service ^u1xqN5Bd

User Preference Service ^tsACnpiT

determines
prefererred channel & delivery window ^jvXdUvTb

scheduled notifications ^9ZWEkgAC

- scheduled_at : timestamp ^mTwjgHFT

Channel Specifi Worker Service ^wYbqfeJx

ready-to-send message ^hC62vodz

EXTERNAL APIS ^CeHSvFcy

WebSockets ^YBTv7dNW

In-app notification ^HhXqqCHf

6. Scalability & Optimization

• Scaling :
    
    - Horizontal Scaling : 
    - Message Queue : 
    - Database Sharding:
    - MicroService Architecture
 
• Optimization :
    
    - Caching : User Preferences 
    - Batching 
    - Rate Limiting 
    - Asynchronous Processing 
    - Prioritization
    - Retry Mechanism
        - Exponential Backoff
        - DLQ (Dead Letter Queue)

    - Connection Pooling
    - CDNs for Assets 
    -------------------
    - Template Pre-compilation
     ^m8GHNUQ8

- Load Balancer
- rate limiting
- authentication ^Hk5UQJYq

Performance & Bottleneck

    • Bottlenecks:
        - External Provider Rate Limits: 
        - Database writes 
        - Message Queue Throughput
        ------------------
        - Template Rendering

    • Performance:
        - Latency
        - Throughput ^XAPbJJOi

System Assessment (Security, Monitoring, Testing) 

• Security : 
    - Data Encryption :Encrypt sensitive user data (e.g phone_numbers, emails)
    - Authentication & Authorization : 
    - Third Party Integration : 
    - Strict Input Validation :  ^FuRLWrhD

Design Notification Service ^XQ7HJkR9

Short Solution ^aaWAR874

5. Data FLows

1. Client calls POST/notifications (via API Gateway)
2. API Gateway forwards to Notification Service
3. Notification Service validates, fetches prefrences, renders templates and publishes messages to Message Queue.
4. Sender Services (Email, SMS, Push) consume from Message Queue, integrate with third-party providers and send notifications
5. Status updated can be sent back to the Notification Service via a dedicated queue ^YuqHAGn0

4. System Interface (API Endpoints) :
    
    - POST / notifications : Create and send a new notification
    - GET / notifications/{id} : Retrieve notification status.
    - PUT /user_preferences/{user_id} : Update user notification preferences.
    - GET /templates : Retrieves available templates ^rN7wp0y9

3. High-Level Components:
    
    - Notification Service : Core logic, template rendering, preference management.
    - Message Queue : Buffering, Decoupling (e.g Kafka, RabbitMQ)
    - Sender Services : Email Sender, SMS sender, PUSH Notification Gateway.
    - Database : Store notifications, preferences, templates
    - API Gateway : Entry Point for external systems. ^KnU5to7g

1. Requirements:
    
    • Functional : 
        - Send notifications via email ,SMS, push. 
        - Support various templates and real-time delivery.

    • Non-Functional :
        - High availability
        - Low Latency
        - Scalability
        - Reliability
    ----------------------------------------
        - Personalization
        - Security ^g1fJBu0p

2. Core Entities :

    • User:
        { user_id :
          email :,
          phone_number :
        }
    
    • Notification : 
        { - notification_id (PK) :
          - user_id (FK) :
          - type :
          - channel:
          - status: (e.g pending, sent , failed) 
          - template_id (FK):
          - timestamp:
            }

    • UserPreference :
        { - userId :
          - channel :
          - enabled :
            }

    • Template :
        { - template_id:
          - name:
          - content:
          - variables: 
            }
     ^HeznJYXx

%%
## Drawing
```compressed-json
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

eQ4AGmUApXCA0Rml0ryCAAA=
```
%%