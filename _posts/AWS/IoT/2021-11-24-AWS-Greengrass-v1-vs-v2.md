---
title:  "Differences between AWS Greengrass v1 & v2"
permalink: /_posts/aws/iot/aws-greengrass-v1-v2
date:   2021-11-24 12:33:22 +0530
categories:
  - Cloud
  - AWS
  - AWS IoT
  - IoT
  - Edge IoT
#toc: true
#toc_label: "Contents"
#toc_icon: "file-alt"
#toc_sticky : true
full_width: true
tags:
  - Cloud
  - AWS
  - AWS IoT
  - IoT
  - Edge IoT
author: Akhilesh Moghe
show_author_profile: true
---

- AWS is supporting both AWS Greengrass V1 & V2 Edge device softwares, but both are mutually exclusive and their working is quite a different. Following is quick differences of various features in Greengrass v1 & v2:

    |---
    | Greengrass<br/> Features | Greengrass v1 | Greengrass v2
    |-|:-|:-|:-
    | Greengrass <br/> Groups | :small_orange_diamond: In GGv1, __*GG_Groups*__ define following settings in *<u>AWS Greengrass Cloud Service</u>*.<br/> :small_orange_diamond: Local devices connectivity subscriptions with GG_Core <br/> :small_orange_diamond: Subscriptions for MQTT Topics: *<u>for local devices</u>*, *<u>GG Lambda functions</u>* <br/> :small_orange_diamond: *<u>Local Secrets</u>* | :small_blue_diamond: In GGv2, we have just __*Deployments*__ <br/> :small_blue_diamond: Deployments are *<u>not necessarily done from Cloud, can be local</u>* <br/> :small_blue_diamond: Local deployments are mainly for testing purpose, but we can use Local deployments for TOTAL OFFLINE GGC functionality. <br/> :small_blue_diamond: Deployments Defines: <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_blue_diamond: Settings/Configs for GGC <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_blue_diamond: *<u>Dependencies</u>* between components <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_blue_diamond: *<u>Receipes</u>* which defines IPC, MQTT Topics
    | Greengrass Core Modularity | :small_orange_diamond: Greengrass_v1 is a *<u>Single Package</u>*, containing all GG Software <br/> :small_orange_diamond: Almost all Features are installed by default with GG installation, even when you are not using them like: *<u>Stream Manager</u>*, *<u>Secrete Manager</u>*, *<u>Log Manager</u>*, etc... <br/> :small_orange_diamond: Very few features can be optinally installed as Connectors that are deployable independently. | :small_blue_diamond: In GGv2, __*GGC Nucleus*__ is very minimal core which is a must install. <br/> :small_blue_diamond: All GGC features are available as *<u>Components</u>*, which can be deployed, as and when required. <br/> :small_blue_diamond: Stream Manager, Secrete Manager are all components, can be deployed independently.
    | GG Lambdas | :small_orange_diamond: In GGv1, everything related to Lambdas are defined in GG Group in Cloud Service like: *<u>Lambda Subscriptions</u>*, *<u>Access to local resources</u>* <br/> :small_orange_diamond: Lambda function is the *<u>main utility to create GG Core device Application</u>*. <br/> :small_orange_diamond: Few other options are: <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_orange_diamond: *<u>Containers</u>* using __*Docker Application Deployment Connector*__ <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_orange_diamond: __*Connectors*__: either *<u>AWS provided pre-built connectors</u>*, which are very few and usable for very specific use-cases | :small_blue_diamond: In GGv2, everything related to Lambdas is defined in Recipes. <br/> :small_blue_diamond: Lambda functions are just like Components. |
    | Subscriptions | :small_orange_diamond: In GGv1, all Subscriptions are *<u>defined in AWS GreenGrass Cloud Service</u>*. | :small_blue_diamond: In GGv2, Subscriptions are *<u>managed by Components in Recipes</u>*, <br/> :small_blue_diamond: *<u>Authorization policies</u>* for MQTT topics, IPC, etc... are also defined in Recipes. |
    | Local Resources | :small_orange_diamond: In GGv1, Lambdas run in a Container environment & access to local resources is restricted, managed in GG group settings, in cloud. | :small_blue_diamond: In GGv2, all components run outside any container, Lambdas also. <br/> :small_blue_diamond: No local resource restrictions. |
    | GGAD | :small_orange_diamond: In GGv1, local devices, their subscriptions are required to be defined in GG_Group in cloud and then, GG group needs to be deployed. <br/> :small_orange_diamond: To remove devices from group, again GG_Group settings have to be changed and a new deployment needs to be done to GGC. | :small_blue_diamond: In GGv2, There's No need to deploy group	to add or remove local devices. <br/> :small_blue_diamond: Authorization policy only need to be defined. |
    | MQTT broker | :small_orange_diamond: In GGv1, MQTT server/broker was part of GG core software. | :small_blue_diamond: In GGv2, __*MQTT broker*__ is a component <br/> :small_blue_diamond: __*MQTT bridge*__ is another component, required for: <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_blue_diamond: local devices to GGC communication <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_blue_diamond: Mesaages between components on core device <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_blue_diamond: Messgaes between IoT devices and AWS IoT core. <br/> :small_blue_diamond: [Moquette broker](https://moquette-io.github.io/moquette/) |
    | Shadow Service | :small_orange_diamond: GGv1 has it *<u>enabled by default</u>*. <br/> :small_orange_diamond: Need to use the Greengrass Core SDK in your Lambda functions. | :small_blue_diamond: In GGv2, __*Shadow Manager*__ component needs to be deployed. <br/> :small_blue_diamond: Need to use the AWS IoT Device SDK V2 in Lambda functions, or in custom components. |
    |---

  ---

- [__*<u>Run AWS IoT Greengrass V1 applications on AWS IoT Greengrass V2</u>*__](https://docs.aws.amazon.com/greengrass/v2/developerguide/move-from-v1.html#run-v1-applications)
  - if your v1.x applications use any of the following listed features, you won't be able to run them on the v2.0 software yet.
    - Stream manager telemetry metrics
    - The C and C++ Lambda function runtimes.
  - <u>Greengrass V1 Lambda functions on V2</u>:
    - The following is the list of Greengrass features, which are used differently in V1 & V2, so if used with Lambda functions, the deployment and usage of Lambda functions needs modifications:
      - Stream Manager
      - Local Secretes
      - Subscriptions
      - Local Volumes and devices
      - Local Shadows
      - Access other AWS Services
  - <u>AWS IoT Connectors</u>:
    - Few connectors are available as components in Greengrass_v2.
      - CloudWatch metrics component
      - AWS IoT Device Defender component
      - Kinesis Data Firehose component
      - Modbus-RTU protocol adapter component
      - Amazon SNS component
    - but some of the connectors are not available in Greengrass_v2 and cannot be used anymore in v2:
      - Serial Connector
      - Docker application deployment connector - Deployment of containers is supported as a component.
  - <u>Connect V1 Greengrass devices</u>:
    - AWS IoT Greengrass V2 support for client devices is backward-compatible with AWS IoT Greengrass V1, so you can connect V1 core devices to V2 core devices without changing their application code.
    - Deploy different Greengrass_v2 components that facilitate discovery and communication between client abd core devices like:
      - To relay messages between client devices, the AWS IoT Core cloud service, and Greengrass components (including Lambda functions), deploy and configure the __*MQTT bridge*__ component.
      - You can deploy the __*IP detector*__ component to automatically detect connectivity information, or you can manually manage endpoints.


