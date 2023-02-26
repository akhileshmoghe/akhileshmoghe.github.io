---
title:  "AWS IoT Sitewise Edge"
date:   2021-10-27 12:33:22 +0530
#permalink: /_posts/aws/iot/sitewise
categories:
  - IoT
  - AWS
  - AWS IoT
  - Edge Computing
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - IoT
  - AWS
  - AWS IoT
  - Edge Computing
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
- *<u>SiteWise Edge uses </u>*__*<u>AWS IoT Greengrass</u>*__, which provides a local software runtime environment for edge devices to help build, deploy and manage applications.
- SiteWise Edge software automates the process of *<u>securely connecting</u>* to and *<u>reading data</u>* from your industrial equipment and *<u>onsite data servers</u>* or *<u>historian databases</u>*.
- SiteWise Edge collects this data using multiple *<u>industrial protocols</u>* including __*OPC-UA*__, __*Modbus TCP*__ and __*EtherNet/IP*__, which are provided *<u>as pre-packaged connectors</u>* that run on AWS IoT Greengrass.
- Once data is collected, with Sitewise Edge you can *<u>filter data streams</u>* by sampling or comparing against a specified criterion (e.g. air temperature above a user specified threshold).
  - Greengrass can also filter data with multiple __*lambda functions*__ or *<u>components</u>*.
  - :question: What does SiteWise filters that's different than what can be done with Greengrass, or SiteWise has some specific filtering mechanisms that are ready to use?
- Once the data is processed, you can send the data to AWS IoT SiteWise *<u>in the cloud</u>* and for *<u>longer term storage</u>* and analysis in your industrial data lake you can send to other AWS Cloud services such as __*Amazon S3*__ and __*Amazon Timestream*__.
- local applications can call AWS IoT SiteWise query APIs on the SiteWise Edge software to read asset time series data, and computed transforms and metrics.

### Data Ingestion:
- AWS IoT SiteWise supports other data ingestion methods, including the __*MQTT*__ protocol via integration with __*AWS IoT Core*__, then use the __*AWS IoT Core Rules Engine*__ to route messages to AWS IoT SiteWise.
- Edge or Cloud Applications can send data to AWS IoT SiteWise using __*REST APIs*__ also.

### Gateway Management:
- Configure and Monitor Edge Gateways(~that run SiteWise Edge software) across all facilities and view a *<u>consolidated list of active gateways</u>*, through the console or using APIs. You can also monitor the health of gateways remotely.
- Gateway metrics about health, status, performance can also be viewed in __*AWS CloudWatch Matrics*__ console.

### SiteWise Monitor:
- AWS IoT SiteWise includes the ability to create __*no-code*__, *<u>fully-managed web applications</u>* using SiteWise Monitor *<u>for visualizing and interacting</u>* with operational data from devices and equipment connected to __*AWS IoT*__.
- *<u>Discover and display</u>* Asset data, computed metrics + *<u>historical time series data</u>* ingested and modeled in AWS IoT SiteWise.
- Visualize these data as *<u>charts</u>*, threshold these charts.
- Integrate *<u>Single-Sign-On</u>* with SiteWise Monitor.
- Administrators can create one or more web applications to share data, accesses across organizations, users.
- SiteWise Monitor web applications can also be *<u>deployed locally on SiteWise Edge</u>* software to visualize equipment data locally in case of intermittent connectivity.

### Alarms:
- You can define, setup and update *<u>alarm rule and notifications</u>* for equipment performance issues on any asset data property using AWS IoT SiteWise Console, Monitor or SDKs.
- You can configure __*Email*__, __*SMS notifications*__ and Severity of alarms.
- can also configure additional actions to other AWS services including __*AWS Lambda*__, __*Amazon Simple Queue Service (SQS)*__, and __*Amazon Simple Notification Service (SNS)*__, to be executed when an alarm triggers.

### Extensibility:
- Custom edge and cloud applications can use *<u>query APIs</u>* to easily retrieve asset data and computed metrics from the AWS IoT SiteWise __*time series data store*__.
- use a *<u>publish/subscribe interface</u>* to consume a near real-time stream of structured IoT data.
- Custom edge applications can also call the same *<u>AWS IoT SiteWise query APIs on the SiteWise Edge</u>* software running on-premises to retrieve asset data and metrics, without relying on connectivity to the cloud.


