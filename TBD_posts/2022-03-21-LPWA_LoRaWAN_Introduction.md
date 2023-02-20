---
title: "LPWA and LoRaWAN Introduction"
date: 2022-03-21 12:33:22 +0530
permalink: /_posts/networking/lpwa_lorawan_intro
categories:
  - IoT
  - Edge IoT
  - LPWA
  - LoRaWAN
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - IoT
  - Edge IoT
  - LPWA
  - LoRaWAN
author: Akhilesh Moghe
show_author_profile: true
sharing: true
---

<style>
r { color: Red }
o { color: Orange }
g { color: Green }
</style>

- __*Low-Power, Wide-Area (LPWA)*__ is a generic term for a group of technologies with the following key characteristics:
  - *<u>Long battery life</u>* (often in excess of 10 years whilst supporting a benchmark smart metering application)
  - *<u>Wide area connectivity</u>* characteristics, allowing for out-of-the-box connected solutions
  - *<u>Low cost</u>* chipsets and networks
  - *<u>Limited data communications throughput</u>* capacity
- LPWA technologies complement existing cellular mobile network and short range technologies, enabling wide area communications at lower cost points and better power consumption characteristics.
- The most fundamental difference between __*LPWA (Low-Power, Wide-Area)*__ technologies includes the *<u>radio spectrum</u>* that the technologies use *<u>licensed</u>* vs *<u>license exempt</u>* spectrum.
- The __*LoRa® Alliance*__ defines a global standard for LPWA networks and the __*LoRa® protocol (LoRaWAN™)*__ being deployed around the world to enable IoT, machine-to-machine (M2M), and industrial or consumer applications.

## Key applications for LPWA connectivity include:
- Connecting power-consuming (or storing) assets to a *<u>umanaged electricity grid</u>* to enable the increased use of renewables.
- *<u>Consumer devices</u>* that are connected in order to enhance the overall value proposition, particularly in the areas of *<u>home automation and assisted living</u>*.
- A range of *<u>smart city applications</u>* to increase the day-to-day efficiency of cities and smooth the way to a highly urbanized future.
- Diverse *<u>agricultural monitoring and control applications</u>* to allow for more effective and efficient use of agricultural land and resources.
- *<u>Intelligent building applications</u>* to increase building efficiency.
- *<u>Supply chain applications</u>* to increase operational efficiencies and allow for new business models and optimal customer satisfaction.

## LPWA for Systems Integrators
- LPWA technologies provide an opportunity for __*Systems Integrators (SIs)*__ to extend reach in terms of the diversity of IoT solutions that can be supported.
- Deploying LPWA networks can also allow SIs to have more control over solution performance for specific clients, enabling the SI to, for example, optimize network coverage or quality of service for a specific client implementation.
- The ability to offer LPWA services is likely to be particularly advantageous to SIs in scenarios that are characterized by ‘campus’ style deployments, including smart cities, industrial processing and manufacturing locations, farms and agricultural environments, and retail and other shared service environments such as docksides, airports and educational facilities.
- __*License-exempt*__ LPWA solutions are likely to be most suitable for Systems Integrators, unless radio spectrum is available (in case MNO itself is SI).

## Alternative to LoRa technology in LPWA space:
- The most fundamental difference between __*LPWA (Low-Power, Wide-Area)*__ technology types includes the __*<u>radio spectrum</u>*__ that the technologies use __*<u>licensed</u>*__ vs __*<u>license exempt</u>*__ spectrum.
- Another key difference between LPWA technologies is the __*commercial strategies*__ of the companies that seek to deploy them.
  - Certain types of connected devices are associated with nationwide (or even multinational) supply chains, and accordingly, only LPWA providers that can offer national (or multinational) solutions are well positioned to provide connectivity for these devices.
  - Conversely, some applications (often supporting industrial or smart city solutions) only require connectivity within a limited geographic area. There is more flexibility for the provision of connectivity in this latter case since such solutions can rely on nationwide networks providing connectivity as a service, but also custom-deployed private networks could be suitable.
- LPWA technologies that are deployed in __*(license exempt)*__ __*ISM (Industrial, Scientific, Medical) spectrum*__ will find an easier path to market and <g>benefit from </g>*"<u><g>free spectrum resource</g></u>"*, but are <r>more limited in terms of </r>*"<u><r>forward path capabilities</r></u>"*<r> and support for application security</r>.
  - __*<u>Licensed Solutions</u>*__:
    - Pros:
      - Better forward path
      - QoS transparency,
      - Authentication & Security
      - Potentiial to re-use existing cellular sites
      - Fast time to blanket coverage (post standards)
    - Cons:
      - Relies on mobile operators
      - Service costs very much TBC
      - Spectrum cost
  - __*<u>License Exempt Solutions</u>*__:
    - Pros:
      - Fast time to market
      - Option for new operators, specialist service providers
      - Better BYO option
      - Ecosystems developing
      - ‘No Surprises’ re: cost
    - Cons:
      - Limited (remote) security
      - Few multinational players


###### References:
- [LPWA Technologies Unlock New IoT Market Potential](https://lora-developers.semtech.com/uploads/static/LPWAN_technologies.pdf)



