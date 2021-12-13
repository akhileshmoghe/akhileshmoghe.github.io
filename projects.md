---
layout: article
titles: "Projects"
sidebar:
  nav: personal-info
---

### *<u>Projects</u>*
#### *<u>Edge IoT Framework primarily for Life Sciences Use-cases</u>*:
* __*Role*__: Solutions Architect (3 Months)
* __*Skills*__: Service Oriented Architecture, __*Azure IoT Edge*__, __*AWS IoT Greengrass*__, __*Docker*__, __*Kubernetes*__
* __*Accomplishments*__:
  * Currently evaluating __*AWS Outpost*__, __*AWS EKS*__, __*Kubernetes*__, Containerized deployments, __*Apache Kafka*__, __*Pravega*__, Scalable __*MQTT brokers*__ deployment to address <u>high volume sensors</u>, <u>high-definition video</u> data use-cases in __*<u>5G</u>*__ Edge Computing scenarios.
  * Worked on common __*Edge IoT use-cases*__, various possible scenarios considering data flows, data types, data restrictions, privacy, latency, bandwidth consumptions, connectivity restrictions, etc., primarily revolving around Life Sciences projects and devices.
  * Evaluated Open-Source Edge Projects such as __*KubeEdge*__, __*ioFog*__, __*EdgeX*__, __*LF-Edge*__ Umbrella projects against identified use-cases.
  * Evaluated suitability of __*AWS IoT Greengrass*__ and __*Azure IoT Edge*__ + other AWS/Azure on-premises services for various Edge computing scenarios, presented pros & cons of both public cloud platforms and created various possible use-cases architecture/design with AWS/Azure as primary components of framework.
  * Architecting a common framework based on open-source Edge projects which can complement the public cloud services in on-premises Edge computing scenarios.

#### *<u>Jetson Nano based Healthcare IoT Device as a Guided Pipetting Tip Sensing System</u>*:
* __*Role*__: Systems Engineer (6 Months)
* __*Accomplishments*__:
  - Carried out PoC tasks like __*flashing boards*__ to simulate mass flashing at factory.
  - Multiple PoCs to understand customizing __*RootFS*__, __*Secure Boot*__, __*Bootloader Splash Screen*__.
  - Interfacing __*Bluetooth module*__ with *<u>NVIDIA L4T BSP</u>* software for *<u>Jetson Nano</u>*. All these PoC tasks resulted in a concrete plan to be executed at factory manufacturing.
  - Evaluated, Designed and implemented *<u>Firmware update</u>* and *<u>OS update</u>* mechanism based on __*Mender-Yocto*__ Open-Source project.
  - Designed and implemented device side __*C++*__ & __*Python*__, __*RESTful*__ HTTP protocol-based __*multiprocessor-distributed*__ IoT connectivity application for features like *<u>device identity</u>* & *<u>registration</u>*, *<u>status</u>*, *<u>user-device association<u>*, *<u>certificates management</u>*, *<u>device shadow</u>*.
  - Evaluated different __*inter-process communication*__ tools, __*RPC*__ mechanisms as __*ROS1*__, __*ROS2*__, __*D-Bus*__, __*ZMQ*__ to Architect & Design multiprocessor-distributed application.

#### *<u>STM32 MCU based Portable COVID-19 Diagnostic device kit</u>*:
* __*Role*__: Firmware Developer (4 Months)
* __*Accomplishments*__:
  - Designed and Implemented *<u>STM32F407</u>* based MCU __*firmware*__ to achieve __*USB communication*__ with Android app using __*Virtual COM Port*__, __*Flash memory read/write*__ and __*PWM generation*__.
  - Created a dummy *<u>test application</u>* in Python to automate the testing of STM32 firmware.
  - Received “__*Bravo Award*__” for the delivering the project in *<u>3 months</u>*.
  - [Client Received $2 Million funding to continue development based on our Project](https://nicoyalife.com/blog/nicoya-portable-covid-19-diagnostic-test-additional-fund/?utm_medium=organic_social&utm_source=Twitter&utm_campaign=atlas_additional_fund&utm_content=blog_promo).

    <blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Check out my iOS game, High5! <a href="https://t.co/QZEKLg3G2i">https://t.co/QZEKLg3G2i</a></p>&mdash; Keita Ito (@keitaitok) <a href="https://twitter.com/Nicoya_Life/status/1398026234039312390">August 26, 2014</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

#### *<u>OTA Firmware Updates for a STM32 MCUs and full OS Updates for x86 carrier boards</u>*:
* __*Role*__: IoT Developer (14 Months)
* __*Accomplishments*__:
  - Designed and implemented __*custom bootloader*__ with __*Dual bank*__ strategy for *<u>STM32F407 MCU</u>* to achieve robust *<u>Firmware update</u>* requirements. __*MAVLink*__ communication protocol and *<u>Signature & checksum verification</u>* were few of the other key features implemented.
  - Designed & developed __*C++*__ & __*Python*__ based __*multithreaded multiprocessor-distributed*__ application to achieve __*OTA firmware update*__ for multiple STM32 MCUs. __*AWS IoT Jobs*__, __*Device Shadow*__, __*Secure communication*__ and UART based *<u>serial communication</u>* were key features.
  - PoC for __*full OS image*__ and __*Application update*__ using __*Mender.io*__ Open-Sorce project. *<u>Full OS OTA updates</u>* with Mender server hosted on __*AWS EC2*__. Also, same kind of *<u>updates with USB and over LAN</u>* were also achieved with local Python server.
  - Evaluated and finalized __*Ayla IoT platform*__ for early market release without full fledge cloud development. *<u>Device provisioning</u>*, *<u>status</u>*, *<u>firmware updates</u>* to multiple devices, sensor *<u>data streaming</u>* were key features achieved in *<u>4 months duration</u>*.
  - Designed and implemented a __*NodeJS module*__ for *<u>Data Synchronization</u>* between device and cloud using __*MongoDB Realm*__ and __*MongoDB Atlas*__ cloud databases. Understanding of new platform and successful delivery was achieved in *<u>3 months</u>*.
  - Multiple product devices are deployed in the field [Product Demo](https://nicoyalife.com/products/spr-instruments/alto/).

    <iframe width="665" height="374" src="https://www.youtube.com/embed/RF7Anut-yQA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


#### *<u>Drone-based Asset Inspection with AWS IoT Greengrass & AWS Robomaker Services</u>*:
* __*Role*__: Robotics, IoT Developer (8 Months)
* __*Accomplishments*__:
  - Understanding of the new to be launched or recently launched __*AWS Services*__ like __*Robomaker*__, __*Greengrass*__, __*AWS IoT*__ and their use-cases for Robotics projects were achieved in *<u>3 months</u>* with demonstratable applications as an outcome.
  - __*Robotics framework ROS*__ based __*distributed application*__ had *<u>Machine Learning</u>* features like *<u>Rust and leakage detection models</u>* that were deployed with __*AWS IoT Greengrass*__ to multiple devices as __*NVIDIA Jetson TX2*__ (*<u>drone</u>*), *<u>mobile robots</u>* and *<u>x86</u>* machines.
  - __*AWS IoT device shadow*__ updates and __*IoT Jobs*__ for *<u>firmware update</u>* were also used.
  - __*AWS Lambda*__ functions were deployed to AWS IoT Greengrass to run *<u>ML inference</u>*.
  - Robotics application was able to *<u>capture and upload</u>* the __*thermal*__ and normal __*camera videos*__ to __*AWS S3*__ buckets, which were consumed by __*AWS Sagemaker*__ for Machine Learning training.
  - __*NodeJS*__, __*JavaScript*__ based Web Application running on __*AWS EC2*__ was developed to control Robots with *<u>commands</u>*, to trigger *<u>firmware updates</u>*, and to initiate *<u>ML training in Sagemaker</u>*.
  - Camera Live video streams were transmitted to __*AWS Kinesis Video Streams*__ and the same were rendered on Web Application in __*HLS format*__.
  - [Drone simulation around oil-rig running in background in AWS official Tech Talk](https://www.youtube.com/watch?v=LYWVF4bGHjs&t=558s&ab_channel=AWSOnlineTechTalks)
  - [Demos were successfully showcased at CERAWeek 2019 and AWS re:MARS 2019 events](https://twitter.com/QuetzalliAle/status/1137016252046594050?s=20)

    <blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Check out my iOS game, High5! <a href="https://t.co/QZEKLg3G2i">https://t.co/QZEKLg3G2i</a></p>&mdash; Keita Ito (@keitaitok) <a href="https://twitter.com/QuetzalliAle/status/1137016252046594050?s=20">August 26, 2014</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>


#### *<u>Robotics Device Management Effort Estimation & Proposal Creation</u>*:
* __*Role*__: Developer (2 Months)
* __*Responsibilities*__:
  - Understand the Product Requirements
  - Understand various __*Azure IoT*__ & other Cloud services and mapping them to Product Requirements.
  - Create an Overview __*ROS*__ & Azure Cloud based __*Architecture*__ using __*Docker Container*__.
  - Create __*Docker Containerized ROS Demo applications*__ running on x86 Linux machine *<u>accessing connected peripheral devices, file system</u>*.
  - Create __*complexity sheet*__ with various modular tasks to arrive at an __*effort estimation*__.
  - Coordinate with other Product teams involved.


#### *<u>Robotics ROS Framework Packages Migration</u>*:
* __*Role*__: Developer (6 Months)
* __*Accomplishments*__:
  - Understand the Open-Source Robotics frameworks __*ROS1*__ & __*ROS2*__.
  - Understand Robotics & Cloud Packages with respect to original architecture and implementations of assigned package.
  - Create package architecture with respect to new ROS2 framework, Migrate packages from ROS1 framework to ROS2 framework.
  - Follow up & Interact with Open-Source community, accommodate community suggestion.
  - Get the migration work Reviewed & Accepted by the Community members.
  - Successfully migrated __*vision-opencv*__ ROS package.


#### *<u>IBM Content Collector (part of IBM Enterprise Content Management)</u>*:
* __*Role*__: Module Lead Engineer (18 Months)
* __*Responsibilities*__:
  * Module Lead Engineer for IBM Content Collector for Email (primarily for IBM Notes/Domino).
  * Developed Domino & Notes Application for Content Collector Expiration Manager module using Lotus Scripts.
  * Debugged and resolved multiple server-side crashes, which improved stability of the product. resulting in retaining the customer to continue use IBM Domino/Notes applications.
  * Solved crucial Application Recovery issue with Email Connector module, that had seriously hampered overall product usability in case of unavoidable server-side crash.
  * Debugged and solved build issues with JNI exports in DLLs that caused intermittent application hang problem.
  * Thoroughly Analyzed the crash reports and provided sufficient evidence to prove the issues with 3rd party components/APIs.
  * Debugged ANT based build structure to resolve build issues.


#### *<u>WebMeeting (Screen Sharing Application) for MAC and Windows</u>*:
* __*Role*__: Backend Developer (26 Months)
* __*Accomplishments*__:
  - Received “__*<u>You Made a Difference Award</u>*__” for the extensive work done in the initial phase of the project, which helped the team to scale up and gain Client’s Confidence.
  - Understood the __*Chromium Open-Source Project*__ relevant modules, build systems which can be reused to create a __*cross-platform*__ (Windows & MAC) Screen Sharing application which used __*VP8 video codec*__ and __*WebM packetizer*__. Demonstrated the key functionality of screen sharing in *<u>3 months</u>*.
  - Evaluated and __*AWS S3*__, Dropbox, Google Drive for *<u>file sharing</u>* capabilities, but Client did not pursue another cloud platform and file share was implemented with Firebase & PubNub.
  - Designed and developed __*HTTP transport module*__ for *<u>screen sharing data</u>* + *<u>Chats</u>* + *<u>File sharing</u>* modules with __*Firebase*__ & __*PubNub*__ cloud platforms.
  - Later, it was developed into a full-fledged product with multiple browsers supports + Chat, file share, recording capabilities.
  - PoC application was developed for __*Image & Text Detection*__ in screen share data using an open-source library.
  - The core screen sharing product [WebMeeting](https://www.conferencecalling.com/screen-sharing) is still in production.
  
    <iframe width="560" height="315" src="https://www.youtube.com/embed/lV_iqJLWnUA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


#### *<u>Porting WebRTC based GChat Application on Linux & Android Platforms</u>*:
* __*Role*__: Backend Software Developer (20 Months)
* __*Accomplishments*__:
  - *<u>The product was successfully showcased in CES2012-13 by the customers</u>*.
__*Responsibilities*__:
  - Set Top Box, __*TI BeagleBone, Panda ARM board bring*__ up with *<u>Linux/Android OS images</u>*.
  - Building & Porting __*WebRTC*__ code for different __*Linux, Android Set top boxes*__.
  - Dealing with all kinds of *<u>compile</u>* and *<u>run-time errors</u>* on every platform.
  - Modifying WebRTC GYP build structure for integrating platform specific *<u>Codec and Camera Libraries</u>*.
  - *<u>Integrating</u>* __*H264 Decoder-Renderer*__ APIs with WebRTC code.
  - *<u>Integrating</u>*/Testing Quanta and Maxim *<u>Camera modules in the applications</u>*.
  - Debugged __*H264 Decoder*__, __*YUV-RGB Render*__ modules for Linux, Android platforms.
  - Thorough Unit Testing & Bug Fixing.


#### *<u>IL&FS Science Exploriments</u>*:
* __*Role*__: iOS Developer (5 Months)
* __*Responsibilities*__:
  - Developed Educational iPad Applications, to demonstrate & help students to understand the different Physics Laws. These Applications are simulations of the Physics Experiments on iPad.
  - Implemented the Core functionality with __*Model-View-Controller (MVC) Architecture*__.
  - Implemented different Physics formulas
  - Implemented Animation in the application with __*Cocoa Framework*__.
  - Thorough Unit testing & Bug Fixing
  - [Exploriments - Fluids - Archimedes Principle, Buoyancy and Flotation](https://www.148apps.com/app/483896866/)

    <iframe width="499" height="374" src="https://www.youtube.com/embed/0-Ml5odp-Uw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

  - [Exploriments – Pendulum - Effect of Length, Mass, Amplitude and Gravity on Oscillatory Motion of a Simple Pendulum](https://www.148apps.com/app/503129151/)

    <iframe width="499" height="374" src="https://www.youtube.com/embed/Nz1AEbu7XHU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
