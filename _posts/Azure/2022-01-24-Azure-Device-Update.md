---
title:  "OTA Update for Edge and IoT devices in Azure"
date:   2022-01-24 12:33:22 +0530
permalink: /_posts/azure/iot/azure-device-update
categories:
  - Azure IoT
  - IoT
  - Edge IoT
  - Cloud
  - Video Analytics
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Azure IoT
  - IoT
  - Edge IoT
  - Cloud
  - Video Analytics
author: Akhilesh Moghe
show_author_profile: true
---

<style>
r { color: Red }
o { color: Orange }
g { color: Green }
y { color: yellow}
</style>

## Device Update Service in IoT Hub
- It is a service that enables you to deploy *<u>over-the-air updates (OTA)</u>* to your IoT devices.
- It supports OTA updates for __*sensors*__ to __*Gateway*__ devices. Few supported vendors:
  - STML4 series
  - NXP
  - Renesas
  - Microchip
  - Supports __*Azure IoT Edge*__ devices
  - Device update agent is Open Source to be port it to any platform, by default it supports for __*Ubuntu 18.04 amd64*__ and __*Raspberry Pi Yocto image*__.
  - Support for Linux and RTOS

- Update Types: 
  - __*<u>Image based</u>*__:
    - Seems to be complete OS image update
    - Avoids issues of packages and their dependencies management.
    - __*Atomic*__ in nature
    - Can adopt __*A/B failover model*__
    - [Azure Percept uses a dual-image partition, which uses Azure Device Update Feature](https://techcommunity.microsoft.com/t5/internet-of-things-blog/how-to-build-a-resilient-over-the-air-update-solution/ba-p/2163991).
    ![azure-device-update-a-b-failover](/assets/images/azure/iot/device-update/azure-device-update-a-b-failover.png)
  - __*<u>Package based</u>*__:
    - Targeted updates that update only a specific component or application on device.
    - Can be used in *<u>Patch Updates</u>*.
      - [Critical Security Patch Update for Sudo Command](https://www.youtube.com/watch?v=7IKMKWuRq8k&t=203s)
    - May be used for *<u>Kernel Patch Update</u>*
      - [KernelCare a CloudLinux company can update Live Kernel patches without rebooting or any downtime](https://www.youtube.com/watch?v=ADyYR8jo7FU&ab_channel=KernelCare)
  - Supports __*Device group*__ update Rollouts
  - __*Schedule Updates*__
  - __*Programmatic APIs*__ for automation and custom portal
  - __*Status*__ views across devices fleets
  - __*RBAC*__ and Subscription based controls from Azure portal. 
  - __*<u>On-premise</u>*____* Microsoft *____*<u>Content Cache</u>*__ and __*<u>Nested Edge</u>*__ support for updates to *<u>disconnected devices</u>*.
  - *<u>Update Management and Reporting tool</u>*
    - Ability to pin-point the failed device
    - Details of failed update
  - __*<u>Device Update Agent</u>*__ Workflow:
  ![device-update-agent-workflow](/assets/images/azure/iot/device-update/device-update-agent-workflow.png)

- __*<u>Importing Update artifacts in Azure IoT Hub System</u>*__:
  - Supports Single Update (~package/artifact) per device.
  - __*Full-image updates*__ that update an __*entire OS partition*__ at once, or
  - An __*apt Manifest*__ that describes all the packages you want to update on your device.
  - Update Package Management may consist of
    - An import manifest describing the update.
    - update file(s).
  - Use __*Device Update REST APIs*__ or __*Azure portal*__ to import update package in system.
    - The *<u>Update artifacts are hosted in a </u>*__*<u>Azure Storage container</u>*__.
    - Device Update uploads the files, processes them, and makes them available for distribution to IoT devices. 
  - :x: __*<u>Delta Updates</u>*__ is not yet a feature for Azure Device Update, but may come in future: 
    - “To reduce the update size and accommodate larger edge devices and those in bandwidth-constrained environments, the Azure Percept team is engaging with partners and investigating feature developments.” 
  - With [*<u>SWUpdate</u>*](https://swupdate.org/) as an installer, we get an <o>Offline-USB based Update</o> (Image partitions also) feature in Azure ecosystem.
  - [Azure Percept](https://azure.microsoft.com/en-us/services/azure-percept/) uses *<u>Device Twin</u>* messages to communicate and start the Device Update.
  - __*<u>Delivery Optimization Agent</u>*__ downloads the payload and the __*<u>SWUpdate Agent</u>*__ installs the update.

&nbsp;
- *<u>References</u>*:
  - [Critical Security Patch Update for Sudo Command](https://www.youtube.com/watch?v=7IKMKWuRq8k&t=203s)
  - [KernelCare a CloudLinux company can update Live Kernel patches without rebooting or any downtime](https://www.youtube.com/watch?v=ADyYR8jo7FU&ab_channel=KernelCare)
  - [end-to-end image update using Device Update for IoT Hub on a Raspberry Pi 3 B+ device](https://docs.microsoft.com/en-us/azure/iot-hub-device-update/device-update-raspberry-pi)
  - [Device Update for IoT Hub (Preview) Overview](https://docs.microsoft.com/en-us/azure/iot-hub-device-update/understand-device-update)
  - [Azure Percept uses an atomic A/B image update to update the host operating system (OS) and firmware (FW) using Device Update for IoT Hub](https://techcommunity.microsoft.com/t5/internet-of-things/how-to-build-a-resilient-over-the-air-update-solution/ba-p/2163991)
  - [Azure Device Update Agent uses SWUpdate Project](https://swupdate.org/testimonials)
  - [Image based updates are handled by SWUpdate](https://docs.microsoft.com/en-us/azure/iot-hub-device-update/update-manifest#update-handler-types)
  - [Package based updates are handled by APT](https://docs.microsoft.com/en-us/azure/iot-hub-device-update/update-manifest#update-handler-types)

  ---

## Disconnected Device Update with Azure IoT: 
- Used to update IoT devices behind Edge Gateway, which are not connected to Azure IoT Hub in cloud.
- Also supports Edge Gateway behind Edge Gateway i.e., nested Gateways scenario.
- :x: Available as a Preview only yet.

![Disconnected Device Update with Azure IoT](/assets/images/azure/iot/device-update/disconnected-device-update.png){:.shadow}

&nbsp;
- *<u>References</u>*:
  - [Understand support for disconnected device updates](https://docs.microsoft.com/en-us/azure/iot-hub-device-update/connected-cache-disconnected-device-update)
  - [How to Update Disconnected IoT Devices](https://channel9.msdn.com/Shows/Internet-of-Things-Show/How-to-Update-Disconnected-IoT-Devices)

  ---

## Firmware Update for MCUs (non-RTOS) connected to IoT/Edge devices:
- For MCUs, that can run [Azure RTOS](https://docs.microsoft.com/en-us/azure/rtos/), [Device Update for IoT Hub]() service provides *<u>OS image-based updates</u>*.
  - [Azure RTOS for STM32, Renesas](https://github.com/azure-rtos/samples)
- MCUs which do not run RTOS will usually be categorized as the devices which cannot be connected and cannot have an identity in IoT Hub. 
- Such MCUs will use IOT Edge devices in __*Translational Gateway Pattern*__ and in turn can be configured in *<u>Protocol translational pattern</u>* or *<u>Identity translational pattern</u>*.
- Such MCUs are usually connected to Edge/IoT devices over __*serial protocol*__.
- If *<u>Identity translational pattern</u>* is used, *<u>each such MCU will have its own identity and Device Twin in IoT Hub</u>*.
- As MCU has its own Device Twin, a standard firmware update flow can be established. Just the difference will be that, the *<u>parent IoT/Edge device will have to own the responsibility of Device Twin interaction, Download, verify, apply firmware images</u>*.
- __*<u>firmware update flow</u>*__:

  ![azure-iot-firmware-update-overview](/assets/images/azure/iot/firmware-update/azure-iot-firmware-update-overview.png)

&nbsp;
- Reference:
  - [Implement a device firmware update process](https://docs.microsoft.com/en-us/azure/iot-hub/tutorial-firmware-update)

  ---

