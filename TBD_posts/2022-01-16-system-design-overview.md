---
title:  "System Design Overview"
date:   2022-01-16 12:33:22 +0530
#permalink: /_posts/system_design
categories:
  - System_Design & Architecture
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - System_Design & Architecture
author: Akhilesh Moghe
show_author_profile: true
---

This article is an overview of System Design explaining the basic, fundamental building blocks of a system. A system is a collection of varios technologies, components that interact with each other to fulfill certain set of requirements defined for the system. System design caters to and can be combination of cloud applications, desktop only applications, smartphone applications, IoT applications, etc. System design of every different type of application will be significantly differnt than the other, e.g. Desktop only applications and IoT device & cloud application will be totally different, which will deal with differnt sets of requirements and technologies.

## System Design Components:

  |---
  | Logical Entities | Tangible Entities </br> (Technologies) |
  |:-|:-|
  | Data | Images, Texts, Videos |
  | Databases | MongoDB, MySQL, Cassandra... |
  | Applications | Java, Go, Python, ReactJS... |
  | Cache | Redis, MemCache... |
  | Message Queues | Kafka, RabitMQ... |
  | Infrastructure | AWS, Azure, GCP... |
  | Communication Protocols | TCP/IP, HTTPS, MQTT... |
  | Security | TLS, Symmetric/Asymmetric Encryption... |
  | APIs/RPCs | REST, gRPC, XML-RPC... |
  | UI | AngularJS, HTML5... |
  |---
  
## Client Server Architecture
- Thick Client Vs Thin Client
  - A Thick Client is that does the processing that would have otherwise done by the server.
    - e.g., MS Outlook Application, Video Editors
  - If the processing happens on the server, and client is just pushing data & receiving response, then it's a Thin Client.
    - e.g., Netflix, Hotstar App
    
- 2-Tier... N-Tier Architecture:
  - The basic client-server architecture has 2 tiers only, Client & Server.
  - But usually, the server functionality can be broken down in different components, like Data + Logic, in that case, we have now 3-tier architecture, as: Client --> Logic/Application --> Data.
  - There can be multiple layers/tiers on the server-side like: Load Balancer + Caching + Queues, etc. Then that complex architecture becomes N-Tier Architecture.

## Proxies
- Proxy is something that works/connects on your behalf.
- 2 Types of Proxies:
  - Forward Proxy
    - This is say a machine, which is on the client side of Client-Server Architecture.
    - It talks to the client on behalf of the client. Client is shielded, not directly exposed to the server in this case.
    - Useful for anonymity, server never knows the IP address of the client.
    - Used to block malicious traffic from reaching an origin web server.
    - Used to block access to certain sites.
    - Improves user experience by caching external site contents.

  - Reverse Proxy
    - This is say a machine, which is on the server side of Client-Server Architecture.
    - Acts as a middle-man for all servers behind the proxy.
    - Client only knows and communicates to the proxy server.
    - Used to scrub all incoming traffic before reaching to the backend servers.
    - Used for Load Balancing
    - Used to cache responses from the server.
    - Useful in mitigating the cyber attcks on web-servers like DDoS Attack.
    - Provides a single configuration point to manage TLS/SSL.

## Data & Data Flow
- Data Representation
  - Business/User Layer - Data is - Text, Images, Videos...
  - Application Layer - Data is - JSON, XML...
  - Network Layer - Data is - Packets...
  - Data Stores - Data is - Tables, Columns, Indexes, Lists, Trees...
    - Databases
    - Queues
    - Caches
    - Indexes
  - Data Flow mechanisms
    - APIs
    - Messages
    - Events
  - Types of Data
    - User Created
      - Using applications like NOtepad, Calender, E-Commerce websites...
    - Internal System Generated Data
      - Application Logs, Metadata, Events...
    - Insights
      - User browsing/purchase history, Invoices...
  - Factors affecting data storage
    - Type of Data
      - System dealing with Streamming Video/Audio will be significantly differnt than Systems dealing with Text/Images data.
      - It will affect the decision about database selection.
    - Volume
      - Systems dealing with few thousand users and text based data will be designed differntly than the system which is streamming video/audio contents, as sheer volume of data will be totally differnt.
      - This affects the Scalability of the application, or database.
    - Consumption & Retrival
      - Frequency of Read & Writes, number of consumers & generator will affect the design.
    - Security
      - Encryption Requirements, Data in Transit and at Rest.
  - Examples of few systems:
    - User Management
      - Tabular data
      - Low Volume
    - Streamming Systems
      - High Volume
      - High Data Retrieval
    - E-commerce websites
      - High transactions
      - Atomicity of transaction
    - Heavy Compute Systems
      - AI/ML applications
      - High volume data generation, storage
      - processing data

## Databases
- __*Types of Databases*__
  - Relational
    - Factors for Selecting Relatioanl DBs
      - Schemas
        - Represents the Structure of the Data
        - Schema Constraints
          - PRIMARY KEY
            - Every Table has a Primary Key
            - It's an Unique ID, which identifies every row in a table uniquely.
          - NOT NULL
          - FOREIGN KEY
            - It is an ID/PRIMARY KEY of some other table.
          - INT
          - VARCHAR
          - DEFAULT
      - ACID property
        - A - Atomicity
          - A Transaction is either Successful completely or a Failure.
        - C - Consistency
          - At any point of time, State of the DB will be Consistent.
          - e.g. If two objects are reading simultaneously any entry in DB, they will always get the same result.
        - I - Isolation
          - Two Transactions are totally independent.
          - Transactions Do Not know about each other.
          - e.g. If Read and Write are happenning at the same time to the same entry, both transactions are independnt and do not interefere in each other, if Read happens before write, it will get the older value only as Write is yet not completed.
        - D - Durability
          - Guarantees that all transactions completed are stored properly in a Persistent manner, permanently.
      - Represent Complex data in simple tabular format.
      - :heavy_check_mark: Schema Constraints ensures NO Garbage, NULL values get populated in DB.
      - :heavy_check_mark: Relational DBs strictly follow Schema Constraints.
      - :x: Schema should not be required to change much.
      - :x: As the Datasize grows, it incresingly becomes complex to add the new tables and modify the schema.
      - :x: If the Queries require to fetch different properties from different tables, the Joints can become expensive.
      - :heavy_check_mark: Good for Vertical Scaling.
      - :x: Difficult to Scale Horizontally.
      - e.g.: Employees Info, Department Info, 
  - Non-Relational/No-SQL DBs
    - :heavy_check_mark: Schema is not Rigidly fixed.
      - Key-Value Stores
        - Very Fast, Quick Access
        - Mostly these are in-memory DBs.
        - Caching Solutions use Key-Value stores.
        - Application or Configuration data is stored in these.
        - Request-Response can be stored in K-V store.
        - __*Redis*__
        - __*DynamoDB*__
        - __*Memcached*__
      - Document DBs
        - :heavy_check_mark: Used if the Schemas, fields of data are going to be evolved overtime.
        - :heavy_check_mark: No Fixed Schemas. Schemaless Dynamic data.
        - :heavy_check_mark: Supports Heavy Read/Writes.
        - :heavy_check_mark: Highly Scalable.
        - :heavy_check_mark: Sharding Capability
        - :heavy_check_mark: Supports Aggregation queries to fetch data more efficiently.
        - :x: No Schema Constraints, so no Validity of the entries. Applications need to handle data sanity and validity. Documents can have empty values.
        - :x: Usually Do Not support ACID Transactions.
        - Collections <===> Tables
        - Documents <===> Rows
      - Column DBs
        - They are somewhat between Relational DBs and Document DBs.
        - Usually have sort of fixed Schemas.
        - :x: Do Not support ACID Transactions.
        - Used for Heavy Writes.
        - Columns are created based of the kind of Queries required, e.g. Songs_liked_by_user, 
        - Good Support for Distributed Databases.
        - e.g.: Event Data, Streaming Data, IoT Data, Tracking Data
        - __*Cassandra*__
        - __*HBase*__
      - Search DBs
        - Index based searches.
        - Usually used in combination with Relational or Document DBs, where Primary Data resides and the Search DBs only have the frequent Queries indexed.
        - __*Elastic DB*__
    
  - File DB
  - Network DB
  - Time-Series DBs
  - & many more...


###### References:

