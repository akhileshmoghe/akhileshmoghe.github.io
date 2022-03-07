---
title:  "AWS IoT Greengrass v2 Overview"
date:   2021-11-24 12:33:22 +0530
permalink: /_posts/aws/ml/iot/greengrass-v2-overview
categories:
  - AWS IoT
  - Edge IoT
  - IoT
  - Machine Learning
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - AWS IoT
  - Edge IoT
  - IoT
  - Machine Learning
author: Akhilesh Moghe
show_author_profile: true
---

- AWS IoT Greengrass is an Open-source IoT Edge Runtime and Cloud Service to build, deploy and manage IoT applications on edge devices.
- AWS IoT Greengrass enables devices to *<u>act locally on the data</u>*, *<u>run predictions based on machine learning models</u>*, and *<u>collect, analyze, filter and aggregate data</u>*, closer to where that data is generated, react autonomously to *<u>local events</u>*, *<u>communicate securely with other devices</u>* on the local network and with AWS IoT Core.
- AWS IoT Greengrass provides pre-built software components to connect edge devices to AWS or third-party services.
- AWS IoT Greengrass can package and run your software using *<u>Lambda functions</u>*, Docker *<u>containers</u>*, as a *<u>native OS Processes</u>*, or *<u>custom runtimes</u>* of your choice.

## Greengrass Core Device: 
- A device that runs the AWS IoT Greengrass Core software.
- *<u>A Greengrass Core device is an AWS </u>*__*<u>IoT thing</u>*__.
- You can add multiple Core devices to AWS IoT thing groups to create groups of Greengrass Core devices.
- You can configure the core device to relay MQTT messages between client devices, the AWS IoT Core cloud service, and Greengrass components.

### *<u>Greengrass Core device Discovery</u>*:
- *<u>Cloud Discovery</u>*:
  - To connect to a core device, client devices can use cloud discovery.
  - *<u>Client devices connect to the AWS IoT Greengrass cloud service to retrieve information about core devices to which they can connect</u>*.
  - Then, they can connect to a core device to process their messages and sync their data with the AWS IoT Core cloud service.
- *<u>Local Discovery</u>*:
  - :x: Greengrass does not provide any way for Local discovery of Greengrass Core device.
  - :x: This limits the use of Greengrass in total offline, behind firewall, air-gap deployment scenarios.
  - :warning: Workaround for this is to hard-code the Greengrass Core device IP and make sure that IP of the Greengrass device never changes.
  - :warning: Also, Greengrass Core device has TLS certificates for local MQTT communication, which needs to be rotated periodically (7-30 days). In order to use Greengrass core device in total offline, air-gap scenarios, this rotation policy for TLS certificates needs to be disabled from AWS cloud backend, contact AWS support for this.

## Greengrass Client Devices: 
- A device that connects to and communicates with a Greengrass core device over MQTT.
- *<u>A Greengrass client device is an AWS </u>*__*<u>IoT thing</u>*__.
- The core device can process, filter, and aggregate data from client devices that connect to it.
- Client devices can run [FreeRTOS](https://docs.aws.amazon.com/freertos/latest/userguide/freertos-lib-gg-connectivity.html) or use the [AWS IoT Device SDK](https://docs.aws.amazon.com/iot/latest/developerguide/iot-sdks.html) or [Greengrass discovery API](https://docs.aws.amazon.com/greengrass/v2/developerguide/greengrass-discover-api.html) to get information about core devices to which they can connect.

## Greengrass Components
- A Greengrass component is a software module that is deployed to and runs on a Greengrass Core device.
- AWS IoT Greengrass provides pre-built public components that provide features and functionality that you can use in your applications.
- You can also develop your own custom components.
- AWS IoT Greengrass Core software *<u>runs components as the system user and group</u>*, such as *<u>ggc_user</u>* and *<u>ggc_group</u>*, that you configure on the core device. This means that components have the permissions of that system user. In order to use, $HOME kind of functionality, you hav eto create ggc_user with a home directory.

  ### *<u>Recipe</u>*:
  - A JSON or YAML file that describes the software module by defining component details, configuration and parameters.
    - Component's *<u>configuration</u>* parameters
    - Component *<u>dependencies</u>*
    - *<u>Lifecycle</u>*
      - The component lifecycle defines: the commands that install, run, and shut down the component.
    - *<u>Platform compatibility</u>*.

  ### *<u>Artifact</u>*:
  - The source code, binaries, or scripts that define the software that will run on your device.
    - Create artifacts from scratch, using a Lambda function, a Docker container, or a custom runtime.
    - There can be any number of artifacts.
    - You can develop and *<u>test components on your Greengrass core device without interaction with the AWS Cloud</u>*.
      - May be useful for offline update mechanisms.
      - [Custom Edge Use-case]: Locally trained model can be deployed locally in a container or as a component.
      - Though it is meant for debugging and testing purpose.

  ### *<u>Dependency</u>*:
  - The *<u>relationship between components</u>* enables you to *<u>enforce automatic updates or restarts of dependent components</u>*.
  - For example, you can have a secure message processing component dependent on an encryption component. This ensures that any updates to the encryption component automatically update and restart the message processing component. 

  ### *<u>Component Lifecycle</u>*:
  - The component lifecycle defines the __*<u>stages</u>*__ that the AWS IoT Greengrass Core software uses to install and run components.
  - *<u>Each stage defines </u>*__*<u>a script</u>*__ and other information that specifies how the component behaves.
  - *<u>Lifecycle stages</u>*:
    - NEW, INSTALLED, STARTING, RUNNING, FINISHED, STOPPING, ERRORED, BROKEN

  ### *<u>Component Types</u>*:
  - __*<u>Nucleus</u>*__:
    - Greengrass nucleus is the component that provides the minimum functionality of the AWS IoT Greengrass Core software.
  - __*<u>Plugin</u>*__:
    - Greengrass nucleus runs a plugin component in the same Java Virtual Machine (JVM) as the nucleus.
    - The nucleus restarts when you change the version of a plugin component on a core device.
    - To install and run plugin components, you must configure the Greengrass nucleus to *<u>run as a system service</u>*.
    - Plugin components use the same log file as the Greengrass nucleus.
    - Several components that are provided by AWS are plugin components.
  - __*<u>Generic</u>*__:
    - Greengrass nucleus runs a generic component's lifecycle scripts, if the component defines a lifecycle.
    - This type is the *<u>default type for custom components</u>*.
  - __*<u>Lambda</u>*__:
    - Greengrass nucleus runs a Lambda function component using the [Lambda launcher component](https://docs.aws.amazon.com/greengrass/v2/developerguide/lambda-launcher-component.html).

## Greengrass Device Management:
### [*<u>Greengrass Core Setup</u>*](https://docs.aws.amazon.com/greengrass/v2/developerguide/setting-up.html)
- *<u>Supported Platform</u>*:
  - __*Linux*__: Armv7l, Armv8 (AArch64), x86_64
  - __*Windows*__: x86_64
    - Versions: Windows 10, Windows Server 2019
    - :x: Windows does not have support for many components, features at this stage, not even Lambda functions.
    - [Greengrass feature compatibility by operating system](https://docs.aws.amazon.com/greengrass/v2/developerguide/operating-system-feature-support-matrix.html)
  - __*Embedded Linux*__:
    - BitBake recipe for AWS IoT Greengrass V2 in the [meta-aws project](https://github.com/aws/meta-aws/tree/master/recipes-iot)
    - BitBake recipe for AWS IoT Greengrass V2 installs, configures, and automatically runs the AWS IoT Greengrass Core software on your device.
- *<u>Device Requirements</u>*:
  - Minimum 256 MB disk space allocated to the AWS IoT Greengrass Core
  - Minimum 96 MB RAM allocated to the AWS IoT Greengrass Core
  - Java Runtime Environment (JRE) version 8 or greater.
  - AWS IoT Greengrass Core software (typically root), must have permission to run `sudo` with any user and any group.
  - `/tmp` directory must be mounted with `exec` permissions.
  - To configure system resource limits for component processes, your device must run Linux kernel version 2.6.24 or later.
  - [Lambda function requirements](https://docs.aws.amazon.com/greengrass/v2/developerguide/setting-up.html#greengrass-v2-lambda-requirements)

### [*<u>Greengrass Installation</u>*](https://docs.aws.amazon.com/greengrass/v2/developerguide/install-greengrass-core-v2.html)
- Required AWS IoT and IAM resources to connect to the AWS Cloud and operate:
  - An AWS IoT thing
    - Useful for [Device authentication and authorization for AWS IoT Greengrass](https://docs.aws.amazon.com/greengrass/v2/developerguide/device-auth.html)
  - (Optional) An AWS IoT thing group
    - Useful for [Deploy AWS IoT Greengrass components to devices](https://docs.aws.amazon.com/greengrass/v2/developerguide/manage-deployments.html)
  - An IAM role
    - Used to [Authorize core devices to interact with AWS services](https://docs.aws.amazon.com/greengrass/v2/developerguide/device-service-role.html)
  - An AWS IoT role alias
    - Greengrass core devices use the role alias to identify the IAM role to use. The role alias enables you to change the IAM role but keep the device configuration the same.
    - Reference: [Authorizing direct calls to AWS services](https://docs.aws.amazon.com/iot/latest/developerguide/authorizing-direct-aws.html)
- Options for installations:
  - [Install with automatic provisioning](https://docs.aws.amazon.com/greengrass/v2/developerguide/quick-installation.html)
    - The installer creates the required AWS IoT and IAM resources.
    - This option requires you to provide AWS credentials to the installer to create resources in your AWS account.
    - You can't use this option to install behind a firewall or network proxy.
  - [Install with manual provisioning](https://docs.aws.amazon.com/greengrass/v2/developerguide/manual-installation.html)
    - Used to install behind a firewall or network proxy.
    - Configure your device to connect on port 443 or through a network proxy.
    - Installer does not need AWS credentials.
    - Configure the AWS IoT Greengrass Core software to use a *<u>private key and certificate</u>* that you store in a __*hardware security module (HSM)*__, __*Trusted Platform Module (TPM)*__, or another cryptographic element.
  - [Install with fleet provisioning](https://docs.aws.amazon.com/greengrass/v2/developerguide/fleet-provisioning.html)
    - Create the required AWS resources from an AWS IoT fleet provisioning template.
    - Choose this option to create similar devices in a fleet, if you manufacture devices that your customers later activate.
    - Devices use __*claim certificates*__ to authenticate and provision AWS resources, including an X.509 client certificate that the device uses to connect to the AWS Cloud.
    - Embed claim certificates into device's hardware.
    - Can use the same claim certificate and key to provision multiple devices.
  - [Install with custom provisioning](https://docs.aws.amazon.com/greengrass/v2/developerguide/custom-provisioning.html)
    - Choose this option if you create your own X.509 client certificates or if you want more control over the provisioning process.
  - [Installer arguments](https://docs.aws.amazon.com/greengrass/v2/developerguide/configure-installer.html)
  
  ### [*<u>Run AWS IoT Greengrass Core software in a Docker container</u>*](https://docs.aws.amazon.com/greengrass/v2/developerguide/run-greengrass-docker.html)
  - AWS IoT Greengrass also provides containerized environments that run the AWS IoT Greengrass Core software.
  - You can provision the required AWS Cloud Resources in advance, or let the AWS IoT Greengrass installer do it for you as in quick install process. Greengrass container image supports both ways.
    - To use [*<u>automatic provisioning</u>*](https://docs.aws.amazon.com/greengrass/v2/developerguide/run-greengrass-docker-automatic-provisioning.html), you must set the Docker environment variable `PROVISION=true` and mount a `credential file` to provide your AWS credentials to the container.
    - To use [*<u>manual provisioning</u>*](https://docs.aws.amazon.com/greengrass/v2/developerguide/run-greengrass-docker-manual-provisioning.html), you must set the Docker environment variable `PROVISION=false`. Manual provisioning is the `default` option.
  - Minimum Requirements:
    - A __*Linux*__-based operating system with an internet connection.
    - __*Docker Engine*__ version 18.09 or later.
    - (Optional) __*Docker Compose*__ version 1.22 or later.
      - Docker Compose is required only if you want to use the Docker Compose CLI to run your Docker images.
    - [Lambda function requirements](https://docs.aws.amazon.com/greengrass/v2/developerguide/setting-up.html#greengrass-v2-lambda-requirements):
      - To *<u>run Lambda function components </u>*__*<u>inside of the Docker container</u>*__.
  - __*<u>Run AWS Greengrass components in process mode</u>*__
    - :x: AWS IoT Greengrass doesn't support running Lambda functions or AWS-provided components in an isolated runtime environment inside the AWS IoT Greengrass Docker container. You must *<u>run these components in process mode without any isolation</u>*.
    - Configure a Lambda function component, set the isolation mode to *<u>No container</u>*.
    - Update the configuration for each AWS Component to set the containerMode parameter to *<u>No container</u>*.
    - Following components are vaialble to be used with containerized Greengrass:
      - [CloudWatch metrics](https://docs.aws.amazon.com/greengrass/v2/developerguide/cloudwatch-metrics-component.html)
      - [Device Defender](https://docs.aws.amazon.com/greengrass/v2/developerguide/device-defender-component.html)
      - [Kinesis Data Firehose](https://docs.aws.amazon.com/greengrass/v2/developerguide/kinesis-firehose-component.html)
      - [Modbus-RTU protocol adapter](https://docs.aws.amazon.com/greengrass/v2/developerguide/modbus-rtu-protocol-adapter-component.html)
      - [Amazon SNS](https://docs.aws.amazon.com/greengrass/v2/developerguide/sns-component.html)
  - [AWS IoT Greengrass Dockerfile](https://github.com/aws-greengrass/aws-greengrass-docker/releases/)
    - [Build the AWS IoT Greengrass container image from a Dockerfile](https://docs.aws.amazon.com/greengrass/v2/developerguide/build-greengrass-dockerfile.html)
  - [Pre-built AWS IoT Greengrass](https://hub.docker.com/r/amazon/aws-iot-greengrass)

  ### [*<u>Run the AWS IoT Greengrass Core software as a system service or without a service</u>*](https://docs.aws.amazon.com/greengrass/v2/developerguide/run-greengrass-core-v2.html)

  ### [*<u>Configure the AWS IoT Greengrass Core software</u>*](https://docs.aws.amazon.com/greengrass/v2/developerguide/configure-greengrass-core-v2.html)

  ### [*<u>Update the AWS IoT Greengrass Core software (OTA)</u>*](https://docs.aws.amazon.com/greengrass/v2/developerguide/update-greengrass-core-v2.html)
  - Points to consider before update:
    - :warning: The Greengrass nucleus shuts down.
    - :warning: All components running on the core device also shut down. If those components write to local resources, they might leave those resources in an incorrect state unless shut down properly. Components can use interprocess communication to tell the nucleus component to defer the update until they clean up the resources that they use.
    - :warning: While the nucleus component is shut down, the core device loses its connections with the AWS Cloud and local devices.
    - :warning: Long-lived Lambda functions that run as components lose their dynamic state information and drop all pending work.
    - [Greengrass nucleus update behavior](https://docs.aws.amazon.com/greengrass/v2/developerguide/update-greengrass-core-v2.html#ota-update-behavior-nucleus)
    - To perform an OTA update, *<u>create a deployment that includes the </u>*__*<u>nucleus component</u>*__*<u> and the version to install</u>*.

  ### [*<u>Uninstall the AWS IoT Greengrass Core software</u>*](https://docs.aws.amazon.com/greengrass/v2/developerguide/uninstall-greengrass-core-v2.html)
 

###### References



