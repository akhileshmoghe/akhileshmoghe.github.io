---
title:  "5G Radio Frequencies Overview"
date:   2022-02-28 12:33:22 +0530
permalink: /_posts/5g/5g_radio_frequencies
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
---

<style>
r { color: Red }
o { color: Orange }
g { color: Green }
</style>

## Why 5G RF Frequencies matter
- *<u>Radio Frequencies are a limited and expensive resource</u>*. It’s the most fundamental and basic building block of a wireless network.
- Frequency Bands Matter: Not all frequencies are made equal.
- New Frequency Bands will continue to open up to expand capacity and coverage. Different build-out and operating models work for different frequency bands. *<u>Devices and Network Frequency bands need to match for a 5G experience</u>*.
- The image below shows how and where radio waves are typically used vary with wavelength and frequencies.
- The __*RF Spectrum (3kHz - 300GHz)*__:

  ![The_RF_Spectrum_3kHz_300GHz](/assets/images/5g/The_RF_Spectrum_3kHz_300GHz.png){:.shadow}

- Below image provides an effective visual for the key differences of the three RF bands, where deployment changes with low, mid and high band frequencies.
  ![RF_Bands](/assets/images/5g/RF_Bands.jpeg){:.shadow}

- __*Low-band*__ are *<r>below 1GHz</r>*.
  - Their longer waves aren’t as affected by interference or blockage, so they will bring 5G farther and wider.
  - Though the bandwidth will be lower (*<r>30-75 Mbps</r>*).
  - This band can be a good option for *<u>remote Industrial IoT connectivity</u>*.
- __*Mid-band*__ (ranging from *<r>1-6 GHz</r>*) are mid-length waves.
  - With download speeds around *<r>115-223 Mbps</r>*.
  - they are useful for performance and *<u>wider 5G coverage in metropolitan areas</u>*.
- __*High-band (mmWave)*__ (*<r>24-100 GHz</r>*) are the ones that get the most attention with their *<u>shorter and dense waves</u>*, *<u>smaller coverage areas (small cells)</u>* and the bragging rights for 5G’s *<u>ultra-fast data transmission rates</u>* (*<r>peak rates of 20 Gb/s</r>*).
  
## 5G NR (New Radio) Frequencies
- __*<u>5G NR (New Radio)</u>*__ uses two key frequencies:
  - __*Frequency Range 1 (FR1)*__, including __*<u><r>sub-6 GHz</r></u>*__ frequency bands.
    - Low-band 5G and Mid-band 5G use frequencies from 600 MHz to 6 GHz.
    - The 3.5-4.2 GHz mid-band 5G frequencies are especially coveted for their ability to provide wider coverage.
  - __*Frequency Range 2 (FR2)*__, including frequency bands in the __*millimeter wave (mmWave) range (<r>24–100GHz</r>)*__.
    - [Millimeter wave (mmWave)](https://en.wikipedia.org/wiki/Millimeter_wave) bands (26, 28, 38, and 60 GHz) that *<u>offer the peak rates of <r>20 gigabits per second</r></u>*.

## Factors affecting 5G Deployment and High Bandwidth
- There are other factors than just the frequency bands that affect how 5G is deployed, especially for high band and high throughput deployments.
  ### *<u>Small Cells</u>*
  - Will be needed for high band 5G cells that are prone to obstacles and have denser and smaller coverage areas.
  - :heavy_check_mark: They can also be small enough to fit just about anywhere, so they are easier and faster to install than traditional and larger cell towers.
  - :x: But for that, it will also need amazingly dense and extensive coverage. The *<u>higher frequencies are prone to interference</u>* (which means everything kinda blocks it), but when there is nothing blocking it, it has a higher bandwidth and more precise connection. Since the range is short, you need a lot of these small cells. So, the buildout and coverage looks very different for different frequency bands. This also *impacts the connectivity and RF and capacity management*.
  - A comparison between 4G and smaller cell mmWave 5G coverage: 4G is narrow band, lower capacity, but wider coverage, whereas 5G is dense, crowded, but higher bandwidth and lower latency coverage.
  ![4g_vs_5g](/assets/images/5g/4g_vs_5g.png){:.shadow}
  
  - :x: This denseness has opened up discussions about the potential *<r>impact of pervasive 5G buildout on human health and the environment</r>*. The scale at which 5G will connect and enable devices and build antennas and edge studies need to continue to analyze impacts as the number of connected devices and RF deployment grow.
    - Further Reading: [5G Is Coming: How Worried Should We Be about the Health Risks?](https://blogs.scientificamerican.com/observations/5g-is-coming-how-worried-should-we-be-about-the-health-risks/)

  ### *<u>Massive MIMO</u>*
  - This gives the 5G cell sites its impressive speed and bandwidth. MIMO simply means __*<u>multiple input and multiple output</u>*__, and *<u>allows sending and receiving more data at the same time</u>*.
  - Today’s 4G base stations have about a dozen ports for antennas (8 that transmit and 4 that receive). 5G can support about 100 ports (that’s a factor of 22 or greater). That’s a lot more users and a lot more data.

  ### *<u>Beamforming</u>*
  - 100 or more ports means, lot many antennas, which also means possibility of much more interference. Beamforming technique has a solution for this.
  - Beamforming allows massive MIMO antennas to plot the best route to each user. It uses techniques to *<u><g>focus a wireless signal towards a specific device, rather than having the signal spread in all directions from a broadcast antenna</g></u>*.
  - Beamforming not only *<u>reduces the likelihood of interference with other antennas</u>*, but for High Band RF that is prone to blockage, it can also *<u>increase the likelihood that the signal will reach a device</u>*.
  ![BeamForming](/assets/images/5g/BeamForming.png){:.shadow}
  
  ### *<u>Full Duplex</u>*
  - This *<u>allows the same antennas to send and receive (duplex) over the same frequency band</u>*, thus doubling efficiency and *<r>raising complexity</r>*.

  ### *<u>Compatibility between Devices and Network Frequency Bands</u>*
  - Devices have their own receivers and they need to match to the 5G sites.
  - It will be important to check which frequency bands for 5G are supported on the 5G device. In order to access the 5G network, the personal devices, the IoTs and other systems have to not only be 5G-enabled, but must have the antennas built in to access the radio frequencies that the available 5G is transmitting on as well.

## 5G deployment diversity
- With different small cell placement strategies, MIMO applications and a variety of spectrum allocations, 5G NR installations could look very unique for different customers and carriers.
- Sites can be on poles, or buildings, or towers.
![5G RF Buildout Will Not Be Limited to Towers and Buildings](/assets/images/5g/5G_RF_Buildout_Will_Not_Be_Limited_to_Towers_and_Buildings.png){:.shadow}

- The experience and performance of coverage, capacity and latency will vary not only with traffic and congestion, but also with implementation. The image below shows a combination of the possible 4G/LTE and 5G network implementation.
- Open RAN and RAN Virtualization and cloud and compute capabilities to the Edge--add their own level of customization and complexity.
![Greater_Deployment_Diversity_with_5G](/assets/images/5g/Greater_Deployment_Diversity_with_5G.png){:.shadow}


