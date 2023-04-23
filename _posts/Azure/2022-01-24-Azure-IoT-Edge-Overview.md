---
title:  "Azure IoT Edge Overview"
date:   2022-01-24 12:33:22 +0530
permalink: /_posts/azure/iot/azure-iot-edge-overview
categories:
  - Cloud Computing
  - Azure
  - IoT
  - Edge Computing
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Cloud Computing
  - Azure
  - IoT
  - Edge Computing
author: Akhilesh Moghe
show_author_profile: true
sidebar:
 nav: IoT
---

- Azure IoT Edge runtime operates as Gateways, providing a connection between other IoT devices on the network and IoT Hub.
- IoT Edge hub module acts like IoT Hub so can handle connections from other devices that have an identity with the same IoT hub.

## Decision Criteria for using IoT Edge:
- __*<u>Near real-time response to local changes</u>*__:
  - If your application needs to react quickly to local changes in near real time.
  - IoT Edge can run modules locally on IoT Edge devices to enable faster response to local changes.
- __*<u>Deploy and manage Edge Application using Containers</u>*__:
  - If your application needs to be deployed in *<u>docker compatible containers</u>* to IoT Edge devices.
  - IoT Edge enables you to use containers to run your logic at the IoT Edge. Containers help to manage software dependencies such as runtimes and libraries, ensuring that the application runs consistently wherever it's deployed.
- __*<u>Secure Edge deployments</u>*__:
  - The lack of security for IoT devices is a significant barrier to entry for many enterprises.
  - IoT Edge provides security in several ways. These include *<u>integrating with </u>*[Azure Security Center](https://docs.microsoft.com/en-us/azure/defender-for-cloud/defender-for-cloud-introduction) and by making use of any *<u>hardware security modules</u>* to provide strong authenticated connections for [*<u>confidential computing</u>*](https://docs.microsoft.com/en-in/azure/iot-edge/deploy-confidential-applications?view=iotedge-2020-11).
- __*<u>Offline or intermittent operations</u>*__:
  - If your application needs to operate with intermittent offline connectivity.
  - IoT Edge devices automatically *<u>synchronize the latest state of your devices once they've reconnected to the cloud</u>* to ensure seamless operations.
- __*<u>AI and analytics workloads on Edge</u>*__:
  - If you need to run *<u>machine learning</u>* algorithms on IoT Edge devices.
  - IoT Edge enables you to *<u>deploy models</u>* built and trained in the cloud and run them on IoT Edge devices.
- __*<u>Optimize data costs</u>*__:
  - Management of costs in the deployment of Cloud resources is essential.
  - You can design your system in such a way that *<u>data sent to the cloud is reduced by pre-processing on the IoT Edge devices</u>*.
- __*<u>Privacy and compliance for Edge deployments</u>*__:
  - If you need to ensure compliance for Privacy regulations.
  - IoT Edge can *<u>protect personally identifiable data</u>* and keep data on-premises in that way improving compliance.

## IoT Edge Gateway Patterns:
- The transparent and translation gateway patterns are *<u>not mutually exclusive</u>*.

### Transparent Gateway Pattern:
  - This type of gateway pattern is called *<u>transparent because messages can pass from downstream devices to IoT Hub as though there were not a gateway between them</u>*.
  - For *<u>devices that theoretically could connect to IoT Hub</u>* can connect to a gateway device instead.
  - These *<u>downstream devices have their own IoT Hub identity</u>*.
  - Both the *<u>devices and the users interacting with them through IoT Hub are unaware that a gateway is mediating their communications</u>*. This lack of awareness means the gateway is considered transparent.
  - You *<u>declare transparent gateway relationships in IoT Hub</u>* by *setting the IoT Edge gateway as the parent of a downstream device child* that connects to it.
  - Child devices can *<u>only have one parent</u>*.
  - Each parent can have *<u>up to 100 children</u>*.
  - IoT Edge devices can be both parents and children in transparent gateway relationships. *<u>The top node of a gateway hierarchy can have up to five generations/layers of children</u>*.
  - __Gateway Discovery__:
    - hostname of Edge Gateway
    - IP Address of Edge Gateway
  - __Secure Connection__:
    - *<u>Parent and child devices also need to authenticate their connections to each other</u>*.
    - Uses a __*shared root CA certificate*__ which the child devices *<u>use to verify that they are connecting to the proper gateway</u>*.

### Translation Gateway Pattern:
  - For *<u>devices that can't connect to IoT Hub</u>* on their own, IoT Edge gateways can provide that connection.
  - IoT Edge device has to perform processing(~like JSON to string conversions for MCUs) on incoming downstream device messages before they can be forwarded to IoT Hub.
  - Devices that don't have the capability to connect to IoT Hub on their own can connect to a gateway device instead. 
  - This pattern is required for *<u>devices that don't support MQTT, AMQP, or HTTP</u>*.

#### Pattern Illustration with Protocol vs Identity Translation:
  ![edge-as-gateway-translation](/assets/images/azure/iot/edge/edge-as-gateway-translation.png)
  
#### Protocol vs Identity Translation

  |---
  | Capability | Protocol translation pattern | Identity translation pattern |
  |:-|:-|:-|
  | __*Identities*__ <br/> (stored in the IoT Hub identity registry) | Only the identity of the gateway device | Identities of all connected devices |
  | __*Device twin*__ | Only the gateway has a device and module twins | Each connected device has its own device twin |
  | __*Direct methods*__ <br/> and <br/> __*cloud-to-device messages*__ | The cloud can only address the gateway device | The cloud can address each connected device individually |
  | [IoT Hub throttles and quotas](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-quotas-throttling?view=iotedge-2020-11) | Applies to the gateway device | Applies to each device |
  | | Only use this pattern when few devices are connecting through each field gateway, and their cloud-to-device traffic is low, <br/> as the cloud-device queue is only for 50 messages for all devices behind the gateway. | |
  |---

## IoT Edge Agent:
- IoT Edge agent is the first runtime component to start on any IoT Edge device.
- Make sure that any downstream, nested IoT Edge devices can access the edgeAgent module image when they start up, and then they can access deployments and start the rest of the module images.

## IoT Edge Runtime Installation & Update:
- Supported Tools, Platforms:
  - container runtime:
    - moby-engine (default)
    - Docker – can be used but not recommended as Microsoft will not provide any fixes for this.
  - OS:
    - Ubuntu Server 18.04 (Tier1)
    - Raspberry Pi OS Stretch (Tier1)
    - Ubuntu 18.04, 20.04 (Tier2)
    - Yocto (Tier2)
    - Debian (Tier2)
- IoT Edge Runtime is provided as an *<u>APT Packages</u>*:
  - __*aziot-edge*__:
    - IoT Edge service provides and maintains security standards on the IoT Edge device.
    - The *<u>service starts on every boot and bootstraps the device by starting the rest of the IoT Edge runtime</u>*.
  - __*aziot-identity-service*__:
    - IoT identity service handles the *<u>identity provisioning and management</u>* for <u>IoT Edge and for other device components that need to communicate with IoT Hub</u>.
  - __*moby-engine*__:
    - Container runtime 
- We can use [*<u>Device Update for IoT Hub</u>*]() service which provides *<u>APT package-based updates</u>*.
- *<u>IoT Edge runtime is like Kubernetes running on a single node</u>*, it ensures that the modules are always running and report module health to the cloud.
- Furtther References:
  - [Update IoT Edge](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-update-iot-edge?view=iotedge-2020-11&tabs=linux)
  - [Install IoT Edge](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge?view=iotedge-2020-11)

## Updating Nested Edge Gateway or IoT device Containerized Applications:
### Docker registry module:
- Docker registry module can be *<u>deployed on the IoT Edge gateway at the top layer</u>* of a gateway hierarchy.
- This module is responsible for __*retrieving and caching container images*__ on behalf of all the IoT Edge devices in lower layers.

### A local container registry module:
- The alternative to deploying this module at the top layer is to *<u>use a local registry</u>*, or to *<u>manually load container images</u>* onto devices and *<u>set the module pull policy to never</u>*.

### API proxy module:
- The API proxy module is __*required on any IoT Edge gateway that has another IoT Edge device below it*__. That means it *<u>must be on every layer of a gateway hierarchy</u>* except the bottom layer. 
- This module uses an [nginx](https://nginx.org/) __*reverse proxy*__ to *<u>route HTTP data through network layers over a single port</u>*.
- It is highly configurable through its *<u>module twin</u>* and environment variables, so can be adjusted to fit your gateway scenario requirements.
- [Route container image pulls for nested IoT Edge devices and IoT devices](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-connect-downstream-iot-edge-device?view=iotedge-2020-11&tabs=azure-portal#route-container-image-pulls)
  - The same mechanism should be usable for IoT leaf devices

### Azure Blob Storage on IoT Edge:
- The __Azure Blob Storage on IoT Edge__ can be *<u>deployed on the IoT Edge gateway at the top layer</u>* of a gateway hierarchy.
- This module is responsible for __*uploading blobs on behalf of all the IoT Edge devices in lower layers*__.
- The ability to upload blobs also *<u>enables useful troubleshooting functionality</u>* for IoT Edge devices in lower layers, like module log upload and support bundle upload.
- Further Reference:
  [Azure-Samples/azure-iotedge-blobstorage-sample](https://github.com/Azure-Samples/azure-iotedge-blobstorage-sample)

## ML model deployment to Edge Gateway:
- In Azure framework, *<u>trained ML model</u>* is deployed as a *<u>container image</u>*.
- So, the ML model deployment to IoT Edge Gateway is almost like pulling any container image from repository. 
- Reference: [Create and deploy custom IoT Edge modules](https://docs.microsoft.com/en-us/azure/iot-edge/tutorial-machine-learning-edge-06-custom-modules?view=iotedge-2020-11)
- [Train machine learning model at the edge pattern](https://docs.microsoft.com/en-us/hybrid/app-solutions/pattern-train-ml-model-at-edge)
  - It uses Azure ML services, which *<u>does not seem to be supported on Azure Stack Hub</u>*.
  - The solution needs to have active connection to Azure cloud.
- [AI at the edge with Azure Stack Hub](https://docs.microsoft.com/en-us/azure/architecture/solution-ideas/articles/ai-at-the-edge)
- [AI at the edge with Azure Stack Hub - disconnected](https://docs.microsoft.com/en-us/azure/architecture/solution-ideas/articles/ai-at-the-edge-disconnected)

## IoT Edge Security:
### IoT Edge Security Manager:
- __*Workload API*__:
  - The workload API is accessible to all modules.
  - It provides __*proof of identity*__, either as an *<u>HSM rooted signed token</u>* or an *<u>X509 certificate</u>*, and the corresponding __*trust bundle to a module*__.
    - The trust bundle contains *<u>CA certificates for all the other servers that the modules should trust</u>*.
- The IoT Edge module runtime uses an *<u>attestation process</u>* to guard this API.
  - When a module calls Workload API, the module runtime attempts to *<u>find a registration for the identity</u>*.
  - If successful, it uses the properties of the registration to *<u>measure the module</u>*.
  - If the result of the measurement process matches the registration, a *<u>new proof of identity</u>* is generated.
  - The corresponding *<u>CA certificates (trust bundle) are returned to the module</u>*.
- The module *<u>uses this certificate to connect to IoT Hub, other modules, or start a server</u>*.
- When the *<u>signed token or certificate nears expiration</u>*, it's the *<u>responsibility of the module to request a new certificate</u>*.

### IoT Edge Certificates:
- IoT Edge uses 2 different types of certificates:
  - [This article talks about the certificates that are used to secure connections between the different components on an IoT Edge device or between an IoT Edge device and any leaf devices](https://docs.microsoft.com/en-us/azure/iot-edge/iot-edge-certs?view=iotedge-2020-11).
  - [You may also use certificates to authenticate your IoT Edge device to IoT Hub. Those authentication certificates are different](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-provision-devices-at-scale-linux-x509?view=iotedge-2020-11&tabs=individual-enrollment).

### Client Server Communication between Edge – to – Edge and Edge – to – end IoT device:
- I think, we should be able to establish a secure HTTPS communication between the two devices.
- Mostly using the same Azure IoT Edge CA, device certificates.
- Few References:
  - [Azure IoT Edge supports HTTPS protocol](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-create-transparent-gateway?view=iotedge-2020-11#open-ports-on-gateway-device)
  - [video-analyzer-iot-edge-python](https://github.com/Azure-Samples/video-analyzer-iot-edge-python) - [HTTP Extension Module](https://github.com/Azure-Samples/video-analyzer-iot-edge-python/tree/main/src/edge/modules/httpExtension)
    - HTTP extension module has a Python Flask framework webserver running in container.
    - Readme shows webserver supporting HTTPS, But I did not understand how it’s serving HTTPS. But nevertheless, I guess, we can do HTTPS.
- [Workload API certificate can be used to connect to IoT Hub, other modules, or start a server](https://docs.microsoft.com/en-us/azure/iot-edge/iot-edge-security-manager?view=iotedge-2020-11#workload-api).

## Azure IoT Edge MQTT Broker:
- [IoT Edge Local Communication](https://docs.microsoft.com/en-us/azure/iot-edge/iot-edge-runtime?view=iotedge-2020-11#local-communication)
- [Publish and subscribe with Azure IoT Edge (preview)](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-publish-subscribe?view=iotedge-2020-11)

## Azure Event Grid on Azure IoT Edge:
- Purposed for Reactive, *<u>Event Driven Programming</u>*
- Service Type: *<u>Event Distribution (discrete)</u>*
- Usage: *<u>React to status changes</u>*
- *<u>Use Event Grid on IoT Edge to trigger simple workflows between modules</u>*.
  - For example, create a *<u>topic and publish events</u>* like "storage blob created" from your storage module to the topic. You can now subscribe one or several functions or custom modules to that topics.
- *<u>Extend your functionality between edge devices</u>*.
- *<u>Store and Forward mechanism</u>*:
  - Event Grid on IoT Edge module handles *<u>routing</u>*, *<u>filtering</u>*, and *<u>reliable delivery of events</u>* at scale.
  - *<u>Retry logic</u>* makes sure that the event reaches the *<u>target destination</u>* even if it's *<u>not available</u>* at the time of publish.
  - It allows you to use Event Grid on IoT Edge as a powerful store and forward mechanism.
- *<u>Event Sources</u>*:
  - __*Azure Blob Storage on IoT Edge*__
  - __*CloudEvents sources*__
  - __*Custom modules*__ & __*containers*__ via HTTP POST
- *<u>Event Handlers</u>*:
  - Event Grid on IoT Edge is built to send events to anywhere you want. Currently, the following destinations are supported:
  - Other modules including *<u>IoT Hub</u>*, *<u>functions</u>*, and *<u>custom modules</u>*
  - Other *<u>edge devices</u>*
  - *<u>WebHooks</u>*
  - [Azure Event Grid](https://docs.microsoft.com/en-us/azure/event-grid/overview) cloud service
  - [Event Hubs](https://azure.microsoft.com/en-in/services/event-hubs)
  - [Service Bus Queues](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-queues-topics-subscriptions)
  - [Service Bus Topics](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-queues-topics-subscriptions)
  - [Storage Queues](https://docs.microsoft.com/en-us/azure/storage/queues/storage-queues-introduction)
- *<u>Concepts</u>*:
  - __*Events*__ — What happened.
  - __*Event sources*__ — Where the event took place.
  - __*Topics*__ — The endpoint where publishers send events.
  - __*Event subscriptions*__ — The endpoint or built-in mechanism to route events, sometimes to more than one handler. Subscriptions are also used by handlers to intelligently filter incoming events.
  - __*Event handlers*__ — The app or service that reacts to the event.
- References: 
  - [Event Grid module on IoT Edge](https://docs.microsoft.com/en-us/azure/event-grid/edge/overview)
  - [Event Hub Vs Event Grid Vs Service Bus (in Azure Cloud)](https://docs.microsoft.com/en-us/azure/event-grid/compare-messaging-services)

## Monitoring and Troubleshooting for Azure IoT Edge (metrics-collector module):
- Measure metrics like *<u>message latencies</u>* and *<u>throughput</u>*; both between local modules and Upstream communication.
  - *<u>Upstream can be</u>* ingestion into the __*Azure IoT cloud*__ or the __*parent IoT Edge device*__ in a nested configuration.
- Right-size your hardware by analyzing *<u>resource consumption</u>* data at both workload and host level. 
- Integrate information from custom modules.
  - [Add custom metrics (Preview)](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-add-custom-metrics?view=iotedge-2020-11)
- Monitor *<u>locked-down/nested gateway assets/devices</u>*
  - In a nested edge configuration where the lower level devices are completely locked down and can only communicate upstream with their parent IoT Edge device.
  - Monitoring this critical edge infrastructure has been a challenge.
    - A solution can be configured to *<u>transport metrics</u>* using __*IoT message telemetry*__ path using the IoT Edge Hub.
    - This path enables monitoring data from assets deep in your network to securely and seamlessly make its way up to the cloud.
- [iotedge-logging-and-monitoring-solution](https://github.com/Azure-Samples/iotedge-logging-and-monitoring-solution)
  - Leverages IoT Edge Agent's built-in functionality to retrieve logs.
  - Logging architecture reference
  - Monitoring architecture reference
  - Monitor alerts architecture reference
- Integration with [Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/overview#:~:text=Azure%20Monitor%20helps%20you%20maximize,cloud%20and%20on%2Dpremises%20environments.&text=Collect%20data%20from%20monitored%20resources%20using%20Azure%20Monitor%20Metrics.) in Azure Cloud:
  - monitor both your *<u>edge</u>* and *<u>cloud</u>* resources with a *<u>unified dashboard</u>*.
  - [Azure Monitor Log Analytics](https://docs.microsoft.com/en-us/azure/azure-monitor/logs/log-analytics-overview) allows you to *<u>create alert rules</u>* at a resource group or subscription level. These broadly-scoped alert rules can be used to monitor IoT Edge devices from multiple IoT Hubs.
- Integrated the __*on-demand log pull feature*__ of the IoT Edge runtime right into the *<u>Azure Monitor</u>* Portal for quick and easy troubleshooting.
  - Pull logs on-demand, automatically *<u>adjusted to the time range of interest</u>*.

### metrics-collector module:
- monitor your IoT Edge fleet using [Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/overview#:~:text=Azure%20Monitor%20helps%20you%20maximize,cloud%20and%20on%2Dpremises%20environments.&text=Collect%20data%20from%20monitored%20resources%20using%20Azure%20Monitor%20Metrics.) and built-in metrics integration.
- [Open-source module (azure-monitor)](https://github.com/Azure/iotedge/tree/release/1.1/edge-modules/azure-monitor)
- Integrated with IoT Hub and IoT Central.
- All modules must emit metrics using the [Prometheus data model](https://prometheus.io/docs/concepts/data_model/).
- Architecture:\
  ![iotedge-logging-monitoring-with-prometheus](/assets/images/azure/iot/edge/iotedge-logging-monitoring-with-prometheus.png)

### Retrieve Logs from IoT Edge devices/gateways without SSH or Physical access:
- Retrieve logs from your IoT Edge deployments without needing physical or SSH access to the device by using the __*direct methods*__ included in the IoT Edge agent module.
- *<u>Direct methods are implemented on the device, and then can be invoked from the cloud</u>*.
- The IoT Edge agent includes following __*direct methods*__ that help you monitor and manage your IoT Edge devices remotely:
  - __GetModuleLogs__ - Retrieve module logs
  - __UploadModuleLogs__ - Upload module logs
  - __UploadSupportBundle__ - Upload support bundle diagnostics
  - __GetTaskStatus__ - Get upload request status
- Reference: 
  - [Collect and transport metrics (Preview)](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-collect-and-transport-metrics?view=iotedge-2020-11&tabs=iothub)
  - [7 ways new monitoring features help make your IoT Edge project successful](https://techcommunity.microsoft.com/t5/internet-of-things/7-ways-new-monitoring-features-help-make-your-iot-edge-project/ba-p/2374478)
  - [Retrieve logs from IoT Edge deployments](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-retrieve-iot-edge-logs?view=iotedge-2020-11)

## Further Readings
- [Simplifying confidential computing: Azure IoT Edge security with enclaves – Public preview](https://azure.microsoft.com/en-in/blog/simplifying-confidential-computing-azure-iot-edge-security-with-enclaves-public-preview/)

###### References
- [Azure IoT Edge: Documentation](https://docs.microsoft.com/en-in/azure/iot-edge/?view=iotedge-2020-11)
- [IoT Edge device as a gateway](https://docs.microsoft.com/en-us/azure/iot-edge/iot-edge-as-gateway?view=iotedge-2020-11)
- [Azure-Samples/azure-intelligent-edge-patterns](https://github.com/Azure-Samples/azure-intelligent-edge-patterns)
- [Azure IoT Samples](https://github.com/orgs/Azure-Samples/repositories?q=iot&type=&language=&sort=)

