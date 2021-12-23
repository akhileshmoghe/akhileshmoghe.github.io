---
title:  "AWS Greengrass Service V2: a Service to build, deploy and manage IoT applications on Edge Devices"
date:   2021-11-12 12:33:22 +0530
permalink: /posts/aws/iot/greengrass_v2
categories:
  - On-Premise Cloud
  - Cloud
  - AWS
  - Edge IoT
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - On-Premise Cloud
  - Cloud
  - AWS
  - Edge IoT
author: Akhilesh Moghe
show_author_profile: true
---

This write-up will only focus on AWS Sagemaker ML service with respect to ML model deployment to Edge devices and Cloud.

## ML Inference on Greengrass devices
  - ML models can be trained using __*<u>AWS Sagemaker</u>*__ or custom ml trainining ways. Models are stored in __*<u>AWS S3</u>*__ for deployment to Greengrass devices.
  - ML models are deployed as artifacts in your components to perform inference on your core devices.

### ML Components
  - AWS provides following Machine Learning components that can be deployed to edge devices to perform Machine Learning Inference.
  - ML models can be trained using __*<u>AWS Sagemaker</u>*__ or custom ml trainining ways. Models are stored in __*<u>AWS S3</u>*__ for deployment to Greengrass devices.
  - AWS-provided machine learning components are broadly categorized as follows:
    - __*Model component*__ — Contains machine learning models as *<u>Greengrass artifacts</u>*.
    - __*Runtime component*__ — Contains the *<u>script</u>* that installs the machine learning framework and its dependencies on the Greengrass core device.
    - __*Inference component*__ — Contains the *<u>inference code</u>* and includes *<u>component dependencies</u>* to install the machine learning framework and download pre-trained machine learning models.
  - To perform custom machine learning inference with *<u>your own models</u>* that are stored in __*<u>Amazon S3</u>*__, or to use a *<u>different machine learning framework</u>*, you can use the recipes of the following public components as templates to create custom machine learning components.
    - Further Reference:
      - [Customize your machine learning components](https://docs.aws.amazon.com/greengrass/v2/developerguide/ml-customization.html)
  - AWS provided ML model components:
    - SageMaker Edge Manager
    - DLR image classification
    - DLR object detection
    - DLR image classification model store
    - DLR object detection model store
    - DLR installer
    - TensorFlow Lite image classification
    - TensorFlow Lite object detection
    - TensorFlow Lite image classification model store
    - TensorFlow Lite object detection model store
    - TensorFlow Lite installer

### AWS SageMaker Edge Manager agent
  - With SageMaker Edge Manager, you can use Amazon SageMaker Neo-compiled models directly on your core device.
  - [AWS SageMaker Edge Manager](/_posts/aws/ml/sagemaker/edge)



## Further References
  - [ROS2 Docker Sample Application with AWS IoT Greengrass 2.0](https://github.com/aws-samples/greengrass-v2-docker-ros-demo)
  
