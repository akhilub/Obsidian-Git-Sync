---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
Design a File Storage & Sharing Service ^BI5A6Wij

1. Requirements

    • Functional:
        - Upload files : Users can upload various file types.
        - Download files: User can download their own files.
        - Share files : Users can share files with specific individuals or generate shareable links
        - Manage files : Users can view, rename and delete their files.

    -----------------------
        - User authentication/authorization : Secure access to user files.

    • Non Functional :
        - Availability : High availability for file access and management
        - Durability: Files must be stored reliably and not lost.
        - Scalability : Support for a large number of user (100k) and PB of data.
        - Performance: Fast upload and download speeds.
    -------------------------        
        - Security : Data encryption in transit and at rest, access control ^R4kLro7J

2. Core Entities

    • User
         { - user_id(PK)
           - username
           - email
           - password_hash  
         }
    
    • File 
        { - file_id (PK)
          - user_id (FK)
          - file_name
          - file_size
          - mime_type
          - storage_path (e.g GCS key)
          - upload_timestamp
        }

    • Share
        { - share_id (PK)
          - file_id (FK)
          - owner_id(FK)
          - shared_with_user_id (FK, nullable for public links)
          - share_token (for public links
          - permissions (e.g read, write)
          - expiry_timestamp (nullable)   
        } ^PFSO49Ki

3. High Level Components

    • API Gateway/Load Balancer : Entry point for all client requests
    
    • File MetaData Service (FMS)  : Manages file information (name, size, owner, etc) in a database
    
    • File storage Service (FSS): Handles the actual storage and retrieval of file data (e.g GCS)

    • Sharing Service (SS): Manages file sharing links and permissions.

    • Content Delivery Network(CDN) : Cache popular files for faster downloads

    • Background Worker Service (BWS) : Handles asynchronous tasks like file processing (e.g virus scanning, thumbnail generation)

--------------------------
    • Authentication Service : Handles user login, registration and token validation ^O1OhyKVu

4. System Interface (API Endpoints)

    • File MetaData Service:
        - POST /files/upload : (initiates upload, gets pre-signed URL)
        - GET /files/{file_id} : (retrieves like metadata)
        - GET /files : (list user's files)
        - PUT /files/{file_id}/rename
        - DELETE/files/{file_id}

    • Sharing Service:
        - POST /files/{file_id}/share/private : (share with user)
        - POST /files/{file_id}/share/private : (generate public link)
        - GET /share/{share_token} : (access shared file)
        - DELETE /share/{share_id}

------------------------------------------------

    • Authentication Service:
        - POST /auth/register
        - POST /auth/login
        - POST /auth/refresh-token ^p8bkwZer

5. Data Flows

    • File Upload :
        
        1. Client request `POST/files/upload` from API Gateway
        2. API Gateway routes to FMS
        3. FMS validates user (via Auth Service), creates file entry in DB, and requests a pre-signed GCS URL from FSS.
        4. FMS returns pre-signed URL to client.
        5. Client directly uploads file data to GCS using the pre-signed URL
        6. GCS notifies BWS upon successful upload
        7. BWS updates file status in FMS.

    • File Download :
        1. Client request `GET/files/{file_id}` from API Gateway.
        2. API Gateway routes to FMS
        3. FMS validates user authorization (via Auth Service and sharing permission if applicable) retrieves file `storage_path`.
        4. FMS returns pre-signed GCS download URL from FSS.
        5. FMS returns pre-signed URL (or CDN URL if available) to client.
        6. Client downloads file directly from GCS or CDN.

    • File Sharing: 
        1. Client requests `POST /files/{file_id}/share/public` from API Gateway.
        2. API Gateway routes to SS.
        3. SS validates ownership (via Auth Service), create a `Share` entry in its DB with a `share_token` and returns the token/link to the
            client.
        4. When another user accesses `GET/share/{share_token}`, API GAteway routes to SS.
        5. SS validates `share_token`, retrieves `file_id`.
        6. Sharing Service requests a pre-signed GCS download URL from FSS (via FMS) and redirects the client or provides the download link. ^NtvpWhJu

6. Scalability & Optimization

    • Horizontal Scaling : All stateless services (API Gateway, Auth, Metadata, Sharing) can be scaled horizontally by adding more instances behind a LB
    
    • Object Storage : Utilize cloud object storage (GCS, S3) which is inherently scalable, durable, and cost-effective for large amount of file data.

    • DataBase Sharding / Replication : File Metadata and Sharing Database can be sharded by `user_id` or `file_id` to handle high read/write loads.
                                                  Read replicas can offload read traffic
    
    • CDN : Use a CDN to cache frequently accessed files and serve them from edge location, reducing latency and load on origin storage.
    
    • Pre-signed URL's : Offload file upload/download traffic directly to object storage, by passing application servers.

    • Asynchronous Processing :  Use message queues (e.g Kafka) and background workers for non-critical tasks like virus scanning, thumbnail generation, and metadata
                                        indexing to prevent blocking user requests

    • Rate Limiting : To protect API endpoints from abuse and ensure fair usage

    • Monitoring & Alerting : Implement robust monitoring to detect performance bottlenecks and availability issues, with automated alerts. ^ebnlDTdP

%%
## Drawing
```compressed-json
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
```
%%