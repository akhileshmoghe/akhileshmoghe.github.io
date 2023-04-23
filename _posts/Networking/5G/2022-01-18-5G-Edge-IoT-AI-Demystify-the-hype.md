---
title:  "5G Introduction: Features, Terminologies an Overview"
date:   2022-01-18 12:33:22 +0530
permalink: /_posts/5g/5g_intro_features_terms_overview
categories:
  - 5G
  - Edge Computing
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - 5G
  - Edge Computing
author: Akhilesh Moghe
show_author_profile: true
sidebar:
 nav: IoT
---

<style>
r { color: Red }
o { color: Orange }
g { color: Green }
</style>

## Wireless Access Technologies Evolution
  ![Wireless Access Evolution](/assets/images/5g/Wireless_Access_Evolution.png)

  ---

## 5G at a Glance:
  ![5G at a Glance](/assets/images/5g/5g-at-a-glance.png)

  ---

  ![Opportunities provided by 5G](/assets/images/5g/5G_Opportunities_.png)

  ---

## Promises of 5G
- 5G will continue to operate with other generations (4G/LTE/3G) of wireless systems for a good forseable future. And the effectiveness of 5G will be measured not only by *<u>the areas they cover</u>*, but how *<u>seamlessly the applications and content work with existing technologies</u>*, including WiFi and 4G.
- 5G __*speed*__ and __*low latency*__ that can enable machine-to-machine communications at speeds and volumes that are incomprehensive.
  - __*Peak data rates*__ are set at __*20Gbps downlink*__ and __*10Gbps uplink*__.
  - On *<u>average</u>*, speeds will be closer to *<u>download at 100Mbps</u>* and *<u>upload at 50Mbps</u>*. Note that, there is also less interference and a more efficient mechanism in 5G for uplink and downlink communication.
  - The higher uplink bandwidth allows for IoT devices to upload more and faster data.
  - *<u>4G Latency is 50 milliseconds</u>*; *<u>5G Latency is expected to be 4 milliseconds or 1ms</u>* for more precise use cases. *It depends on the implementation*.
  - It is the improvements in latency that hold much of the promise for Internet of Things (IoT) and Artificial Intelligence (AI) apps.
- Radio interfaces should be __*energy-efficient*__ when in use. They thus *<u>drop into a low-energy mode when not in use</u>*.
  - With *<u>5G</u>*, the time it takes to switch into a low-energy state should be within *<u>10 milliseconds</u>*.
  - Note: The *overall energy consumption may still be high because of the additional antennas and small cells*.
- 5G should have an increased __*spectral efficiency*__ over LTE, with *<u>30bits/Hz downlink</u>*, and *<u>15 bits/Hz uplink</u>*.
  - __*What is Spectral Efficiency?*__
    - Spectral efficiency, spectrum efficiency or bandwidth efficiency refers to the information rate that can be transmitted over a given bandwidth in a specific communication system.
      <iframe width="665" height="374" src="https://www.youtube.com/embed/qVXOYgczcHw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    - Reference: [Spectral Efficiency : 5G-NR and 4G-LTE](https://www.techplayon.com/spectral-efficiency-5g-nr-and-4g-lte/)
- Base stations with 5G should support __*movement/mobility*__ from *<u>0 to 310 mph (miles per hour)</u>*.
- __*Software-defined infrastructure*__, *<u>Open Radio Access Network (RAN)</u>*, and the ability to bring IT and cloud features and the ability to dynamically allocate resources and bring computing to the edge of the network.
- <r> 5G will raise questions/concerns about *<u>surveillance</u>* and *<u>privacy</u>*. And It will introduce new *<u>security vulnerabilities</u>* and *<u>inefficiencies</u>*, but this is the conundrum and tradeoff between being open versus being safe. </r>
- As 5G networks grow, and more and more *<u>edge devices</u>* or *<u>edge nodes</u>* are created to *<u>distribute computing, storage and decision making</u>*, they will enable the smartness in __*smart grid*__, __*smart cars*__, __*smart cities*__, __*smart factories*__, __*smart ports*__, and more. Smart in this context primarily refers to *<u>something that is connected</u>* and can be *<u>remotely managed</u>*, and has *<u>some level of automated decision making</u>*.

- This __*<u>performance</u>*__*<u> though is dependent on</u>*:
  - __*<r>how the network is designed?</r>*__
  - __*<r>what frequency band you are on?</r>*__
  - __*<r>what the transport network can support?</r>*__
  - __*<r>how many other users are congesting the network?</r>*__.
  - This [Lifewire article](https://www.lifewire.com/5g-speed-4180992) compared how long it would take to download a *<u>3GB movie</u>* with average speeds (all the assumptions are listed there). Whereas __*3G*__ networks would take *<u>1 hour and 8 minutes</u>*, __*4G/LTE*__ would take *<u>27 minutes</u>*, __*Gigabit LTE*__ brought it to *<u>61 seconds</u>* and __*5G*__ to *<u>35 seconds</u>*. If you had dedicated connectivity and 5G was operating at its 20 Gb/s peak rate, then the download will take just over 1 second.

## Network Features of 4G, 5G, and the Future 6G

|---
|Feature | 4G | 5G | 6G |
|:-|:-|:-|:-|
|Usage Scenarios | MBB (Mobile Broadband) | :small_orange_diamond: __*eMBB*__ (Enhanced Mobile Broadband) <br/> :small_orange_diamond: __*URLLC*__ (Ultra Reliable and Low Latency Communications) <br/> :small_orange_diamond: __*mMTC*__ (Massive Machine Type Communications) | :small_orange_diamond: __*FeMBB*__ (Further-Enhanced Mobile Broadband) <br/> :small_orange_diamond: __*ERLLC*__ (Extremely Reliable and Low-Latency Communications) <br/> :small_orange_diamond: __*umMTC*__ (Ultra Massive Machine Type Communications) <br/> :small_orange_diamond: __*LDHMC*__ (Long Distance and High Mobility Communications) <br/> :small_orange_diamond: __*ELPC*__ (Extremely Low Power Communications) |
| Applications | :small_orange_diamond: High Definition Videos <br/> :small_orange_diamond: Voice <br/> :small_orange_diamond: Mobile TV <br/> :small_orange_diamond: Mobile Internet <br/> :small_orange_diamond: Mobile Pay | :small_orange_diamond: VR (Virtual Reality) <br/> :small_orange_diamond: AR (Augmented Reality) <br/> :small_orange_diamond: 360° videos <br/> :small_orange_diamond: UHD (Ultra High Definition) Videos <br/> :small_orange_diamond: V2X (Vehicle-to-everything) <br/> :small_orange_diamond: IoT (Internet of Things) <br/> :small_orange_diamond: Smart City/Factory/Home <br/> :small_orange_diamond: Telemedicine <br/> :small_orange_diamond: Wearable Devices | :small_orange_diamond: Holographic Verticals and Society <br/> :small_orange_diamond: Tactile/Haptic Internet <br/> :small_orange_diamond: Full Sensory Digital Sensing and Reality <br/> :small_orange_diamond: Fully Automated Driving <br/> :small_orange_diamond: Industrial Internet <br/> :small_orange_diamond: Space travel <br/> :small_orange_diamond: Deep Sea Sightseeing <br/> :small_orange_diamond: Internet of Bio-Nano-Things |
| Network Characteristics | :small_orange_diamond: Flat and All-IP | :small_orange_diamond: Cloudization <br/> :small_orange_diamond: Softwarization <br/> :small_orange_diamond: Virtualization <br/> :small_orange_diamond: Slicing | :small_orange_diamond: Intelligentization <br/> :small_orange_diamond: Cloudization <br/> :small_orange_diamond: Softwarization <br/> :small_orange_diamond: Virtualization <br/> :small_orange_diamond: Slicing |
| Service Objects | :small_orange_diamond: People | :small_orange_diamond: Connection (People and Things)	| :small_orange_diamond: Interaction (People and World) |
| <r>KPI: Peak Data Rate</r>	| :small_orange_diamond: <r>100 Mb/s</r>	| :small_orange_diamond: <r>20 Gb/s</r> | :small_orange_diamond: <r>Over 1 TB=b/s</r> |
| <r>KPI: Experienced Data Rate</r> | :small_orange_diamond: <r>10 Mb/s</r> | :small_orange_diamond: <r>0.1 Gb/s</r> | :small_orange_diamond: <r>1 Gb/s</r> |
| KPI: Spectrum Efficiency | :small_orange_diamond: 1x | :small_orange_diamond: 3x that of 4G | :small_orange_diamond: 5-10x that of 5G |
| KPI: Network Energy Efficiency | :small_orange_diamond: 1x | :small_orange_diamond: 10-100x that of 4G | :small_orange_diamond: 10-100x that of 5G |
| <r>KPI: Area Traffic Capacity</r> | :small_orange_diamond: <r>0.1 Mb/s/m²</r> | :small_orange_diamond: <r>10 Mb/s/m²</r> | :small_orange_diamond: <r>1 Gb/s/m²</r> |
| <r>KPI: Connectivity Density</r> | :small_orange_diamond: <r>10⁵ Devices/km²</r> | :small_orange_diamond: <r>10⁶ Devices/km²</r> | :small_orange_diamond: <r>10⁷ Devices/km²</r> |
| <r>KPI: Latency</r> | :small_orange_diamond: <r>10 ms</r> | :small_orange_diamond: <r>1 ms</r> | :small_orange_diamond: <r>10-100 µs</r> |
| <r>KPI: Mobility</r> | :small_orange_diamond: <r>350 km/h</r> | :small_orange_diamond: <r>500 km/h</r> | :small_orange_diamond: <r>Over 1,000 km/h</r> |
| Technologies | :small_orange_diamond: OFDM (Orthogonal Frequency-Division Multiplexing) <br/> :small_orange_diamond: MIMO (Multiple Input, Multiple Output) <br/> :small_orange_diamond: Turbo Code <br/> :small_orange_diamond: Carrier Aggregation <br/> :small_orange_diamond: Hetnet (Heterogeneous Networks) <br/> :small_orange_diamond: ICIC (Inter-Cell Interference Coordination) <br/> :small_orange_diamond: D2D (Device-to-Device) Communications <br/> :small_orange_diamond: Unlicensed Spectrum | :small_orange_diamond: mm-wave Communications <br/> :small_orange_diamond: Massive MIMO <br/> :small_orange_diamond: LDPC (Low Density Parity Check) and Polar Codes <br/> :small_orange_diamond: Flexible Frame Structure <br/> :small_orange_diamond: Ultradense Networks <br/> :small_orange_diamond: NOMA (Non-Orthogonal Multiple Access) <br/> :small_orange_diamond: Cloud/Fog/Edge Computing <br/> :small_orange_diamond: SDN/NFV/Network Slicing | :small_orange_diamond: THz (Terahertz) Communications <br/> :small_orange_diamond: SM-MIMO (Spatial Modulation Multiple Input, Multiple Output) <br/> :small_orange_diamond: LIS (Large Intelligent Surfaces) and HBF (Holographic Beamforming) <br/> :small_orange_diamond: OAM (Orbital Angular Momentum) Multiplexing <br/> :small_orange_diamond: Laser and VLC (Visible Light Communication) <br/> :small_orange_diamond: Blockchain-based Spectrum Sharing <br/> :small_orange_diamond: Quantum Communications and Computing <br/> :small_orange_diamond: AI/Machine Learning |
|---

- In above table, as we navigate from 4G to 6G, you will notice the following trends:
  - __*Expanding coverage*__ for frequency bands *<u>for denser and private networks</u>*.
  - __*Higher speeds*__: The trend is to establish a high bar for future speed, and then figure out how to make more efficient use of radio frequencies, add more bands, better antennas, more controls, etc.
  - __*Greater scale and precision*__: We are moving to increase in scale and capabilities for Massive IoT and critical communication to meet needs of specific verticals like automotive, transport, industry and health. The features in the standards show up as __*mMTC*__- massive Machine Type Communication and __*URLLC*__- Ultra-Reliable Low-Latency Communication.
  - As we get to 6G, applications include considerations for bio and nano tech sensors, space and under-sea explorations and full-sensory digital reality.
  - Deeper integration of AI/ML at different network layers and continuing exploitation of open architecture.

## Open & Flexible 5G Architecture
- Components & Layers of 5G Network from Edge to Cloud:
![Components & Layers of 5G Network from Edge to Cloud](/assets/images/5g/Potential_for_Machine_Learning_to_Enhance_E2E_5G_Networks__Edge_to_Cloud_.png)
- The __*5G core*__, as defined by 3GPP, utilizes cloud-aligned, __*service-based architecture (SBA)*__ that spans across all 5G functions and interactions, including *<u>authentication</u>*, *<u>security</u>*, *<u>session management</u>* and *<u>aggregation of traffic</u>* from end devices.
- There are basically *<u>four different layers</u>* identified as __*network layer*__, __*controller layer*__, __*management and orchestration layer*__, and __*service layer*__.
- 3GPP standards divide the 5G network architecture into the following components:
  - *<u>User Equipments</u>* (Terminals or Devices)
    - devices, or IoT sensors, or robots
  - *<u>Radio Access Networks</u>*
    - Connects the user equipment and devices to the rest of the 5G network through radio connections.
    - It includes everything that enables that *<u>dynamic radio connection</u>* to be maintained, handed off and reached, and utilizes *<u>radio transceivers to connect you to the cloud</u>*.
    - RAN is made up of *<u>functional components</u>* such as a __*Radio Unit (RU)*__, __*Distributed Unit (DU)*__, and a __*Centralized Unit (CU)*__.
  - *<u>Access or Transport Network</u>* (that connect the different RAN and Core components)
    - can be __*microwave*__, __*fixed wireless*__ or __*wireline*__.
    - It basically *<u>connects the core network to the RAN</u>*.
    - The __*long-haul*__ (also referred to as __*backhaul*__) or
    - __*short hops*__ (often referred to as __*front hauls*__ for smaller cells or the last mile).
      - Further Reading: [Mobile Backhaul: An Overview](/assets/docs/5g/GSMA _ Mobile Backhaul_ An Overview - Future Networks.pdf)
    - How this layer is designed (how many hops you take, etc.) will affect a service’s ability to connect.
    - Bandwidth constraints and latency will impact the overall performance equation and over dimensioning will impact cost.
  - *<u>Core Network</u>*
    - where the intelligence and data is managed.
    - With 5G, edge nodes can extend the core network functionality all the way to the edge.
    - The core network provides all the details about the *<u>users</u>* and *<u>applications to orchestrate and manage the communication</u>*.
    - 5G Core aggregates data traffic from end devices and RAN that is connected using the transport or backhaul network.
    - Think of the 5G Core less as a physical network and more as a *<u>virtualized set of software-based network functions (or services)</u>* that can show up in a *<u>centralized location</u>* or within *<u>Multi-access Edge Computing (MEC)</u>* cloud infrastructures.

## Technologies defining Open Architecture of 5G:
- __*<u>Software-defined networks (SDN)</u>*__:
  - SDN are not limited to 5G. At a high level, SDN *<u>enables operators to dynamically allocate bandwidth and update software and features that bring operational and resource efficiency</u>*. It allows for dynamic and software-based performance and network monitoring. This will also allow servers and hardware resources to be allocated on-demand and with greater speed and flexibility.
- __*<u>Network Virtualization Function (NVF)</u>*__:
  - NVF allows network operators to apply IT and cloud virtualization technology and is an important component of allowing more general purpose servers to be utilized in wireless networks.
- [*<u>Open RAN (Radio Access Network)</u>*](https://www.5gamericas.org/understanding-open-ran/):
  - Open RAN concept is not limited to 5G. Basically, Open RAN will bring interoperability of open hardware, software and interfaces and cellular wireless networks by leveraging a programmable open software development approach. Open RAN decouples or disaggregates hardware and software used in radio base stations.
  - In addition to the 3GPP RAN interfaces, the [O-RAN Alliance](https://www.o-ran.org/) is creating specifications for open, interoperable 5G RAN architecture and standards, compatible with 3GPP standards.
  - The O-RAN Software Community ([O-RAN-SC](https://wiki.o-ran-sc.org/)) is the open source software for RAN.
  - Other industry initiatives:
    - TIP (Telecom Infra Project) Forum’s __*OpenRAN 5G NR Project*__ Group
    - __*RAN Intelligence & Automation (RIA)*__ focused on AI/ML applications
    - Open Network Foundation’s SD RAN and Open RAN Policy Coalition.
    - __*NEC Open RAN laboratory*__ in *<u>India</u>*
    - [Intel and Google announced a partnership to accelerate virtualized RAN and Open RAN solutions](https://newsroom.intel.com/news/intel-google-cloud-aim-advance-5g-networks-edge-innovations/#gs.wuuwm8).
  ![RAN_transformation](/assets/images/5g/RAN_transformation.jpeg){:.shadow}
- __*<u>Network slicing</u>*__:
  - Will allow operators to *<u>slice off portions of their networks for specific uses, or even specific enterprise clients, to meet security, bandwidth, or usage requirements</u>*.
  - Network slicing essentially creates multiple mini-networks operating in parallel for different workloads or bandwidth requirements.
  - Slicing could potentially allow dynamic reallocation of resources to organize communication of data and information into layers, containers, and modules that can sit on different hardware and be remotely managed.
  ![Slicing a Wireless Network from RAN, Transport to Core](/assets/images/5g/Network_Slicing.png){:.shadow}

  ---

## Concerns around 5G
- The 5G World is emerging around us. Most research predicts that 5G networks will carry about a quarter of all global mobile traffic by 2024. This means that not only do we need to understand the opportunities and requirements to navigate this new 5G world, but also be aware of the responsibilities and risks that comes with it.
- __*<u>Health & 5G</u>*__:
  - As of January 2021, over [255 scientists in 44 countries have signed on an appeal](https://ehtrust.org/key-issues/cell-phoneswireless/emf-scientist-appeal-advisors-call-moratorium-5g/) to the United Nations urging action to reduce EMF (electric and magnetic fields) exposure emitting from wireless sources.
  - A 2019 New York Times article backed most of industry and FCC claims that [cancer concerns related to 5G are not based on science](https://www.nytimes.com/2019/07/16/science/5g-cellphones-wireless-cancer.html) because the signals don’t penetrate human skin at high levels. But what about skin cancer?
  - A paper from the National Institute of Health’s Journal of Biomedical and Physics Engineering raised the question: [5G Technology: Why Should We Expect a shift from RF-Induced Brain Cancers to Skin Cancers?](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6820018/)
  - When health risks are involved, it is imperative to proceed with caution. It would serve the collective wireless and technology industry to understand the impacts so that personal devices along with small cells and networks could be designed and built to minimize potential harm with prolonged usage, increased proximity and dense networks.
- __*<u>Environment & 5G</u>*__:
  - Only Time will tell whether the claim of using optimized networks to reduce environmental impact will hold true.
  - :heavy_check_mark: Though 5G claims to be 50-90% energy-efficient compared to 4G,
  - :x: The density of the networks and the volume of devices and connections is estimating a 160-170% increase in energy consumption (by 2026-2030) just by telecom networks.
  - And this is not even a comprehensive view of the increased energy consumption by all the related technologies and industries.
- __*<u>Privacy & Security</u>*__:
  - When you are connected everywhere and always on-- data and location privacy and security become real concerns and issues.
  - This concern is not only for the public and consumers, but also businesses and enterprises. People’s rights and concerns are important aspects of our social systems that use the technologies and products we build. Security and privacy are critical for businesses to serve their customers' interests.
  - These hyper-connected networks can make it harder and harder to safeguard privacy and security and to effectively manage surveillance.
  - The European Union’s GDPR (General Data Protection Regulation) has changed the conversation and landscape of privacy online.


