---
title:  "AWS IoT Sitewise Edge"
date:   2021-10-27 12:33:22 +0530
permalink: /_posts/aws/iot/sitewise
categories:
  - IoT
  - Cloud
  - AWS IoT
  - Edge IoT
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - IoT
  - Cloud
  - AWS IoT
  - Edge IoT
author: Akhilesh Moghe
show_author_profile: true
---


# AWS IoT Sitewise:
- AWS IoT SiteWise is a managed service that simplifies collecting, organizing, and analyzing industrial equipment data.
![AWS IoT Sitewise Overview](/assets/images/aws/aws-iot-sitewise-edge.png)

## Features:
### Assets Modeling:

### Assets Metrics:

### SiteWise Edge:
- SiteWise Edge software runs on on-premises servers, industrial gateways, computers, AWS Outposts and AWS Snow Family devices.
- SiteWise Edge can collect-organize-process-monitor industrial equipment data locally before sending it to AWS cloud.
- SiteWise Edge uses AWS IoT Greengrass, which provides a local software runtime environment for edge devices to help build, deploy and manage applications.
- SiteWise Edge software automates the process of securely connecting to and reading data from your industrial equipment and onsite data servers or historian databases.
- SiteWise Edge collects this data using multiple industrial protocols including OPC-UA, Modbus TCP and EtherNet/IP, which are provided as pre-packaged connectors that run on AWS IoT Greengrass.
- Once data is collected, with Sitewise Edge you can filter data streams by sampling or comparing against a specified criterion (e.g. air temperature above a user specified threshold).
  - Greengrass can also filter data with multiple lambda functions or components.
  - What does SiteWise filters that's different than what can be done with Greengrass, or SiteWise has some specific filtering mechanisms that are ready to use?
- Once the data is processed, you can send the data to AWS IoT SiteWise in the cloud and for longer term storage and analysis in your industrial data lake you can send to other AWS Cloud services such as Amazon S3 and Amazon Timestream.
- local applications can call AWS IoT SiteWise query APIs on the SiteWise Edge software to read asset time series data, and computed transforms and metrics.

### Data Ingestion:
- AWS IoT SiteWise supports other data ingestion methods, including the MQTT protocol via integration with AWS IoT Core, then use the AWS IoT Core Rules Engine to route messages to AWS IoT SiteWise.
- Edge or Cloud Applications can send data to AWS IoT SiteWise using REST APIs also.

### Gateway Management:
- Configure and Monitor Edge Gateways(~that run SiteWise Edge software) across all facilities and view a consolidated list of active gateways, through the console or using APIs. You can also monitor the health of gateways remotely.
- Gateway metrics about health, status, performance can also be viewed in AWS CloudWatch Matrics console.

### SiteWise Monitor:
- AWS IoT SiteWise includes the ability to create __*no-code*__, *<u>fully-managed</u>* web applications using SiteWise Monitor for visualizing and interacting with operational data from devices and equipment connected to AWS IoT.
- Discover and display Asset data, computed metrics + historical time series data ingested and modeled in AWS IoT SiteWise.
- Visualize these data as charts, threshold these charts.
- Integrate Single-Sign-On with SiteWise Monitor.
- Administrators can create one or more web applications to share data, accesses across organizations, users.
- SiteWise Monitor web applications can also be deployed locally on SiteWise Edge software to visualize equipment data locally in case of intermittent connectivity.

### Alarms:
- You can define, setup and update alarm rule and notifications for equipment performance issues on any asset data property using AWS IoT SiteWise Console, Monitor or SDKs.
- You can configure Email, SMS notifications and Severity of alarms.
- can also configure additional actions to other AWS services including AWS Lambda, Amazon Simple Queue Service (SQS), and Amazon Simple Notification Service (SNS), to be executed when an alarm triggers.

### Extensibility:
- Custom edge and cloud applications can use query APIs to easily retrieve asset data and computed metrics from the AWS IoT SiteWise __*time series data store*__
- use a publish/subscribe interface to consume a near real-time stream of structured IoT data.
- Custom edge applications can also call the same AWS IoT SiteWise query APIs on the SiteWise Edge software running on-premises to retrieve asset data and metrics, without relying on connectivity to the cloud.


