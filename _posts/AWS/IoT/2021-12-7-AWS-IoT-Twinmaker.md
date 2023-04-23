---
title:  "AWS IoT Twinmaker"
permalink: /_posts/aws/iot/twinmaker
date:   2021-12-7
categories:
  - AWS
  - Cloud Computing
  - IoT
  - Digital Twin
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - AWS
  - Cloud Computing
  - IoT
  - Digital Twin
author: Akhilesh Moghe
show_author_profile: true
sidebar:
 nav: IoT
---

This write-up will focus on newly launched AWS IoT Twinmaker service.

## Digital Twins
  - Digital twins are *<u>virtual representations of physical systems</u>* such as buildings, factories, production lines, and equipment that are regularly updated with real-world data to mimic the __structure__, __state__, and __behavior__ of the systems they represent.
  - To create and use digital twins of real-world systems to monitor and optimize operations.
  - We can create digital twins of __equipment__, __processes__, and __facilities__ by connecting data from different data sources like *<u>equipment sensors</u>*, *<u>video feeds</u>*, and *<u>business applications</u>*.

## Digital Twin Graph
  - AWS IoT TwinMaker forms a digital twin graph that combines and understands the *<u>relationships between virtual representations of your physical systems and connected data sources</u>*, so you can accurately model your real-world environment.
  - *<u>Import</u>* existing __3D models__ (such as *<u>CAD files</u>*, and *<u>point cloud scans</u>*) to compose and arrange __3D scenes__ of a physical space and its contents (e.g. a factory and its equipment) using simple 3D tools.
  - We can add data to these models/scenes for visualization as:
    - Interactive video.
    - sensor data overlays from the connected data sources.
    - insights from connected machine learning (ML) and simulation services.
    - equipment maintenance records and manuals.
  - Using the digital twin graph, customers can now issue geospatial queries such as finding all cameras that are pointing to an equipment to help with root cause analysis.

## Amazon Managed Grafana plugin
  - Can be used to create a web-based application for end users.
  - Use Grafana applications to observe and interact with the digital twin to help them optimize factory operations, increase production output, and improve equipment performance.
  
## Model Builder
  - Allows you to create *<u>workspaces</u>* that will hold the resources, such as entity models and visual assets needed to create a digital twin.
  - In this workspace, create entities that represent digital replicas of your equipment.
  - Specify custom relationships between these entities to create a digital twin graph of your real-world system.
  - Using the digital twin graph, customers can now issue geospatial queries such as finding all cameras that are pointing to an equipment to help with root cause analysis.
  
## Data Connector
  - Associate entities with connectors (called as *components* in AWS IoT TwinMaker) to data stores such as AWS IoT SiteWise, to provide context to the data present in various data stores.
  - Built-in __*<u>Data Connectors</u>*__ for the following AWS services:
    - __AWS IoT SiteWise__ for equipment and *<u>time-series sensor data</u>*
    - __Amazon Kinesis Video Streams__ for *<u>video data</u>*
    - __Amazon Simple Storage Service (S3)__ for *<u>storage of visual resources</u>* (for example, CAD files) and data from business applications.
  - Provides a framework for you to create *<u>your own Data Connectors</u>* to use with other data sources (such as [Snowflake](https://www.snowflake.com/) and [Siemens MindSphere](https://siemens.mindsphere.io/en)).

## Scene Composer 
  - A tool to create visualizations in 3D.
  - You can bring previously built 3D/CAD models into your resource library in Amazon S3.
  - these visual assets can be brought into a scene, and position the 3D assets to match your real-world systems.
  - visual annotations such as tags on top of the base scene.
  

##### References
  - [Introducing AWS IoT TwinMaker](https://aws.amazon.com/blogs/iot/introducing-aws-iot-twinmaker/)

##### Further Reference
  - [aws-samples/aws-iot-twinmaker-samples](https://github.com/aws-samples/aws-iot-twinmaker-samples)
  
