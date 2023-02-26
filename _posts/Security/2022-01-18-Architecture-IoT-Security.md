---
title:  "Architecting a Secure IoT Solution"
date:   2022-01-18 12:33:22 +0530
permalink: /_posts/iot/security/architecture
categories:
  - IoT
  - Cloud Computing
  - IoT Security
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - IoT
  - Cloud Computing
  - IoT Security
author: Akhilesh Moghe
show_author_profile: true
---

The goal of every IoT solution is to create an infrastructure that will enable ease of use, flexibility, automated patching, and security. Security should be built into the fabric of each layer of your design.

## Ask Yourself
To begin to think in terms of Secure IoT Solution, identify the ansfers to the following question with respect to the solution you are building:
- *<u>Depending on Device Type</u>*:
  - __*What types of devices are you deploying at the edge?*__
    - Structural automation devices, such as cameras, sprinkler systems, or thermostats.
    - Industrial IoT devices, such as sensors to detect oil spills, mechanical failures, temperature readings, or GPS tracking. 
    - Medical devices or wearable health monitoring devices.
    - Tags used to monitor and track items used for patient care or vials of medicine in a freezer.

  - __*How to update firmware and software?*__

  - __*How will you list the device physical locations and how you will track and update existing and newly deployed devices?*__

  - __*How will you understand and track how the devices behave, and How will you audit their behavior to identify when they deviate from their normal behavioral patterns?*__

- *<u>Depending upon the Security issue your device can face</u>*:
  - __*What type of Security incidents can happen to you devices?*__
  
  - __*Do you need to care about Physical security breach of the device?*__

  - __*How will your soultion tackle situation in which bad actors have got access to internal system of your device?*__
  
  - __*What can go wrong in your overall solution if the security credentials are exposed? Do you have plan to revoke access?*__

- *<u>Dealing with a Compromised Device</u>*:
  - __*Can a bad actor access your environment from within if the a device is compromised?*__

  - __*What will you do when a device is compromised?*__

  - __*Is it a device you have physical access to, or is it a remote sensor on a shipping container?*__ 

  - __*How will you know when the device starts to behave in an unexpected manner?*__

  - __*Will you have an analytics mechanism to detect and notify of unexpected behaviors?*__

  - __*Do you have a quarantine process to take the device offline?*__

- *<u>Dealing with Business Impact of the Security Breach</u>*:
  - __*What is the customer and business impact if you experience a security breach?*__
  
  - __*How many customers would be impacted by each type of breach?*__

  - __*To what degree would the customers be impacted based on the device type?*__
    - A personal fitness device that stops tracking a person's steps *<u>is a nuisance</u>*.
    - A hacked smart house door-lock, where the owner can't get inside at 2:00 AM, creates __*a security risk*__.
    - A person with diabetes who has their insulin pump compromised and their Personal Health Information (PHI) exposed to the internet could mean that the __*<u>company is breaking both compliance, federal law, or both</u>*__. 

  - __*What can be done to reduce recovery time?*__


## Designing a Secure Solution
- __*AWS Well-Architected Framework*__ helps identify the pros and cons of decisions while building systems on AWS. With this, you can incorporate architectural best practices for designing and operating reliable, secure, efficient, and cost-saving systems in the cloud. It provides a way for you to consistently measure your architectures against best practices and identify areas for improvement.
- The framework is based on five pillars: __*Operational excellence*__, __*Security*__, __*Reliability*__, __*Performance efficiency*__, and __*Cost optimization*__.
- When architecting technology solutions, informed trade-offs are made between the pillars based upon your business context.
- When a breach occurs, a well-designed architecture must have a well-tested response plan and a team that is prepared to act.
- Reference:
  - [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)

## Layers of IoT Solution
- __*Edge Layer*__:
  - Consists of the physical devices, the embedded operating systems, and the device firmware.

- __*Provisioning Layer*__:
  - Consists of the *<u>Public Key Infrastructure (PKI)</u>* used to create *<u>unique identities for devices</u>*
  - The *<u>process by which firmware is first installed on devices</u>*.
  - The *<u>application workflow that provides configuration data to the device</u>*.
  
- __*Communication Layer*__:
  - Handles the connectivity, message routing among and between devices and the Cloud.
  - The communication layer lets you establish how *<u>IoT messages</u>* are sent and received by devices, and how *<u>devices represent and store their physical state</u>* in the Cloud.
  
- __*Data Ingestion Layer*__:
  - Collecting and Aggregating sensor data from devices while decoupling the flow of data from the communication between devices.

- __*Analytics Layer*__:
  - Processes and performs analytics on IoT data.
  
- __*Application Layer*__:
  - Ease with which data generated by IoT devices can be consumed by other relevant cloud native capabilities/applications.
  - The Device Management applications is to create scalable ways to operate your devices after they are deployed in the field.
  

