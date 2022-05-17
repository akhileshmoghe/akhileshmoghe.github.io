---
title:  "Azure Event Grid Overview"
date:   2022-03-24 12:33:22 +0530
permalink: /_posts/azure/iot/azure-event-grid
categories:
  - Azure IoT
  - IoT
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Azure IoT
  - IoT
author: Akhilesh Moghe
show_author_profile: true
---

- [Azure Event Grid](https://azure.microsoft.com/en-us/services/event-grid/) is one of the Messaging Service from Azure services, which allows you to easily build applications with __*<u>event-based architectures</u>*__.
- Event Grid has built-in support for events coming from Azure services, like storage blobs and resource groups. Event Grid also has *<u>support for your own events, using custom topics</u>*.
- You can use *<u>filters to route specific events</u>* to different endpoints, *<u>multicast</u>* to multiple endpoints, and make sure your events are *<u>reliably delivered</u>*.
  ![azure-event-grid-functional-model](/assets/images/azure/messaging/event-grid/azure-event-grid-functional-model.png)

### Concepts
- There are five concepts in Azure Event Grid:
  - __*Events*__ - What happened.
  - __*Event sources*__ - Where the event took place.
  - __*Topics*__ - The endpoint where publishers send events.
  - __*Event subscriptions*__ - The endpoint or built-in mechanism to route events, sometimes to more than one handler. Subscriptions are also used by handlers to intelligently filter incoming events.
  - __*Event handlers*__ - The app or service reacting to the event.

### Capabilities
- key features of Azure Event Grid:
  - __*Simplicity*__ - Point and click to aim events from your Azure resource to any event handler or endpoint.
  - __*Advanced filtering*__ - Filter on event type or event publish path to make sure event handlers only receive relevant events.
  - __*Fan-out*__ - Subscribe several endpoints to the same event to send copies of the event to as many places as needed.
  - __*Reliability*__ - 24-hour retry with exponential backoff to make sure events are delivered.
  - __*Pay-per-event*__ - Pay only for the amount you use Event Grid.
  - __*High throughput*__ - Build high-volume workloads on Event Grid.
  - __*Built-in Events*__ - Get up and running quickly with resource-defined built-in events.
  - __*Custom Events*__ - Use Event Grid to route, filter, and reliably deliver custom events in your app.

### Event sources
- Currently, the following Azure services support sending events to Event Grid.
  - Azure API Management
  - Azure App Configuration
  - Azure App Service
  - Azure Blob Storage
  - Azure Cache for Redis
  - Azure Communication Services
  - Azure Container Registry
  - Azure Event Hubs
  - Azure FarmBeats
  - Azure Health Data Services
  - Azure IoT Hub
  - Azure Key Vault
  - Azure Kubernetes Service
  - Azure Machine Learning
  - Azure Maps
  - Azure Media Services
  - Azure Policy
  - Azure resource groups
  - Azure Service Bus
  - Azure SignalR
  - Azure subscriptions

### Event handlers
- Currently, the following Azure services support handling events from Event Grid:
  - Webhooks - Azure Automation runbooks and Logic Apps are supported via webhooks.
  - Azure functions
  - Event hubs
  - Service Bus queues and topics
  - Relay hybrid connections
  - Storage queues

### Use-cases
- Serverless application architectures
- Operations(~DevOps) Automation
- Application Integration

### Choose between Azure messaging services - Event Grid, Event Hubs, and Service Bus
- [Choose between Azure messaging services - Event Grid, Event Hubs, and Service Bus](https://docs.microsoft.com/en-us/azure/event-grid/compare-messaging-services)

###### Further References:
- [What is Azure Event Grid?](https://docs.microsoft.com/en-us/azure/event-grid/overview#event-sources)



