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