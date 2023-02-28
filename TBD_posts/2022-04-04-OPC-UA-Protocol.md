---
title:  "OPC-UA Protocol"
date:   2022-04-04 12:33:22 +0530
#permalink: /_posts/iot/protocols/opc-ua
categories:
  - IoT
  - Edge IoT
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - IoT
  - Edge IoT
author: Akhilesh Moghe
show_author_profile: true
---

- Open Platform Communications - Unified Architecture (OPC-UA)
- OPC-UA is one of the most important communication protocols for __*Industry 4.0*__ and the __*IIoT*__.

## OPC Classic vs. OPC-UA
- The current standard of the OPC specification is OPC UA (OPC Unified Architecture). It is the successor of the old OPC standard, which is called __*OPC Classic*__.
- The old standard already very successfully solved the task of realizing data exchange in automation independent of the manufacturer and defined the basic interfaces.
- :x: The *<u>disadvantage</u>* of OPC Classic was the *<u>lack of platform independence</u>*. OPC Classic is *<u>based on the Microsoft technologies</u>* COM and DCOM and therefore *<u>OPC Server and OPC Client installations were limited to Microsoft Windows</u>* operating systems and networks. With the increasing success of other platforms (Linux, Web architectures, Cloud, IoT Devices, CPS, …) the distribution of OPC was limited.
- :heavy_check_mark: OPC UA has *<u>platform independence</u>* and *<u>interoperability</u>* as its primary goal. Technically, the standard was built on the basis of basic web technologies (TCP/IP, http/SOAP). The basic concepts for data exchange were adopted, combined and supplemented by further concepts.

  ### Platform independence and interoperability
  - OPC UA has become completely platform-independent through TCP/IP and web protocols for communication. An OPC server, which makes its data available in the network, can be addressed via these protocols.
  - It is irrelevant whether the OPC Server runs on a Windows operating system, a Linux, UNIX or Mac. Even completely own platforms with TCP/IP stack can implement an OPC server. So typical embedded systems, devices and controls can serve as servers.
  - The goal of platform independence has been achieved and leads to a fast distribution of OPC UA. Interoperability is the logical consequence of the infrastructure with various OPC UA participants in the network.

## OPC Server
- The OPC Server is the basis of OPC communication.
- It is a software that implements the OPC standard and thus provides the standardized OPC interfaces to the outside world.
- OPC Servers are provided by different parties.

  ### OPC Server from hardware vendor
  - The basic idea of OPC is that the manufacturer of the hardware provides an OPC Server for his system and therefore allows standardized access.
  - The OPC Server from the manufacturer can be supplied as stand-alone software or as an embedded OPC Server on the device or the machine controller.

  ### Independent OPC Servers
  - In addition to OPC Servers from the manufacturers themselves, there are providers who develop independent OPC Servers.
  - These servers differ by a broader support of communication protocols. This means that OPC communication can also be provided for systems for which the manufacturer does not provide its own OPC Server.
  - Manufacturers of independent OPC Servers include [Kepware](https://www.kepware.com/en-us/products/kepserverex/) (over 150 drivers available), Softing, INAT and Matrikon.
  - The OPC Router can also use the [OPC UA Server Plug-in](https://www.opc-router.com/opc-ua-server-opc-router/) to provide data as an OPC server.

## OPC Client
- The OPC Client is the logical counterpart to the OPC Server.
- The OPC Server can be connected to the OPC Client and read out the data provided by the Server.
- Since the OPC Servers implement the predefined interfaces of the OPC standard, each client can access any OPC Server and exchange data with the server in the same way.

  ### OPC Test Client
  - There are many free OPC test clients on the market, with which the function and configuration of a server can be tested very easily and clearly.
  - The existing data points can be searched, connected and the current values can be viewed in a very short time.
  - In operational use it is very important to test the function of the data source “OPC” via an independent application and thus abstract it from higher-level applications.
  - A popular test client is the [UaExpert—A Full-Featured OPC UA Client from Unified Automation](https://www.unified-automation.com/products/development-tools/uaexpert.html).

## Applications acting as OPC Clients
- Typical OPC clients are applications that depend on data exchange with industrial systems. What happens to the data in the client is very application-specific.
- Common applications are __*visualization*__ and __*SCADA systems*__ (WinCC, InTouch, FAS inMOVE) or __*MES systems*__.
- The OPC Router with its OPC UA Client Plug-in is also a client with a gateway function.

## OPC UA Specifications
- The standard OPC UA consists of individual specifications.
- Each specification describes a partial function and specifies which server and client interfaces must be implemented for this function in order to support it.
- *<u>OPC servers and clients do not have to support all specifications</u>*.
- Depending on the application, often only individual specifications are programmed. When using an OPC server and clients, it is therefore important to consider which specifications are required and which are implemented by the server and client.
- OPC UA consists of these specifications:
  ### [Concepts](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-1-overview-and-concepts/)
  - Introduction of each of the OPC UA Specifications
  - Overview of the design, security, and scalability goals
  - Client/Server methodology used by OPC - Systems concepts of OPC, such as address-space, subscriptions, and events
  - Introduction of the concepts behind the Services (interfaces)
  
  ### [Security Model](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-2-security-model/)
  - Introduction of the security objectives and identifying threats to OPC-based systems
  - Mitigation of identified threats
  - Overview of *<u>user-authentication</u>* and *<u>controlling access rights</u>*, *<u>application identification</u>* using *<u>digital certificates</u>*, *<u>auditing user, system activities</u>*, availability of OPC systems (redundancy) and *<u>secure/encrypted message transmission</u>*.

  ### [Address Space Model](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-3-address-space-model/)
  - The concepts of an *<u>address space</u>*, *<u>Node</u>* and *<u>Views</u>*
  - Detailed description of Nodes and References and how they are used to logically organize the address space
  - Detailed descriptions of *<u>node types</u>*, *<u>reference types</u>*, *<u>data-types</u>*, *<u>event types</u>*, and how they can be used for information modelling.

  ### [Services](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-4-services/)
  - The UA *<u>Services (interfaces)</u>* that Servers and Clients must use
  - Server and Client *<u>behavior expectations</u>*
  - Common *<u>data-types</u>* used by Service parameters.

  ### [Information Model](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-5-information-model/)
  - Detailed descriptions and rules for Nodes and References
  - Detailed descriptions and rules for *<u>standard object types</u>*, *<u>variable types</u>*, *<u>methods</u>*, *<u>event types</u>*, and *<u>standard data-types</u>*.
  - Information modelling rules, including sub-typing
  - *<u>State-Machines</u>* and *<u>File transfer</u>* descriptions
  
  ### [Mappings](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-6-mappings/)
  - This specification describes how *<u>data and information are transferred</u>* between OPC UA Servers and Clients, covering:
  - *<u>Data encoding/decoding</u>* overview and rules for *<u>standard data-types</u>* and for *<u>complex data-types</u>* and *<u>objects</u>*.
  - *<u>Securing OPC UA messages</u>* for secure conversations
  - Security validation rules
  - Transport protocol mappings: UA TCP, SOAP/HTTP, and HTTPS
  
  ### [Profiles](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-7-profiles/)
  - This specification describes categories of behaviors that OPC UA Servers and Clients can implement, covering:
  - Concepts of Profiles and conformance units
  - UA Server and UA Client *<u>categories for behavior, functionality supported, and supported security</u>*
  - Detailed descriptions of required behaviors and optional behaviors for each Profile, including *<u>nested Profiles</u>*

  ### [Data Access](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-8-data-access/)
  - This specification describes Data Access applications, covering:
  - Overview and concepts of Data Access and its evolution from the OPC Classic Data Access specifications.
  - Information model description and behavior rules of data-types
  - Address-space organization
  - Description and rules of the *<u>PercentDeadband</u>* behavior
  - Detailed description of *<u>error codes</u>* that are specific to this specification

  ### [Alarms and Conditions](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-9-alarms-and-conditions/)
  - This specification describes the Alarms & Conditions applications, covering:
  - Overview and concepts of Alarms & Conditions and its evolution from the OPC Classic Alarms & Events specification
  - Information model description and behavior rules, each data type, and expected behaviors
  - Address-space organization

  ### [Programs](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-10-programs/)
  - This specification describes Programs and how they can be used in OPC UA applications, covering:
  - Concepts of a Program, and where they can be used, life-times, and state
  - Information model description and behavior rules including program types, causes and effects, *<u>parameters</u>*, and *<u>return codes</u>*
  - Implementation of *<u>diagnostics</u>*
  - Example Program implementations

  ### [Historical Access](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-11-historical-access/)
  - This specifications describes how data can be archived and retrieved from a Historian/database, covering:
  - Overview and concepts of *<u>history for data and events</u>*, and its evolution from OPC Classic Historical Data Access
  - Information model description and *<u>behavior rules for nodes and each data-type and event</u>*
  - Detailed descriptions of behavior for *<u>creating, retrieving, updating, and deleting archived data</u>* and/or annotations/notes.
  - Detailed descriptions for *<u>security including access rights and auditing</u>*

  ### [Discovery and Global Services](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-12-discovery-and-global-services/)
  - This specification describes how *<u>UA products can be discovered and managed on a computer, network infrastructure, or enterprise-wide</u>*. Topics include:
  - The discovery process
  - Local Discovery Server (LDS) concepts
  - Global Discovery Server (GDS) concepts
  - Certificate Management for Push and Pull methods
  - Deployment and Configuration
  - KeyCredential Management
  - Authorization Services

  ### [Aggregates](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-13-aggregates/)
  - This specification describes the use of *<u>Aggregate functions</u>* for UA applications, covering:
  - The concepts of an aggregate and *<u>where they can and should be applied to generic applications and Historians</u>*
  - Detailed descriptions and behavior requirements of all *<u>37 aggregates</u>*
  - Information model description and behavior rules for each data type
  - Extensive library of reference material showing examples of queries and results for each aggregate

  ### [PubSub](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-14-pubsub/)
  - This specification defines the OPC Unified Architecture (OPC UA) PubSub communication model. The PubSub communication model defines an OPC UA publish subscribe pattern instead of the client server pattern defined by the Services in Part 4.
  - a general introduction of the concepts,
  - a definition of the PubSub communication parameters,
  - a PubSub configuration information model,
  - and mappings to messages and protocols

  ### [Safety](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-15-safety/)
  - It extends OPC UA to fulfill the requirements of functional safety as defined in the IEC 61508 and IEC 61784-3 series of standards.
  
  ### [State Machines](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-16-state-machines/)
  - This specification defines an OPC UA Information Model that describes state machines.

  ### [Alias Names](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-17-alias-names/)
  - This specification provides a definition of AliasNames functionality. AliasNames provide a manner of configuring and exposing an alternate well-defined name for any Node in the system.

  ### [Role-Based Security](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-18-role-based-security/)
  - This specification defines an OPC UA Information Model which manages role based security.

  ### [Dictionary Reference](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-19-dictionary-reference/)
  - This specification defines an Information Model of the OPC Unified Architecture. The Information Model describes the basic infrastructure to reference from an OPC UA Information Model to external dictionaries like IEC Common Data Dictionary or eCl@ss.

  ### [File Transfer](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-20-file-transfer/)
  - This specification defines an OPC UA Information Model which describes a *<u>files, file systems and mechanisms for transferring files to and from the Server</u>*.
  
  ### [Base Network Model](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-22-base-network-model/)
  - The Base Network Model (BNM) specifies an OPC UA Information Model for a basic set of network related components to be used in other OPC specifications.
  
  ### [Common ReferenceTypes](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-23-common-referencetypes/)
  - This specification describes an Information Model for common ReferenceTypes.
  
  ### [Device Information Model](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-100-device-information-model/)
  - Companion Specification featuring an Information Model for Devices.
  - The information model specification includes: *TopologyElementType*, *FunctionalGroupType*, *Identification Functional Group*, *DeviceType*, *DeviceSet Entry Point*, *ProtocolType*, *Uses Reference Type*, *Block Type*, and *Configurable Components*.
  
## Security
- During the development of the OPC UA standard, the highest degree of safety was taken into account from the very beginning.
- In contrast to OPC Classic, OPC UA was developed __*firewall-fiendly*__, i.e. it can be controlled and steered via standard network techniques.
- Several protocols have been made available on the transport layer. Thus, a binary protocol can be used directly on TCP/IP for fast applications or the cross-platform __*SOAP*__ with __*HTTPS*__.
- __*Encryptions of 128 or 256 bits*___ are used to secure the data during transmission, as well as __*message signing*__, __*packet sequencing*__ and __*user authentication*__.
- OPC UA uses a __*certificate exchange*__ for further security, so that *<u>each client has to authenticate with a certificate</u>*. In this way it can be controlled which client is allowed to connect to the server.

  ### Security in REST
  - Since HTTP is used to call REST endpoints, the __*authentications*__ available in the standard system can also be used, such as __*HttpBasic*__, __*JWT*__, __*Ntml*__, __*OAuth1*__, __*OAuth2*__.
  - In addition, __*https*__ is of course used instead of http in order to use *<u>a tap-proof connection</u>*.
  - Besides the standard authentication options, a so-called __*AppKey*__ is often exchanged. This *<u>key is a secret code created for the client, which is transferred with every call to get the authorization for the call</u>*. The so-called __*Bearer Token*__ is also supported by the OPC Router.
  - REST is considered secure due to the use of widely used methods.

## Tunneling (not an issue in OPC UA)
- Tunneling refers to the transfer of OPC data from one network segment to another.
- :x: The term originates from the time of OPC Classic, because due to the *DCOM* technology used, cross-network communication via OPC Classic is very difficult or almost impossible. Some software manufacturers have created a solution for this by encapsulating the data traffic and converting it into simple TCP/IP so that the data traffic could take its way through the firewall. In the target network, the data traffic was unpacked again and made available in an OPC Classic Server.
- :heavy_check_mark: With the OPC UA technology, tunneling is no longer necessary.
- If only one OPC Classic Server has already been installed and yet communication is to take place across networks, an OPC wrapper is required. For this an OPC UA Server is used, e.g. the KEPServerEX, to get the data as a client from the OPC Classic Server and to make this data available via OPC UA firewall friendly.

## OPC UA over TSN
- OPC UA over TSN was designed for *<u>real-time communication</u>* at the field level between control systems.
- The TSN stands for __*<u>Time Sensitive Network</u>*__ and describes the requirements of machine networks with deterministic response times.
- In contrast to the client-server operation of OPC UA, *<u>OPC UA over TSN</u>* communicates according to the __*Publisher-Subscriber method*__.
- For the use of OPC UA in normal Ethernet-based, non-real-time environments, OPC UA over TSN currently plays no role.

## Development with OPC UA
- [Python OPC-UA Documentation](https://python-opcua.readthedocs.io/en/latest/)
- [FreeOpcUa/python-opcua](https://github.com/FreeOpcUa/python-opcua)

  ---

###### References
- [What is OPC UA? A practical introduction](https://www.opc-router.com/what-is-opc-ua/)
- [OPC Unified Architecture Specification](https://opcfoundation.org/developer-tools/specifications-unified-architecture)



