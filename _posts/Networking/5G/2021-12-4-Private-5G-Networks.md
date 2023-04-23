---
title:  "5G: Private 5G Networks for IoT Use cases"
date:   2021-12-4 12:33:22 +0530
permalink: /_posts/networking/5g
categories:
  - 5G
  - IoT
  - Edge Computing
  - Cloud Computing
  - AWS
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - 5G
  - IoT
  - Edge Computing
  - Cloud Computing
  - AWS
author: Akhilesh Moghe
show_author_profile: true
sidebar:
 nav: IoT
---

This write-up explains different use cases of Private 5G Networks, specifically in IoT & Edge-Computing domains.

## Problems with existing Enterprise Networks
Enterprise networks are under strain, because:
  - Increasing internet connected-serving applications.
  - Increasing number of users.
  - Increasing number of Connected IoT Devices.
  - Increased Video content consumption.
  - Low Latency requirements.
  - Wired networks are inflexible and expensive to expand
  - Wi-Fi networks suffer from Coverage, Reliability & Capacity issues.


## Advantages of 5G Networks
  - Reliable network connectivity for use cases that involve device mobility.
  - Longer range than Wi-Fi networks.
  - Better coverage, especially in industrial/hospital environments.
  - Low-latency connectivity for IoT-Edge applications.
  - Full enterprise control over users, devices, network utilization and data. Implement enterprise-specific network configuration and security policies.
  - Convenience of CBRS (Citizens Broadband Radio Service) in the US with no need to acquire spectrum licenses.(US specific)

## Private 5G use-cases
  - Indoor Scenarios such as Industrial Manufacturing Hub or Hospitals or Retail stores
    - Improved coverage and reliability close to wired networks with flexibility as Wi-Fi.
    - Connect autonomous mobile robots (AMRs), automated guided vehicles (AGVs), video surveillance systems.
    - Underground deployments, where cellular network is often difficult to reach, unreliable.
      - Mining sites can be a great scenario for connectivity to Iot Sensors, video surveillance.
    - Point-of-sale (PoS) transactions, video surveillance in Retail stores.
  - Outdoor Scenarios such as Transport Hubs
    - Increase range and penetration over Wi-Fi in shipping ports, airports, train and bus terminals.
    - Reliable connectivity for IoT sensor data for predictive maintenance, video feeds for safety, push-to-talk (PTT) applications for communications.
    - Remote Places where public network services are not available like Oil-Gas, Mining IIoT sites
  - On-premise IoT-Edge-AI-ML Analysis
    - Low latency use-cases can be a reality as in case of Internet of Medical things (IoMT).
    - Real-time AI-ML diagnosis on sensors, video data.
  - Large Images/Videos data analysis
    - As in case of Radiology scans or live video, or Raw image data, which are relatively large chunks of data.
    - with 5G low latency and dedicated network without the risk of channel interference, these loads can be transmitted in real-time and analysis be performed on it or it can be made available for remote monitoring or diagnosis services.
  - AR-VR applications
    - with all of the promises of 5G networks such as Low Latency, dedicated bandwidth, AR/VR application experiances can be made production grade, deployed and simultaneously used by large gatherings.

## AWS Private 5G
  - It simplifies the procurement, deployment, and installation process
  - Allowing customers to deploy their own 4G/LTE or 5G network.
  - In the US, AWS Private 5G uses CBRS (Citizens Broadband Radio Service), avoiding the need to procure spectrum licenses.
  - Integration with Spectrum Access System (SAS) for FCC regulation compliance are handled by AWS.
  - Delivers, provisions, and maintains all the preintegrated hardware, 5G Core and RAN software, and SIM cards.
  - Small cell radio units (RUs) are deployed on premises and the 5G Core (5GC) software runs either in an AWS Region or on premises using AWS managed infrastructure (AWS Outposts).
  - Setup Customized Bandwidths, Latency and Quality of Service (QoS) profiles for groups of devices and applications.
  - Pricing is based on Network Coverage and Capacity requirements, but not the number of connected devices.
  - Zero upfront charges for radio units, on-premises hardware, and SIMs. No installation charges or additional software licensing fees.
  - Provision a private cellular network of any size.
  - Expand coverage, and scale number of connected devices as needed.
  - AWS IAM integration to manage access between AWS services and devices in the network.
    - 5G-powered SIM cards are treated as IAM resources.
    - Manage SIM control policies.
  - Amazon CloudWatch Monitoring
    - Query metrics for network status, connected APs or SIMs, uplink and downlink usage by user, device, network categories.

## Reference
  - [AWS Private 5G](https://aws.amazon.com/private5g/)



  
