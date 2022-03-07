---
title:  "Pravega Sensor Collector: A Pravega Agent for IoT devices"
permalink: /_post/streaming_data/pravega_sensor_collector
date:   2021-12-15 1:33:22 +0530
categories:
  - Pravega
  - Streming Data
  - IoT
  - Edge IoT
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Pravega
  - Streming Data
  - IoT
  - Edge IoT
author: Akhilesh Moghe
show_author_profile: true
---

## Hardware/Software requirements
  - It's an Open-source project hosted on [GitHub: pravega/pravega-sensor-collector](https://github.com/pravega/pravega-sensor-collector).
  - It's a __*Micro-edge*__ software with pre-requisites as: __*x86 machine*__ with __*4GB RAM*__ and is supported on __*Linux*__ OS.
  - It's a *<u>Java Application</u>*, with <u>plug-in architecture</u>.
  - It supports:
    - __*I2C Communication*__ and is officially tested for *<u>ST Micro lng2dm 3-axis Femto accelerometer</u>*.
    - __*Linux network interface card (NIC)*__ statistics like *<u>byte counters</u>*, *<u>packet counters</u>*, *<u>error counters</u>*, etc.
    - Phase IV Leap __*Wireless Gateway*__
    - Generic __*CSV file import*__: if there's no other way.

## Data Collection
- Continuously reads data from directly connected sensors.
- Writes the data to a Pravega stream at the edge.
- Pravega Sensor Collector can support *<u>Data ingestion Speed</u>* of around *<u>1000 samples per seconds (1 kHz) per sensor</u>*, and may be more.
- Data is transferred in *<u>Batches</u>* to Pravega cluster. It batches multiple samples into larger events that are periodically sent to Pravega cluster.
  - The *<u>batch size is configurable</u>* to allow you to tune the trade-off between latency & throughput.

## Offline mode
- It supports *<u>Intermittent connectivity loss</u>* to Edge Pravega Clusters.
- It can continuously collect high-resolution samples without interruption.
- It periodically attempts to reconnect to the Pravega server.
- *<u>Storage</u>*
  - It stores collected sensor data on a local disk in *<u>SQLite database</u>*.
- *<u>Sync</u>*
  - Pravega Sensor COllector takes care of data synchronization with Pravega Clusters.
  - It *<u>coordinates SQLite transactions with Pravega Transactions</u>* with *<u>Two-Step Commit protocol</u>*.
  - Samples bufferred are sent in-order without gaps or without duplicates.

## Predictive Maintenance for IoT Example Solution Architecture
![Predictive Maintenance for IoT Example Solution Architecture](/assets/images/streaming_data/pravega/Predictive-Maintenance-Solution-Architecture.jpg){:.shadow}
- [Complete Blog: Predictive Maintenance for IoT](https://cncf.pravega.io/predictive-maintenance-for-iot/)

###### References:
- [GitHub: pravega/pravega-sensor-collector](https://github.com/pravega/pravega-sensor-collector)
- [Predictive Maintenance for IoT](https://cncf.pravega.io/predictive-maintenance-for-iot/)


