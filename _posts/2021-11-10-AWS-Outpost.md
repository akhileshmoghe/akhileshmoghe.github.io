---
title:  "AWS cloud services extended to on-premise data centres with AWS Outpost"
date:   2021-11-10 12:33:22 +0530
categories:
  - On-Premise Cloud
  - Cloud
  - AWS
  - Edge IoT
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - On-Premise Cloud
  - Cloud
  - AWS
  - Edge IoT
author: Akhilesh Moghe
show_author_profile: true
---

# AWS Outpost
- AWS Outposts is a *<u>fully managed service</u>* that extends same AWS infrastructure, AWS services, APIs, and tools to virtually any datacenter or *<u>on-premises</u>* facility for a consistent hybrid experience.
- Ideal for workloads that require __*<u>low latency</u>*__ access to on-premises systems, __*<u>local data processing</u>*__, or __*<u>local data storage</u>*__.
- AWS compute, storage, database, and other services run locally on Outposts, and you can access the full range of AWS services available in the Region to build, manage, and scale your on-premises applications using familiar AWS services and tools.
- Outposts are connected to the nearest AWS Region.
- We can run following AWS Resources on AWS Outposts on premises: 
  - __Amazon EC2 instances__
  - __Amazon EBS volumes__
  - __Amazon EKS nodes__
  - __Amazon ECS clusters__ (container-based services)
  - __Amazon RDS DB__ instances (database services)
  - __Amazon EMR clusters__ (analytics services)
  - __Amazon S3__ for AWS Outposts will be available in 2020 for local object storage on Outposts.
- AWS Outposts allows you to securely store and process customer data that needs to remain on premises or in countries where there is no AWS region.
- With AWS Outposts, we do not need to manage different APIs, manual software updates, and purchase of third-party hardware and support.
- AWS Outposts is fully managed and supported by AWS. *<u>Outpost is delivered, installed, monitored, patched, and updated by AWS</u>*.
- AWS Outposts address low latency application requirements and local data processing requirements.

## AWS Outposts Compute & Storage
- Choose from pre-validated configurations with mix of EC2, EBS, S3 capacity.
- Or contact AWS to customize configurations to meet your needs.

### Compute
  - __*<u>General purpose</u>*__ (M5/M5d):
    - a balance of compute, memory, and network resources
    - can be used for general-purpose workloads, *<u>web and application servers</u>, *<u>backend servers for enterprise applications</u>*, *<u>gaming servers</u>*, and *<u>caching fleets</u>*.
  - __*<u>Compute optimized</u>*__ (C5/C5d):
    - suited for compute intensive applications such as *<u>batch processing</u>*, *<u>media transcoding</u>*, *<u>high performance web servers</u>*, *<u>high performance computing (HPC)</u>*, *<u>scientific modeling</u>*, *<u>dedicated gaming servers</u>* and *<u>ad server engines</u>*, *<u>machine learning inference</u>*.
  - __*<u>Memory optimized</u>*__ (R5/R5d):
    - to deliver fast performance for workloads that *<u>process large data sets</u>* __*<u>in memory</u>*__.
    - suited for memory intensive applications such as *<u>high-performance databases</u>*, distributed web scale *<u>in-memory caches</u>*, *<u>mid-size in-memory databases</u>*, real- time *<u>big data analytics</u>*.
  - __*<u>Graphics optimized</u>*__ (G4dn):
    - To accelerate *<u>machine learning inference</u>*.
      - For machine learning inference for applications like *<u>adding metadata to an image</u>*, *<u>object detection</u>*, *<u>recommender systems</u>*, *<u>automated speech recognition</u>*, and *<u>language translation</u>*.
    - *<u>graphics-intensive workloads</u>*:
      - For building and running graphics-intensive applications, such as *<u>remote graphics workstations</u>*, *<u>video transcoding</u>*, *<u>photo-realistic design</u>*, and *<u>game streaming</u>* in the cloud.
  - __*<u>I/O optimized</u>*__ (I3en):
    - __*<u>Non-Volatile Memory Express (NVMe) SSD</u>*__ instance storage optimized for *<u>low latency</u>*, *<u>high random I/O performance</u>*, *<u>high sequential disk throughput</u>*, and offers the lowest price per GB of SSD instance storage on Amazon EC2.
    - Suited for *<u>NoSQL databases</u>* (Cassandra, MongoDB, Redis), *<u>in-memory databases</u>* (Aerospike), *<u>scale-out transactional databases</u>*, *<u>distributed file systems</u>*, *<u>data warehousing</u>*, *<u>Elasticsearch</u>*, *<u>analytics workloads</u>*.

### Storage
  - __*<u>Amazon EBS</u>*__:
    - __*local instance storage*__ - that gets vanished when EC2 instance is stopped.
    - __*Elastic Block Store (EBS)*__ *<u>gp2 volumes</u>* for *<u>persistent</u>* block storage.
    - snapshot and restore capabilities
    - lets you [increase volume size without any performance impact](/_posts/2021-06-30-Extend-EC2-EBS-Volume-size-without-downtime.md).
    - All EBS volumes and snapshots on Outposts are *<u>fully encrypted</u>* by default.
    - EBS is offered in tiers of 11 TB, 33 TB, and 55 TB.
  - __*<u>Amazon S3</u>*__:
    - Store, Retrieve Data on Outpost
    - Secure Data
    - Control Access
    - Tags, Reports
    - S3 APIs
    - Add 26 TB, 48 TB, 96 TB, 240 TB, or 380 TB of S3 storage capacity
    - Create *<u>up to 100 buckets per AWS account</u>* on each Outpost
  - __*<u>Amazon EBS Snapshot</u>*__:
    - a point-in-time copy of your EBS volumes.
    - Snapshots of EBS volumes on your Outpost are stored on Amazon S3 *<u>in the Region</u>*
      - in the region means in nearest cloud region? and not on local Outpost S3 storage?
        - If you have S3 provisioned on your Outpost, then you can store EBS snapshot on Outpost S3 itself, it's called *<u>EBS Local Snapshot</u>*.
    - use EBS Local Snapshots on Outposts for __*<u>disaster recovery</u>*__ and __*<u>back up</u>*__.
    - Secure and protect data on EBS storage using *<u>resource-level</u>* __*<u>IAM policies</u>*__.

## Migration
  - [CloudEndure Migration](https://aws.amazon.com/cloudendure-migration/):
    - allows customers to migrate workloads onto AWS Outposts from physical, virtual, or cloud-based sources, from on-premises locations, public AWS Regions, and other clouds to Outposts.
  - using EBS Local Snapshots on Outposts:
    - migrate workloads from any source directly onto Outposts, or from one Outpost to another, without requiring the EBS snapshot data to go through the region.
  - [CloudEndure Disaster Recovery](https://aws.amazon.com/cloudendure-disaster-recovery/):
    - business continuity solution for physical, virtual, and cloud-based workloads onto AWS Outposts.
    - you can replicate and recover:
      - from on-premises to Outposts
      - from AWS Regions onto Outposts
      - from Outposts into AWS Regions
      - and between two Outposts.
    - CloudEndure Disaster Recovery improves resilience, enabling *<u>recovery point objectives (RPOs)</u>* of seconds and *<u>recovery time objectives</u>* (RTOs) of minutes.

## Networking
### Extended VPC
  - Extend your existing __*Amazon VPC*__ to your Outpost in your on premises location.
  - Create a __*subnet*__ in your *<u>regional VPC</u>* and associate it with an Outpost just as you associate subnets with an Availability Zone in an AWS Region.
  - *<u>Instances in Outpost subnets communicate with other instances in the AWS Region using</u>* __*<u>private IP addresses</u>*__, all *<u>within the same VPC</u>*.

### Local Gateway
  - Each Outpost provides a new __*local gateway (LGW)*__ that allows you to *<u>connect your Outpost resources with your on premises networks</u>*.
  - LGW enables *<u>low latency connectivity</u>* between the Outpost and any local data sources, end users, local machinery and equipment, or local databases.

### Load Balancer
  - You can provision an __*<u>Application Load Balancer (ALB)</u>*__ to automatically *<u>distribute incoming HTTP(S) traffic</u>* across *<u>multiple targets</u>* on your Outposts, such as *<u>Amazon EC2 instances, containers, and IP addresses</u>*.
  - ALB on Outposts is *<u>fully managed</u>*, operates in a *<u>single subnet</u>*, and *<u>scales automatically</u>* up to the capacity available on the Outposts rack to meet varying levels of application load without manual intervention.

### Private Connection to AWS Cloud
  - With AWS Outposts Private Connectivity, you can establish a service link __*<u>VPN connection</u>*__ </u>from your Outposts to the AWS Region over</u> __*<u>AWS Direct Connect</u>*__.
  - Minimizes public internet exposure
  - Removes the need for special firewall configurations.

## AWS Services on Outposts:
### Containers
  - __*<u>Amazon ECS</u>*__:
    - Scalable, High-performance *<u>container orchestration service</u>*, supports *<u>Docker containers</u>*
    - run and scale containerized applications.
    - ECS eliminates the need for:
      - Install and Operate your own container orchestration software
      - Manage and Scale a cluster of virtual machines
      - Schedule containers on those virtual machines
    - With simple *<u>API calls</u>*, you can <u>launch and stop</u> Docker-enabled applications and *<u>query the complete state of your application</u>*.
    
  - __*<u>Amazon EKS</u>*__:
    - Managed Service to run Kubernetes.
    - Used to run Containerized applications.
    
  - *<u>AWS ECS vs AWS EKS</u>*:
    - [Amazon ECS vs Amazon EKS: making sense of AWS container services](/assets/docs/Amazon-ECS-vs-Amazon-EKS.pdf)

### Databases
  - __*<u>Amazon RDS (Relational Database Service) </u>*__:
    - supports *<u>Microsoft SQL Server</u>*, *<u>MySQL</u>*, and *<u>PostgreSQL</u>* database engines.
    - Amazon RDS provides cost-efficient and resizable capacity while automating time-consuming administration tasks including infrastructure provisioning, database setup, patching, and backups.
    - fully managed databases on premises
    - Amazon RDS can be managed using AWS Management Console, APIs, and CLI as if in cloud.
    - Amazon RDS enables low-cost, high-availability hybrid deployments, with disaster recovery back to the AWS Region, read replica bursting to Amazon RDS in the cloud, and long-term archival in Amazon Simple Storage Service (Amazon S3) in the cloud.

  - __*<u>Amazon ElastiCache</u>*__:
    - Fully managed *<u>in-memory data store</u>*, compatible with *<u>Redis</u>* or *<u>Memcached</u>*
    - Optimized for real-time applications with *<u>sub-millisecond latency</u>*.
    - Amazon ElastiCache on Outposts enables real-time use cases like *<u>Caching</u>*, *<u>Session Stores</u>*, Gaming, Geospatial Services, *<u>Real-Time Analytics</u>*, and *<u>Queuing</u>*.

### Data Analytics
  - __*<u>Amazon EMR</u>*__:
    - Deploys secure and managed EMR clusters.
    - Deploys latest versions of __*<u>Apache Spark</u>*__, __*<u>Apache Hive</u>*__, and __*<u>Presto</u>*__ to access critical on premises data sources and systems for big data analytics.
    - Use the EMR console, SDK, or CLI to specify the subnet associated with your Outpost to launch EMR clusters.

## Upgrades to Outpost Services
  - AWS services running locally on Outposts will be upgraded automatically to the latest version as and when available.
  - Amazon RDS like services also patch both OS and database engines within scheduled maintenance windows with minimum downtime.

## Access to Cloud hosted Regional Services
  - We can *<u>extend</u>* our __*Amazon Virtual Private Cloud(VPC)*__ on premises and *<u>run some AWS services locally on Outposts and also connect to a broad range of services available in the local AWS Region</u>*.
  - We can access all regional AWS services in your private VPC environment, for example, through Interface Endpoints, Gateway Endpoints, or their regional public endpoints.

## AWS Tools:
  - With AWS Outposts, customers *<u>can access AWS tools</u>* __*<u>running in the region</u>*__ such as, *<u>AWS CloudFormation, Amazon CloudWatch, AWS CloudTrail, Elastic BeanStalk, Cloud 9,</u>* and others to run and *<u>manage workloads</u>* on AWS Outposts the same way they do in the cloud.

## AWS Resource Access Manager
  - __*<u>AWS Resource Access Manager (RAM)</u>*__ lets customers share access to Outposts resources – EC2 instances, EBS volumes, S3 capacity, subnets, and local gateways (LGWs) – across multiple accounts under the same AWS organization.

## Further Readings
  - [AWS Outpost with AWS Greengrass Sample Architecture](https://d1.awsstatic.com/Solutions/Outposts%20Manufacturing%20Solution%20Brief%20US%20Letter%20AWS%2009.30.20%20FINAL.pdf)
  - [AWS Outpost Healthcare Sample Architecture](https://d1.awsstatic.com/re19/AWS19-0029%20Outpost%20Solution%20Brief_v7%20Final.pdf)
  - [Cloud at the Edge for Healthcare and Life Sciences](https://d1.awsstatic.com/product-marketing/Outposts/AWS%20HCLS%20eBook.pdf)
  - Yet To Read:
  - [Deploying Containerized Application on AWS Outposts with Amazon EKS](https://aws.amazon.com/blogs/containers/deploying-containerized-application-on-aws-outposts-with-amazon-eks/)
  - [Building Modern Applications with Amazon EKS on Amazon Outposts](https://aws.amazon.com/blogs/compute/building-modern-applications-with-amazon-eks-on-amazon-outposts/)
  - [Configuring an Application Load Balancer on AWS Outposts](https://aws.amazon.com/blogs/networking-and-content-delivery/configuring-an-application-load-balancer-on-aws-outposts/)
  - [Managing your AWS Outposts capacity using Amazon CloudWatch and AWS Lambda](https://aws.amazon.com/blogs/compute/managing-your-aws-outposts-capacity-using-amazon-cloudwatch-and-aws-lambda/)
  - [Run applications on premises and access Amazon S3 objects from AWS Outposts](https://aws.amazon.com/blogs/storage/run-applications-on-premises-and-access-s3-objects-from-aws-outposts/)
  - [Introducing AWS Outposts private connectivity](https://aws.amazon.com/blogs/networking-and-content-delivery/introducing-aws-outposts-private-connectivity/)
  
  


