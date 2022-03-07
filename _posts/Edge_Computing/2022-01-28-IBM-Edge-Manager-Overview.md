---
title:  "IBM Edge Application Manager Overview"
permalink: /_posts/edge-computing/ibm-edge-manager
date:   2022-01-28 12:33:22 +0530
categories:
  - Edge IoT
  - Microservices
  - Edge Computing
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
#full_width: true
tags:
  - Edge IoT
  - Microservices
  - Edge Computing
author: Akhilesh Moghe
show_author_profile: true
---

- Manages and deploys workloads from a management hub cluster to *<u>edge devices</u>* and *<u>remote instances of </u>*__*<u>OpenShift Container Platform</u>*__ or other __*Kubernetes*__-based clusters.
- IEAM is designed to harness the disciplines of hybrid cloud computing to support remote operations of edge computing facilities.

## Architecture Overview
![IEAM Overview](/assets/images/edge-computing/ibm-edge-manager/01_IEAM_overview.svg){:.shadow}

- IAEM deployment includes a __*Management Hub*__ that runs in an instance of [OpenShift Container Platform](https://www.redhat.com/en/technologies/cloud-computing/openshift/container-platform) installed in your *<u>data center</u>*.
- The management hub *<u>manages of all of your remote edge nodes</u>*, be it __*edge devices*__ and __*edge clusters*__.
- The management hub *<u>minimize deployment risks</u>* and to *<u>manage the service software lifecycle on edge nodes</u>* fully autonomously.
- __*edge nodes*__ can be installed in *<u>remote on-premises locations</u>* to make your *<u>application workloads local</u>* to where your critical business operations physically occur, such as at your factories, warehouses, retail outlets, distribution centers, and more.
- Software developers develop and publish edge services to the management hub.
- Administrators define the deployment policies that control where edge services are deployed.

## Components of IEAM
- [*<u>IBM Cloud Platform Common Services</u>*](https://www.ibm.com/support/knowledgecenter/SSHKN6/kc_welcome_cs.html):
  - This is a set of foundational components that are installed automatically as part of the IEAM operator installation.
- __*<u>Agbot</u>*__:
  - __*Agreement bot (agbot) instances*__ are created centrally and are *<u>responsible for deploying</u>* __*workloads*__ and __*machine learning models*__ to IEAM.
- [*<u>Model Management System (MMS)</u>*](https://www.ibm.com/docs/en/eam/4.3?topic=reading-model-management-details):
  - The __*MMS*__ facilitates the *<u>storage, delivery, and security of models, data, and other metadata packages</u>* needed by edge services.
  - This *<u>enables edge nodes to easily send and receive models and metadata</u>* __*to and from the*__ [cloud](https://www.ibm.com/in-en/cloud/internet-of-things).
  - This service can be useful for *<u>remotely updating the configuration of your Services</u>* in the field.
  - Enables you to dynamically push new versions of the models to your small edge machines in the field.
  - The MMS enables you to *<u>manage the lifecycle of </u>*__*<u>data files</u>*__*<u> on your edge node</u>*, remotely and independently from your code updates.
  - In general the MMS provides facilities for you to securely send any data files to and from your edge nodes.
  - The MMS runs in the *<u>IBM Edge Application Manager (IEAM) hub</u>* and on *<u>edge nodes</u>*.
  - The __*Cloud Sync Service(CSS)*__, *<u>delivers models, metadata, or data to specific nodes</u>* or groups of nodes within an organization. After the objects are delivered to the edge nodes, *<u>an API is available that enables the edge service to obtain the models or data from the</u>* __*Edge Sync Service (ESS)*__.
  - Objects are published in the MMS by service developers, devops administrators, and model authors, which makes them immediately available to edge nodes.
  - *<>Integrity & Verification of Models</u>*:
    - By default, integrity of the model is ensured by *<u>hashing and signing the model</u>*, and then uploading the signature and verification key before publishing the model.
    - The MMS uses the signature and key to verify that the uploaded model has not been tampered with.
    - This same procedure is also used when the MMS deploys the model to edge nodes.
  - IEAM also provides a CLI (__*hzn mms*__) that enables manipulation of the model objects and their metadata.
  - Several components make up the MMS: __CSS__, __ESS__, and __objects__.
    - __*CSS*__:
      - The CSS is *<u>deployed in the IEAM management hub</u>* when IEAM is installed.
      - The CSS uses the __*MongoDB database*__ to store objects and maintain the status of each edge node.
    - __*ESS*__:
      - The ESS is *<u>embedded in the IEAM agent</u>* that runs on the edge node.
      - The ESS *<u>continually polls the CSS</u>* for object updates and *<u>stores any objects</u>* that are delivered to the node *<u>in a local database on the edge node</u>*.
      - *<u>ESS APIs can be used by services</u>* that are deployed on the edge node *<u>to access the metadata and data or model objects</u>*.
    - __*Objects (metadata and data)*__:
      - Metadata describes your data models. An object is published to the MMS with metadata and data, or with metadata only.
  - Few MMS Commands:
    - Check the MMS status: `hzn mms status`
    - Create the MMS Object: `hzn mms object new`
    - Publish the MMS object: `hzn mms object publish -m my_metadata.json -f my_model`
    - List the MMS object: `hzn mms object list --objectType=model --objectId=my_model`
    - Delete the MMS object: `hzn mms object delete --type=model --id=my_model`
    - Update the MMS object: `hzn mms object publish -m my_metadata.json -f my_updated_model`
- __*<u>Exchange API</u>*__:
  - The Exchange provides the *<u>REST API</u>* that holds all the definitions (*<u>patterns, policies, services</u>*, and so on) used by all the other components in IEAM.
  - It is the *<u>Centralized Service</u>* in IEAM.
- __*<u>Management console</u>*__:
  - The *<u>web UI</u>* used by IEAM administrators to view and manage the other components of IEAM.
- __*<u>Secure Device Onboard</u>*__:
  - The __*SDO*__ component enables technology that is created by *<u>Intel</u>* to make it simple and secure to *<u>configure edge devices</u>* and *<u>associate them with an edge management hub</u>*.
- __*<u>Cluster Agent</u>*__:
  - This is a *<u>container image</u>*, which is installed as __*an agent on edge clusters*__ *<u>to enable node workload management</u>* by IEAM.
- __*<u>Device Agent</u>*__:
  - This component is installed on __*edge devices*__ to enable *<u>node workload management</u>* by IEAM.
- [*<u>Secrets Manager</u>*](https://www.ibm.com/docs/en/eam/4.3?topic=reading-secrets-manager-details):
  - The Secrets Manager is the *<u>repository for secrets deployed to edge devices</u>*, enabling services to *<u>securely receive credentials used to authenticate to the cloud services</u>*.
  - A secret can be a *<u>userid/password</u>*, *<u>certificate</u>*, *<u>RSA key</u>*, or any other credential that grants access to a protected resource, which an edge application needs in order to perform it's function. All these Secrets are stored in the Secrets Manager.
  - Administrator can access Secrete Manager using UI or CLI.
  - The Secrets Manager is a pluggable component of IEAM.
  - The details of every IEAM secret are composed of *<u>a key and a value</u>*. Both are stored as part of the details of the secret, and both can be set to any string value. The type of secret on the key and sensitive information as the value.
  - Currently, [HashiCorp Vault](https://www.vaultproject.io/) is the only supported Secrets Manager.
    - HashiCorp Vault is a secrets management tool specifically designed to control access to sensitive credentials in a low-trust environment.
    - It has 2 versions:
      - Open-source - Self Managed
      - Cloud - Managed Vault - paid
  - Example: [Hello Secret World Example](https://github.com/open-horizon/examples/blob/master/edge/services/helloSecretWorld/CreateService.md)


## Edge Services
- An __*edge service*__ can let you create an associated *<u>deployment pattern</u>*, and *<u>register your edge nodes</u>* to run that deployment pattern.

### Deployment Policies
- IBM Edge Application Manager uses __*policies*__ to determine where and when to autonomously __*deploy*__ *<u>services and machine learning models</u>*.
- Types of Policies:
  - Node Policy
  - Service Policy
  - Deployment Policy
  - Model Policy
- Policies have mainly 2 attribute sections:
  - Properties
    - Node built-in properties
    - Server built-in properties
  - Constraints
- There are few Policy Commands also, using which we can apply, revoke, list, update policies.
- Further Reference: [Deployment policy use cases](https://www.ibm.com/docs/en/eam/4.3?topic=services-deployment-policy-use-cases)

### Deployment Patterns
- Using a deployment pattern is a straightforward and simple way to deploy a service to your edge node.
- You specify the top-level service, or multiple top-level services, to be deployed to your edge node and IEAM handles the rest, including the deployment of any dependent services your top-level service might have.
- After you create and add a service to the IEAM exchange, you need to create a __*pattern.json*__ file.
- Further Reading: [Using patterns](https://www.ibm.com/docs/en/eam/4.3?topic=services-using-patterns)

### Developing edge services
- [Developing an edge service for devices](https://www.ibm.com/docs/en/eam/4.3?topic=services-developing-edge-service-devices)
  ![Developing an edge service for devices](/assets/images/edge-computing/ibm-edge-manager/03a_Developing_edge_service_for_device.svg){:.shadow}

- [Developing an edge service for clusters](https://www.ibm.com/docs/en/eam/4.3?topic=services-developing-edge-service-clusters)
  ![Developing an edge service for clusters](/assets/images/edge-computing/ibm-edge-manager/03b_Developing_edge_service_for_cluster.svg){:.shadow}


### Example Edge Services
- [Transform image to edge service](https://www.ibm.com/docs/en/SSFKVV_4.3/OH/docs/developing/transform_image.html)
  - Demonstrates deploying an existing docker image as an edge service.
- [Creating your own hello world edge service](https://www.ibm.com/docs/en/SSFKVV_4.3/OH/docs/developing/developingstart_example.html)
  - Demonstrates the basics of developing, testing, publishing, and deploying an edge service.
- [CPU to IBM Event Streams service](https://www.ibm.com/docs/en/SSFKVV_4.3/OH/docs/developing/cpu_msg_example.html)
  - Demonstrates how to define edge service configuration parameters, specify that your edge service requires other edge services, and send data to a cloud data ingest service.
- [Hello world using model management](https://www.ibm.com/docs/en/SSFKVV_4.3/OH/docs/developing/model_management_system.html)
  - Demonstrates how to develop an edge service that uses the model management service. The model management service asynchronously provides file updates to edge services on edge nodes, for example to dynamically update a machine learning model each time it evolves.
- [Hello world using secrets](https://www.ibm.com/docs/en/SSFKVV_4.3/OH/docs/developing/developing_secrets.html)
  - Demonstrates how to develop an edge service that uses secrets. Secrets are used to protect sensitive data like login credentials or private keys. Secrets are deployed securely to services running on the edge.
- [Updating an edge service with rollback](https://www.ibm.com/docs/en/SSFKVV_4.3/OH/docs/using_edge_services/service_rollbacks.html)
  - Demonstrates how to monitor the success of the deployment, and if it fails on any edge node, revert the node back to the previous version of the edge service.


## Installation of IEAM Management Hub
![Management hub installation flow](/assets/images/edge-computing/ibm-edge-manager/06_IEAM_management_hub_install.svg){:.shadow}

### [Sizing and system requirements](https://www.ibm.com/docs/en/eam/4.3?topic=hub-sizing-system-requirements)
- *<u>IEAM database considerations</u>*:
  - __*Local databases*__ are installed (by default) as *<u>OpenShift resources onto your OpenShift cluster</u>*.
  - __*remote databases*__ are databases that you provisioned, which can be *<u>on-premises, cloud provider</u>* SaaS offerings, and so on.
    - At a minimum, provision Remote databases with the following resources and settings:
    - 2vCPU's
    - 2 GB of RAM
    - The default storage sizes mentioned in the previous section
    - For the PostgreSQL databases, 100 max_connections (which is typically the default)
- *<u>IEAM local storage requirements</u>*:
  - Following are default settings for storage requirements:
  - __*PostgreSQL Exchange*__ (Stores data for the __*Exchange*__, and fluctuates in size depending on usage, but the default storage setting can support up to the advertised limit of edge nodes)
    - 20 GB
  - __*PostgreSQL AgBot*__ (Stores data for the __*AgBot*__, the default storage setting can support up to the advertised limit of edge nodes)
    - 20 GB
  - __*MongoDB Cloud Sync Service*__ (Stores content for the __*Model Management Service (MMS)*__). Depending on the number and size of your models, you might want to modify this default allocation
    - 50 GB
  - __*Hashicorp Vault*__ *<u>persistent volume</u>* (Stores secrets used by edge device services)
    - 10 GB (This volume size is not configurable)
  - __*Secure Device Onboarding*__ persistent volume (Stores device ownership vouchers, device configuration options, and the deployment status of each device)
    - 1 GB (This volume size is not configurable)
- *<u>Worker node sizing</u>*:
  - Minimal requirements for the default IEAM configuration:
  - Number of worker nodes = 3
  - vCPUs per worker node = 8
  - Memory per worker node (GB) = 32
  - Local disk storage per worker node (GB) = 100

### Installing IEAM
- Following components are deployed as installation IEAM:
  - IBM Cloud Platform Common Services 3.6.x.
  - IBM Edge Application Manager Management Hub operator.
  - IBM Edge Application Manager exchange API.
  - IBM Edge Application Manager agbot.
  - IBM Edge Application Manager Cloud Sync Service (CSS).
  - IBM Edge Application Manager user interface.
  - IBM Edge Application Manager Secure Device Onboarding (SDO).
  - IBM Edge Application Manager Secrets manager (Vault).

- Further Installation Details:
  - [Browser installation process](https://www.ibm.com/docs/en/eam/4.3?topic=installation-install-ieam)

### Installing IEAM in an air gap environment
- If your cluster is not connected to the internet, you can install IEAM in your cluster by using a bastion host, portable compute device, or a portable storage device.
  - A bastion host is a special-purpose computer on a network specifically designed and configured to withstand attacks. The computer generally hosts a single application or process, for example, a proxy server or load balancer, and all other services are removed or limited to reduce the threat to the computer.
![Installing IEAM in an air gap environment](/assets/images/edge-computing/ibm-edge-manager/airgap_scenarios.svg)
- All of these scenarios use __*Container Application Software for Enterprises (CASE)*__ files to mirror content from a source to a target. __*CASE*__ is *<u> a specification that defines metadata and structure for packaging, managing, and unpacking containerized applications</u>*.
- [Installing IEAM in an air gap environment](https://www.ibm.com/docs/en/eam/4.3?topic=installation-installing-ieam-in-air-gap-environment)
  - Further Readings, steps for air-gap installation:
    - [Set up your image registry access and mirroring environment (One-time action)](https://www.ibm.com/docs/en/eam/4.3?topic=installation-installing-ieam-in-air-gap-environment#setup)
      - [Prerequisites including preparing a host](https://www.ibm.com/docs/en/eam/4.3?topic=installation-installing-ieam-in-air-gap-environment#prereqs)
      - [Set up local image registry and access](https://www.ibm.com/docs/en/eam/4.3?topic=installation-installing-ieam-in-air-gap-environment#registry)
    - [Set environment variables and download CASE files](https://www.ibm.com/docs/en/eam/4.3?topic=installation-installing-ieam-in-air-gap-environment#pcenv)
    - [Mirror images depending on installation scenario](https://www.ibm.com/docs/en/eam/4.3?topic=installation-installing-ieam-in-air-gap-environment#mirror)
      - [Mirror images to your host](https://www.ibm.com/docs/en/eam/4.3?topic=installation-installing-ieam-in-air-gap-environment#pcmirrorhost)
      - [Copy saved offline data (for a portable storage device only)](https://www.ibm.com/docs/en/eam/4.3?topic=installation-installing-ieam-in-air-gap-environment#offlinedata)
      - [Connect your host to your air-gapped environment and set up your container](https://www.ibm.com/docs/en/eam/4.3?topic=installation-installing-ieam-in-air-gap-environment#connectpcairgap)
      - [Mirror images to final location and configure the cluster](https://www.ibm.com/docs/en/eam/4.3?topic=installation-installing-ieam-in-air-gap-environment#configcluster)
    - [Install IBM Edge Application Manager by way of Red Hat OpenShift Container Platform](https://www.ibm.com/docs/en/eam/4.3?topic=installation-installing-ieam-in-air-gap-environment#install)
      - [Create the catalog source and install IBM Edge Application Manager](https://www.ibm.com/docs/en/eam/4.3?topic=installation-installing-ieam-in-air-gap-environment#install_fs)
      - [Access the management console](https://www.ibm.com/docs/en/eam/4.3?topic=installation-installing-ieam-in-air-gap-environment#console)
      - [Retrieve your management console username and password](https://www.ibm.com/docs/en/eam/4.3?topic=installation-installing-ieam-in-air-gap-environment#lp)
    - [Post installation configuration](https://www.ibm.com/docs/en/eam/4.3?topic=installation-post)
      - Prerequisites
        - [IBM Cloud Pak CLI (cloudctl) and OpenShift client CLI (oc)](https://www.ibm.com/docs/en/SSFKVV_4.3/cli/cloudctl_oc_cli.html) Note: IEAM must be installed before you can download cloudctl.
          - It installs cloudctl, kubectl, and oc
        - [jq](https://stedolan.github.io/jq/download/)
        - [git](https://git-scm.com/downloads)
        - [docker](https://docs.docker.com/get-docker/) version 1.13 or greater
        - make

    - [Gather edge node files](https://www.ibm.com/docs/en/eam/4.3?topic=installation-gather-edge-node-files)
      - Several files are needed to install the IBM Edge Application Manager (IEAM) agent on your edge devices and edge clusters and register them with IEAM. This content guides you through bundling the files that are needed for your edge nodes.
    - [Creating your API key](https://www.ibm.com/docs/en/eam/4.3?topic=installation-creating-your-api-key)
      - Before edge nodes are set up, you or your node technicians must create an API key and gather other environment variable values.


### Upgrades of IEAM Management Hub
- Upgrading the IEAM Management Hub is provided from the OCP web console for the cluster.
- Further Details: [Upgrades](https://www.ibm.com/docs/en/eam/4.3?topic=hub-upgrade)
- Upgrading edge nodes:
  - Existing IEAM nodes do not upgrade automatically.
  - In order to upgrade the IBM Edge Application Manager agent on your edge devices and edge clusters, you first need to put the 4.3.0 edge node files into the Cloud Sync Service (CSS).
  - Further Details: [Upgrades](https://www.ibm.com/docs/en/eam/4.3?topic=hub-upgrade)


## Installation of Edge Devices
![Installation of Edge Devices](/assets/images/edge-computing/ibm-edge-manager/05a_Installing_edge_agent_on_device.svg){:.shadow}

### Preparing an edge device
- All edge devices (edge nodes) require the __*Horizon agent software*__ to be installed.
- The Horizon agent also depends upon __*Docker*__ software.
- Support:
  - Linux x86_64 devices
  - Ubuntu 20.x (focal), Ubuntu 18.x (bionic), Debian 10 (buster), Debian 9 (stretch)
  - Linux on ARM (32-bit)
    - Raspberry Pi, running Raspbian buster or stretch
  - Linux on ARM (64-bit)
    - NVIDIA Jetson Nano, TX1, or TX2, running Ubuntu 18.x (bionic)
  - MacOS
- 100 MB random access memory (RAM)
- 400 MB disk (including docker). Disk increases this amount based on the size of the container images
- Further Details: [Preparing an edge device](https://www.ibm.com/docs/en/SSFKVV_4.3/installing/adding_devices.html)

### Installing and registering edge devices
- Four different methods are provided to install the agent on and register edge devices, each is useful in different circumstances:
  - [Automated agent installation and registration](https://www.ibm.com/docs/en/SSFKVV_4.3/installing/automated_install.html)
    - Install and register one edge device with a minimum of steps.
    - First-time users should use this method.
  - [Advanced manual agent installation and registration](https://www.ibm.com/docs/en/SSFKVV_4.3/installing/advanced_man_install.html)
    - Do each step yourself to install and register one edge device.
    - Use this method if you need to do something out of the ordinary and the script that is used in Method 1 does not have the required flexibility.
    - You can also use this method if you want to understand exactly what is required to set up an edge device.
    - *<u>Install Steps in brief</u>*:
      - Obtain the `agentInstallFiles-<edge-device-type>.tar.gz` file and the API Key that is created along with this file.
        - As a post-configuration step for Installing the management hub, a compressed file was created. This file contains the necessary files to install the Horizon agent on your edge device and register it with the helloworld example.
      - Copy this file to the edge device with a USB stick, the secure copy command, or another method.
      - Install Horizon install .deb file: `apt update && apt install ./*horizon*.deb`
      - Point your edge device horizon agent to your IBM Edge Application Manager cluster by populating /etc/default/horizon with the correct information.
      - Have the *<u>horizon agent trust</u>* `agent-install.crt`.
      - Restart the agent to pick up the changes to `/etc/default/horizon`.
      - Verify that the agent is running and correctly configured.
    - *<u>Edge Device Registration Steps in brief</u>*:
      - Set your specific information as environment variables: `export HZN_EXCHANGE_USER_AUTH=iamapikey:<api-key>`
      - View the list of sample edge service deployment patterns: `hzn exchange pattern list IBM/`
      - Register your edge device with Horizon to run the helloworld deployment pattern: `hzn register -p IBM/pattern-ibm.helloworld`
        - The __*<u>node ID</u>*__ is shown in the output of above command in the line that starts with Using node ID.
      - The edge device will make an agreement with one of the Horizon agreement bots. `hzn agreement list`
      - After the agreement is made, list the docker container edge service that started as a result: `sudo docker ps`
      - View the helloworld edge service output: `sudo hzn service log -f ibm.helloworld`
  - [Bulk agent installation and registration](https://www.ibm.com/docs/en/SSFKVV_4.3/installing/many_install.html#batch-install)
    - Install and register many edge devices at one time.
  - [SDO agent installation and registration](https://www.ibm.com/docs/en/SSFKVV_4.3/installing/sdo.html)
    - Automatic install with SDO devices.
- Further Reading: [Updating the agent](https://www.ibm.com/docs/en/eam/4.3?topic=devices-updating-agent)


## Installation of Edge Clusters
- Manage and Deploy workloads from a management hub cluster to remote instances of __*OpenShift® Container Platform*__ or other __*Kubernetes-based clusters*__.
- IEAM deploys edge services to an edge cluster, via a *<u>Kubernetes operator</u>*, enabling the same autonomous deployment mechanisms used with edge devices.
- The full power of Kubernetes as a container management platform is available for edge services that are deployed by IEAM.
![Installation of Edge Clusters](/assets/images/edge-computing/ibm-edge-manager/05b_Installing_edge_agent_on_cluster.svg){:.shadow}

### Preparing an edge cluster
- [Preparing an edge cluster](https://www.ibm.com/docs/en/eam/4.3?topic=clusters-preparing-edge-cluster)
  - [Install an OCP edge cluster](https://www.ibm.com/docs/en/eam/4.3?topic=clusters-preparing-edge-cluster#install_ocp_edge_cluster)
  - [Install and configure a k3s edge cluster](https://www.ibm.com/docs/en/eam/4.3?topic=clusters-preparing-edge-cluster#install_k3s_edge_cluster)
  - [Install and configure a microk8s edge cluster](https://www.ibm.com/docs/en/eam/4.3?topic=clusters-preparing-edge-cluster#install_microk8s_edge_cluster)
    - for development and test, not recommended for production

### Installing the agent from Edge Clusters
- Begin by installing the IBM Edge Application Manager agent on one of these types of Kubernetes edge clusters:
  - [Installing agent on OCP Kubernetes edge cluster](https://www.ibm.com/docs/en/eam/4.3?topic=clusters-installing-agent#install_kube)
  - [Installing agent on k3s and microk8s edge clusters](https://www.ibm.com/docs/en/eam/4.3?topic=clusters-installing-agent#install_lite)
- Then, deploy an edge service to your edge cluster:
  - [Deploying services to your edge cluster](https://www.ibm.com/docs/en/eam/4.3?topic=clusters-installing-agent#deploying_services)

### Uninstall the agent from Edge Clusters
  - [Removing agent from edge cluster](https://www.ibm.com/docs/en/SSFKVV_4.3/using_edge_services/removing_agent_from_cluster.html)


###### References:
- 



