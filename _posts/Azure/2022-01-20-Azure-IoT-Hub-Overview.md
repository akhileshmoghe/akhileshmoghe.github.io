---
title:  "Azure IoT Hub Overview"
date:   2022-01-20 12:33:22 +0530
permalink: /_posts/azure/iot/azure-iot-hub-overview
categories:
  - Cloud Computing
  - Azure
  - IoT
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Cloud Computing
  - Azure
  - IoT
author: Akhilesh Moghe
show_author_profile: true
sidebar:
 nav: IoT
---

## Azure IoT Hub
- A cloud-hosted solution back end (Platform-as-a-service, __*PaaS*__) to connect Securely and Reliably to any device in field. Provides *<u>Per-device authentication</u>*, *<u>Device Management</u>*, *<u>Scaled Provisioning</u>*.
- Available as an __*on-premises*__ solution using __*Azure Stack Hub*__.

### IoT Hub Variations
- Two tiers/packages of IoT Hub:
  - *<u>Basic Tier</u>*
    - provides a subset of features and is intended for solutions that only need __*uni-directional communication*__ from *<u>devices to the cloud</u>*.
  - *<u>Standard Tier</u>*
    - To develop full-featured and __*bi-directional communication*__ capabilities
    - device-to-cloud telemetry
    - Per-device identity
    - Message routing
    - message enrichments
    - Event Grid integration
    - support for HTTP, AMQP, and MQTT protocol
    - Device Provisioning Service
    - Monitoring and diagnostics
    - Cloud-to-device messaging
    - Device twins
    - Module twins
    - Device management
    - Device streams
    - Azure IoT Edge
    - IoT Plug and Play Preview
- Both tiers offer same kind of security and authentication. 
- *<u>IoT Hub is available in</u>* __*<u>3 sizes</u>*__, to choose from *<u>depending upon the Data Throughput</u>* you are considering. Each unit of a *<u>level 1</u>* IoT hub can handle *<u>400 thousand messages a day</u>*, while a *<u>level 3</u>* unit can handle *<u>300 million</u>*.

### Protocol support
- IoT Hub supports following protocols for device connection:
  - __*MQTT*__
  - __*MQTT over WebSocket*__
  - __*HTTPS 1.1*__
  - __*AMQP*__
  - __*AMQP over WebSocket*__

### Device Identity Registry:
- Stores information about the devices and modules permitted to connect to the IoT Hub.
- Devices needs to authenticate with the credentials stored in the registry.

### Authentication:
- Grants access to endpoints by verifying a *<u>token</u>* against the *<u>shared access policies</u>* and *<u>identity registry</u>* security credentials. 
- Supported certificates: 
  - An existing __*X.509 certificate*__
  - *<u>CA-signed</u>* X.509 certificate
  - A self-generated and *<u>self-signed</u>* X-509 certificate

### Device Twin:
- JSON documents that store *<u>device state information</u>*, including *metadata*, *configurations*, and *conditions*.
- Maintains a *<u>device twin for each device</u>* that you connect to IoT Hub.

### IoT Hub Endpoints:
- Send device-to-cloud *<u>messages</u>*
- Receive cloud-to-device messages
- Initiate *<u>file uploads</u>*
- Retrieve and update *<u>device twin properties</u>*
- Receive *<u>direct method requests</u>*
- Additional *<u>(custom) endpoints</u>* for Azure services:
  - Azure Storage containers
  - Event Hubs
  - Service Bus Queues
  - Service Bus Topics

### Device Provisioning Service:
- Enables zero-touch, __*just-in-time provisioning*__ to the right IoT Hub (~regional) *<u>without requiring human intervention</u>*, allowing the customers to provision millions of devices in a secure and scalable manner.

### Telemetry Function:
- The telemetry function involves recording and transmitting values received by an IoT device.

### Direct Method invocation from IoT Hub:
- Represent a *<u>request-reply interaction with a device</u>*.
- This approach is *<u>useful for scenarios where the course of</u>* __*<u>immediate action</u>*__ *<u>is different depending on whether the device was able to respond</u>*.
- Each device method __*<u>targets a single device</u>*__.
  - [Schedule jobs on multiple devices](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-jobs) shows how to provide a way to *<u>invoke direct methods on multiple devices</u>*, and *<u>schedule method invocation for disconnected devices</u>*.
- May have *<u>zero or more inputs/parameters</u>* in the method payload. (__*max. payload size = 128KB*__)
- Invoked through a *<u>service-facing URI</u>*: `{iot hub}/twins/{device id}/methods/`.
- *<u>Device receives direct methods through a</u>* __*<u>device-specific MQTT topic</u>*__: `$iothub/methods/POST/{method name}/` or through __*<u>AMQP links</u>*__.
- Direct methods are __*<u>synchronous</u>*__ and *<u>either succeed or fail after the timeout</u>* period (default: 30 seconds, settable between 5 and 300 seconds)
- There is *<u>no guarantee on ordering</u>* or any *<u>concurrency semantics</u>* on method calls. 
- [further details](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-direct-methods)

### Schedule Jobs on IoT devices
- Jobs execute __*<u>device twin updates</u>*__ and __*<u>direct methods</u>*__ against a set of devices at a *<u>scheduled time</u>*.
- Consider using jobs when you need to *<u>schedule and track progress</u>* any of the following activities on a set of devices:
  - *<u>Update desired properties</u>*
  - *<u>Update tags</u>*
  - *<u>Invoke direct methods</u>*
- [*<u>Job Properties</u>*](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-jobs#jobs-properties)
  - jobId 
  - startTime, endTime 
  - type: 
    - *<u>scheduleUpdateTwin</u>*: A job used to update a set of desired properties or tags.,
    - *<u>scheduleDeviceMethod</u>*: A job used to invoke a device method on a set of device twins.
  - status:
    - pending, scheduled, running, cancelled, failed, completed
  - deviceJobStatistics:
    - deviceCount
    - failedCount
    - succeededCount
    - runningCount
    - pendingCount

### Cloud-to-device communication ways
- [Direct methods](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-direct-methods)
  - Used for communications that require immediate confirmation of the result.
  - Direct methods are often used for interactive control of devices such as turning on a fan.
- [Twin's desired properties](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-device-twins)
  - Used for long-running commands intended to put the device into a certain desired state.
  - For example, set the telemetry send interval to 30 minutes. 
- [Cloud-to-device messages](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-messages-c2d)
  - Used for one-way notifications to the device app.
- [Further detailed comparison of the various cloud-to-device communication options](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-c2d-guidance)

## Azure IoT Hub on Premise:
- [Running your Azure IoT Hub based solution on-premises!](https://techcommunity.microsoft.com/t5/internet-of-things/running-your-azure-iot-hub-based-solution-on-premises/ba-p/1835867)
- [Bring your whole IoT Solution on premises](https://www.youtube.com/watch?v=8jm6RvCGkaE&list=PL1ljc761XCiYVaDEfS4X-f493capyL-cL&index=41&ab_channel=MicrosoftIoTDevelopers)

## Lambda/Serverless Architecture few points to note:
- The Lambda architecture of Azure IoT enables multiple paths for data storage and processing.
- Possible issues with Lambda architecture:
  - Duplication of data and code.
  - Greater chance of an unwanted divergence between the duplicate copies.
  - There may be code duplication in the analysis apps, if there are separate apps for the hot and cold paths.
  - Costs:
    - Fast services tend to be the more expensive, slower services cheaper.
    - There's usually a compromise to be made.

###### References
- [Azure IoT Samples](https://github.com/orgs/Azure-Samples/repositories?q=iot&type=&language=&sort=)




