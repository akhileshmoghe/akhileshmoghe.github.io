---
title:  "Azure IoT Hub Device Provisioning Service Overview"
date:   2022-03-01 12:33:22 +0530
permalink: /_posts/azure/iot/azure-iot-hub-device-provisioning
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

- The Azure IoT Hub Device Provisioning Service is a *<u>helper service for IoT Hub</u>*. The device provisioning service enables customers to *<u>provision millions of devices in a secure and scalable manner</u>*.

## Features of the Device Provisioning Service
- Secure attestation support for both __*X.509*__ and __*TPM-based identities*__.
- Enrollment list containing the complete record of *<u>devices/groups of devices</u>* that may at some point register. The enrollment list contains information about the *<u>desired configuration of the device</u>* once it registers, and it can be updated at any time.
- Multiple __*allocation policies*__ to control how the Device Provisioning Service assigns devices to IoT hubs in support of your scenarios.
- *<u>Monitoring and diagnostics logging</u>* to make sure everything is working properly.
- __*Multi-hub support*__ allows the Device Provisioning Service to *<u>assign devices to more than one IoT hub</u>*. The Device Provisioning Service can talk to *<u>hubs across multiple Azure subscriptions</u>*.
- __*Cross-region support*__ allows the Device Provisioning Service to *<u>assign devices to IoT hubs in other regions</u>*.
- __*Encryption*__ for __*data at rest*__ allows data in DPS to be encrypted and decrypted transparently using __*256-bit AES*__ encryption, one of the strongest block ciphers available, and is FIPS 140-2 compliant.
- *<u>Cross-platform support</u>*:
  - The Device Provisioning Service, like all Azure IoT services, works *<u>cross-platform</u>* with *<u>several operating systems</u>*.
  - Azure offers *<u>open-source SDKs in various programming languages</u>* to facilitate connecting devices and managing the service.
  - The Device Provisioning Service supports the following protocols for connecting devices:
    - HTTPS
    - AMQP
    - AMQP over web sockets
    - MQTT
    - MQTT over web sockets

## Use cases for Device Provisioning Service
- *<u>Zero-touch provisioning</u>* to a single IoT solution *<u>without hardcoding IoT Hub connection information</u>* at the factory (initial setup).
- *<u>Load-balancing devices across </u>*__*<u>multiple hubs</u>*__.
- Connecting devices to their owner’s IoT solution based on sales transaction data (__*<u>multitenancy</u>*__)
- Connecting devices to a particular IoT solution depending on use-case (__*<u>solution isolation</u>*__)
- Connecting a device to the IoT hub with the lowest latency (__*<u>geo-sharding</u>*__)
- __*<u>Reprovisioning</u>*__ based on a change in the device
- __*<u>Rolling the keys</u>*__ used by the device to connect to IoT Hub (:x: when not using X.509 certificates to connect)

## DPS service Concepts
- __*Service operations endpoint*__
  - The service operations endpoint is the *<u>endpoint for managing the service settings</u>* and maintaining the enrollment list. This endpoint is *<u>only used by the service administrator</u>*; it is not used by devices.
- __*Device provisioning endpoint*__
  - The device provisioning endpoint is the *<u>single endpoint all devices use for </u>*__*<u>autoprovisioning</u>*__.
  - The __*URL*__*<u> is the same for all provisioning service instances</u>*, to eliminate the need to reflash devices with new connection information in supply chain scenarios. The ID scope ensures tenant isolation.
- __*Linked IoT hubs*__
  - The Device Provisioning Service can only provision devices to IoT hubs that have been linked to it.
  - Linking gives the service read/write permissions to the IoT hub's device registry;
  - DPS can *<u>register a device ID</u>* and *<u>set the initial configuration</u>* in the __*device twin*__.
  - Linked IoT hubs may be in *<u>any Azure region</u>* or *<u>other subscriptions</u>* to your provisioning service.
- __*Allocation policy*__
  - Determines how Device Provisioning Service assigns devices to an IoT hub.
    - __*Evenly weighted distribution*__:
      - Linked IoT hubs are equally likely to have devices provisioned to them.
      - __*default setting*__
      - If you are provisioning devices to only one IoT hub, you can keep this setting.
    - __*Lowest latency*__:
      - Devices are provisioned to an IoT hub with the lowest latency to the device.
      - If multiple linked IoT hubs would provide the same lowest latency, the provisioning service hashes devices across those hubs.
    - __*Static configuration*__ via the enrollment list:
      - *<u>Specification of the desired IoT hub in the enrollment list</u>* __*takes priority*__ over the service-level allocation policy.
    - __*Custom (Use Azure Function)*__:
      - A custom allocation policy gives you more control over how devices are assigned to an IoT hub.
      - The device provisioning service calls your Azure Function code providing all relevant information about the device and the enrollment to your code. Your function code is executed and returns the IoT hub information used to provisioning the device.


