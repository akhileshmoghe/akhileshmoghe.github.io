---
title:  "Why IoT Edge Computing? "
date:   2021-10-23 12:33:22 +0530
permalink: /_posts/edge/why_edge_computing
categories:
  - IoT
  - Cloud
  - Edge Computing
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - IoT
  - Cloud
  - Edge Computing
author: Akhilesh Moghe
show_author_profile: true
---


## Introduction
This article highlights the key factors that will drive the need of Edge Compute in ever growing field of IoT.


## Why Edge Compute?
### Latency: 
* IoT systems generate huge data from sensors.
* Sending all these raw data every time to cloud is a costly affair in terms of money, latency and many more.
* The round time for sending data from IoT devices to cloud and receiving back the inference data/command from cloud usually take 100s of milliseconds.
* There are many real-life use-cases, where this much of latency is not acceptable, usually in these cases inference needs to happen under few milliseconds. 

### Fast Response Time: 
* Decisions can be made locally on devices with the stream of data arriving.
* With local decision-making capability, there’s no round trip of data.
* Resulting in Fast Response. 

### Data Privacy: 
* In many industries such as Healthcare, Industrial, Government Organizations, customers do not want to send data to public cloud or outside their data centers for data privacy and safety concerns.
* Also, regulations like __*HIPAA*__, __*GDPR*__ mandates the data privacy and security, which is a strong motive behind keeping data on-premises servers. 
* Though data privacy, safety and security can be addressed in cloud infrastructures with Multitenancy and similar practices, it may not be acceptable to all customers.

### Bandwidth:
* As the data generated and frequency increases, the data to be synced will increase.
* This definitely results in increased bandwidth consumption. 
* There may be restrictions on Network Bandwidth allocation per device or network. 

### Cost: 
* IoT systems generate huge data from sensors.
* Sending all these raw data every time to cloud is costly affair in terms of money, latency and many more. 

### Pre-processing: 
* IoT systems generate huge data from sensors.
* Sending all these raw data every time to cloud is costly affair in terms of money, latency and many more.
* So usually, the raw IoT sensors data is pre-processed, transformed, fine grained before sending it over to data centers.
* Many of the IoT devices, do not have that much of compute capability, e.g., MCUs, in these scenarios, data pre-processing can be done on the Edge devices before cloud.
* Edge devices can also act as a translation bridge between different protocols, e.g., Industrial protocols like CAN Bus, Modbus, OPC-UA are not directly compatible with cloud data storage protocols like JSON. Edge devices can handle these protocol translations.

### No direct connectivity, Gateway kind of functionality: 
* In many scenarios, IoT devices will not be directly exposed to the internet.
* IoT devices are only connecting to the gateway devices, which provide them local connectivity, but no internet access.
* In such scenarios, IoT devices needs to send data to gateway i.e., Edge devices.
* And Edge device then sends data to cloud.
* In these cases, Routers or the mobiles acting as hotspots can be Edge devices.

### On-premise AI, ML Inference scenarios:
* Allows you to deploy models built and trained in the cloud and run them on edge devices.
* Edge computing uses this model to process data locally and respond to the event rapidly.

### Intermittent Connectivity:
* No one can guarantee continuous internet access.
* There will be many situations of intermittent connectivity.
* Though internet connectivity is intermittent, we can have local uninterrupted connectivity to IoT devices.
* Having Edge processing, we can have AI, ML inferences on Edge devices, without bothering about internet cloud connectivity.

 
## IoT Architecture Limitations:
### Today’s IoT Approach:
* In today’s prevailing IoT architecture, sensor data is transmitted over a wide area network to be centralized, processed and analyzed — which creates an additional supply of enriched data.
* These data and analytical models are intended to trigger actions either on the thing itself, in upstream business systems, or in other platforms that can use the data outside of the original context.

### Challenges in IoT Approach:
* IoT approach is unsustainable in the long term, due to:
  * the number of device connections,
  * volume of data,
  * latency across different locations and networks,
  * and the asynchronous nature of many connections between data flow and analytical cloud services.
  * In addition, today’s heterogeneous networks are unable to manage the massive growth anticipated in the number of endpoints and the data volume. 

These challenges have created a pressing need to move information and analytical models closer to the source of data, in order to provide compute capability inside an environment where connectivity and response times can be controlled.
That's where Edge Computing comes into picture.


## LF EDGE - Open Edge
- Linux Foundation Umbrella Project
- [LF EDGE Overview Doc](/assets/docs/edge-computing/open-source-projects/LF-Edge-web-July2021-1.pdf)
- [EdgeX Foundry Vertical Solutions Doc](/assets/docs/edge-computing/open-source-projects/EdgeX Vertical Solutions WG 20200714.pdf)


