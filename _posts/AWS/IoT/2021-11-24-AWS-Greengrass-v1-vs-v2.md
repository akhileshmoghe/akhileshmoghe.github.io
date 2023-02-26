---
title:  "Differences between AWS Greengrass v1 & v2"
permalink: /_posts/aws/iot/aws-greengrass-v1-v2
date:   2021-11-24 12:33:22 +0530
categories:
  - Cloud Computing
  - AWS
  - Edge Computing
  - IoT
#toc: true
#toc_label: "Contents"
#toc_icon: "file-alt"
#toc_sticky : true
full_width: true
tags:
  - Cloud Computing
  - AWS
  - Edge Computing
  - IoT
author: Akhilesh Moghe
show_author_profile: true
---

- AWS IoT Greengrass is an Edge Computing IoT Service from AWS. AWS IoT Greengrass helps you build, deploy and manage IoT applications on devices by providing an IoT Edge Runtime and a corresponding Cloud Service.
- AWS released updated version of Greengrass in Q1-2021 & has been supporting both AWS Greengrass V1 & V2 Edge device softwares since then. But Greengrass V1 is now nearing it's End of Life, will stop being supported by AWS in Q2-2023. So it's time for us to understand the differences between the two versions of Greengrass.
- Both these versions are mutually exclusive and their working is quite a different. Greengrass V1 was Monolithic in nature, whereas Greengrass V2 follows the Micro-Services approach.
- Following are few notable differences of various features in Greengrass V1 & V2:

  |---
  | *<u>Greengrass<br/> Features</u>* | *<u>Greengrass V1</u>* | *<u>Greengrass V2</u>*
  |-|:-|:-|:-
  | __*<u>Greengrass <br/> Group</u>*__ | :small_orange_diamond: In GGv1, __*GG_Groups*__ define following settings in *<u>AWS Greengrass Cloud Service</u>*.<br/> :small_orange_diamond: Local devices connectivity subscriptions with GG_Core <br/> :small_orange_diamond: Subscriptions for MQTT Topics: *<u>for local devices</u>*, *<u>GG Lambda functions</u>* <br/> :small_orange_diamond: *<u>Local Secrets</u>* | :small_blue_diamond: In GGv2, we have just __*Deployments*__ <br/> :small_blue_diamond: Deployments are *<u>not necessarily done from Cloud, can be local</u>* <br/> :small_blue_diamond: Local deployments are mainly for testing purpose, but we can use Local deployments for TOTAL OFFLINE GGC functionality. <br/> :small_blue_diamond: Deployments Defines: <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_blue_diamond: Settings/Configs for GGC <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_blue_diamond: *<u>Dependencies</u>* between components <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_blue_diamond: *<u>Receipes</u>* which defines IPC, MQTT Topics
  | __*<u>Greengrass Core Modularity</u>*__ | :small_orange_diamond: Greengrass_v1 is a *<u>Single Package</u>*, containing all GG Software <br/> :small_orange_diamond: Almost all Features are installed by default with GG installation, even when you are not using them like: *<u>Stream Manager</u>*, *<u>Secrete Manager</u>*, *<u>Log Manager</u>*, etc... <br/> :small_orange_diamond: Very few features can be optinally installed as Connectors that are deployable independently. | :small_blue_diamond: In GGv2, __*GGC Nucleus*__ is very minimal core which is a must install. <br/> :small_blue_diamond: All GGC features are available as *<u>Components</u>*, which can be deployed, as and when required. <br/> :small_blue_diamond: Stream Manager, Secrete Manager are all components, can be deployed independently.
  | __*<u>Greengrass Lambda functions</u>*__ | :small_orange_diamond: In GGv1, everything related to Lambdas are defined in GG Group in Cloud Service like: *<u>Lambda Subscriptions</u>*, *<u>Access to local resources</u>* <br/> :small_orange_diamond: Lambda function is the *<u>main utility to create GG Core device Application</u>*. <br/> :small_orange_diamond: Few other options are: <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_orange_diamond: *<u>Containers</u>* using __*Docker Application Deployment Connector*__ <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_orange_diamond: __*Connectors*__: either *<u>AWS provided pre-built connectors</u>*, which are very few and usable for very specific use-cases | :small_blue_diamond: In GGv2, everything related to Lambdas is defined in Recipes. <br/> :small_blue_diamond: Lambda functions are just like Components. <br/> :small_blue_diamond: Components either AWS-provided or your custom ones are the building blocks for GG_Core device application. |
  | __*<u>Greengrass Subscriptions</u>*__ | :small_orange_diamond: In GGv1, all Subscriptions are *<u>defined in AWS GreenGrass Cloud Service</u>*. | :small_blue_diamond: In GGv2, Subscriptions are *<u>managed by Components in Recipes</u>*, <br/> :small_blue_diamond: *<u>Authorization policies</u>* for MQTT topics, IPC, etc... are also defined in Recipes. |
  | __*<u>Local Resource Access</u>*__ | :small_orange_diamond: In GGv1, access to local resources is restricted, and managed in GG group settings in cloud. <br/> :small_orange_diamond: If any Greengrass component needs access to local resources like *<u>peripheral devices</u>* or *<u>folder access</u>*, it needs a configuration defined in Greengrass group settings. <br/> :small_orange_diamond: Lambda functions running in a Container environment needs association of local resorce access in it's own configuration. | :small_blue_diamond: In GGv2, all components run as independent processes, with their own recipes, <br/> :small_blue_diamond: So access/restrictions to local resources is also maintained in their own recipes. <br/> :small_blue_diamond: Same is the case for Lambda functions running in containers, they maintain their own configuration settings for local resources. <br/> :small_blue_diamond: So essentially, No Global Group level local resource restrictions in GGv2 as such. |
  | __*<u>Greengrass Client devices</u>*__ | :small_orange_diamond: In GGv1, local devices, their subscriptions are required to be defined in GG_Group in cloud and then, GG group needs to be deployed. <br/> :small_orange_diamond: To remove devices from group, again GG_Group settings have to be changed and a new deployment needs to be done to GGC. | :small_blue_diamond: In GGv2, There's No need to deploy group	to add or remove local devices. <br/> :small_blue_diamond: Authorization policy only need to be re-defined/modified to remove access of local devices to Greengrass. |
  | __*<u>Greengrass MQTT broker</u>*__ | :small_orange_diamond: In GGv1, MQTT server/broker was part of GG core software. | :small_blue_diamond: In GGv2, __*MQTT broker*__ is a component, [Moquette broker](https://moquette-io.github.io/moquette/) <br/> :small_blue_diamond: __*MQTT bridge*__ is another component, required for: <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_blue_diamond: local devices to GGC communication <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_blue_diamond: Mesaages between components on core device <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_blue_diamond: Messgaes between IoT devices and AWS IoT core. <br/> :small_blue_diamond: __*Client device auth*__ is another component required to: <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_blue_diamond: Authenticate client devices & Authorize client device actions. |
  | __*<u>Shadow Service</u>*__ | :small_orange_diamond: GGv1 has it *<u>enabled by default</u>*. <br/> :small_orange_diamond: Need to use the Greengrass Core SDK in your Lambda functions. | :small_blue_diamond: In GGv2, __*Shadow Manager*__ component needs to be deployed. <br/> :small_blue_diamond: Need to use the AWS IoT Device SDK V2 in Lambda functions, or in custom components. |
  | __*<u>Local MQTT broker Server Certificate Rotation</u>*__ | :small_orange_diamond: By default, this certificate <u>expires in seven days</u>. You can set the expiration to any value between <u>7 and 30 days</u>. <br/> :small_orange_diamond: For certificate rotation to occur, your <u>Greengrass core device must be online</u> and able to access the AWS IoT Greengrass service directly on a regular basis. <br/> :small_orange_diamond: The *<u>core device downloads a new MQTT server certificate</u>*. <br/> :small_orange_diamond: If the core device is offline at the time of expiry, it does not receive the replacement certificate. Client devices cannot connect to the core device until the connection to the AWS IoT Greengrass service is restored and a new *<u>MQTT server certificate can be downloaded</u>*. Though Existing connections are not affected. | :small_blue_diamond: In GGv2, *<u>Greengrass core devices generate a local MQTT server certificate</u>* that client devices use for mutual authentication. This certificate is signed by the core device CA certificate, which the core device stores in the AWS IoT Greengrass cloud. <br/> :small_blue_diamond: The *<u>core device CA certificate expires after 5 years</u>*. <br/> :small_blue_diamond: The MQTT server certificate expires every 7 days by default, and you can configure this duration to between 2 and 10 days. <br/> :small_blue_diamond: The Greengrass core device rotates the MQTT server certificate 24 hours before it expires. |
  |---

  ---

- [__*<u>Run AWS IoT Greengrass V1 applications on AWS IoT Greengrass V2</u>*__](https://docs.aws.amazon.com/greengrass/v2/developerguide/move-from-v1.html#run-v1-applications)
  - :x: *<u>if your v1.x applications use any of the following listed features, you won't be able to run them on the v2.0 software yet</u>*.
      - Stream manager telemetry metrics
      - The C and C++ Lambda function runtimes.
  - :heavy_check_mark: *<u>Greengrass V1 Lambda functions on V2</u>*:
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
    - :x: But some of the connectors are not available in Greengrass V2 and cannot be used anymore in GGv2:
      - Serial Connector
      - Docker application deployment connector - Deployment of containers is supported as a component.
  - <u>Connect V1 Greengrass devices</u>:
    - AWS IoT Greengrass V2 support for client devices is backward-compatible with AWS IoT Greengrass V1, so *<u>you can connect V1 core devices to V2 core devices without changing their application code</u>*.
    - Deploy different Greengrass V2 components that facilitate discovery and communication between client abd core devices like:
      - To relay messages between client devices, the AWS IoT Core cloud service, and Greengrass components (including Lambda functions), deploy and configure the __*MQTT bridge*__ component.
      - You can deploy the __*IP detector*__ component to automatically detect connectivity information, or you can manually manage endpoints.


