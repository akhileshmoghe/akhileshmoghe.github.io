---
title:  "AWS IoT Greengrass & AWS Sagemaker for ML deployment & inference on Edge devices"
date:   2021-11-12 12:33:22 +0530
permalink: /_posts/aws/ml/iot/sagemaker-greengrass-v1-v2-ml-inference
categories:
  - Cloud
  - AWS
  - AWS IoT
  - Edge IoT
  - IoT
  - Machine Learning
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Cloud
  - AWS
  - AWS IoT
  - Edge IoT
  - IoT
  - Machine Learning
author: Akhilesh Moghe
show_author_profile: true
---

- This write-up will only focus on AWS Sagemaker & AWS Greengrass Services with respect to ML model deployment to Edge devices.
- We will explore different ways to deploy the ML model to edge devices using Greengrass v1 & v2 and also using Sagemaker Edge Manager Agent.

## ML Inference on Greengrass devices
- ML models can be trained using __*<u>AWS Sagemaker</u>*__ or custom ML trainining ways like [KubeFlow](https://www.kubeflow.org/). Models are stored in __*<u>AWS S3</u>*__ for deployment to Greengrass devices.
- ML models are deployed as *<u>artifacts</u>* in your components to perform inference on your core devices.

### *<u>ML Components</u>*
- AWS provides following Machine Learning components that can be deployed to edge devices to perform Machine Learning Inference.
- ML models can be trained using __*<u>AWS Sagemaker</u>*__ or custom ML trainining ways like [KubeFlow](https://www.kubeflow.org/). Models are stored in __*<u>AWS S3</u>*__ for deployment to Greengrass devices.
- AWS-provided machine learning components are broadly categorized as follows:
  - __*Model component*__ — Contains machine learning models as *<u>Greengrass artifacts</u>*.
  - __*Runtime component*__ — Contains the *<u>script</u>* that installs the machine learning framework and its dependencies on the Greengrass core device.
  - __*Inference component*__ — Contains the *<u>inference code</u>* and includes *<u>component dependencies</u>* to install the machine learning framework and download pre-trained machine learning models.
- To perform custom machine learning inference with *<u>your own models</u>* that are stored in __*<u>Amazon S3</u>*__, or to use a *<u>different machine learning framework</u>*, you can use the __*recipes*__ of the following public components as templates to create custom machine learning components.
  - Further Reference:
    - [Customize your machine learning components](https://docs.aws.amazon.com/greengrass/v2/developerguide/ml-customization.html)
- [*<u>AWS provided ML model components</u>*](https://docs.aws.amazon.com/greengrass/v2/developerguide/machine-learning-components.html):
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

### *<u>AWS SageMaker Edge Manager agent</u>*
- With SageMaker Edge Manager, you can use Amazon SageMaker Neo-compiled models directly on your core device.
- [AWS SageMaker Edge Manager](/_posts/aws/ml/sagemaker/edge)

## Perform machine learning inference with Greengrass v1
- Supported ML model sources:
  - AWS Sagemaker 
  - AWS S3
- Requires AWS Greengrass v1.6+
- Following ML runtimes and libraries are supported with AWS IoT Greengrass v1:
  - Amazon SageMaker Neo deep learning runtime
  - Apache MXNet
  - TensorFlow
- [Access machine learning resources from Lambda functions](https://docs.aws.amazon.com/greengrass/v1/developerguide/access-ml-resources.html)
  - User-defined Lambda functions can access machine learning resources to run local inference on the AWS IoT Greengrass core.
  - Checkout Access permissions required for machine learning resources
- [Configure machine learning inference using the AWS Management Console](https://docs.aws.amazon.com/greengrass/v1/developerguide/ml-console.html)
- [Configure optimized machine learning inference using the AWS Management Console](https://docs.aws.amazon.com/greengrass/v1/developerguide/ml-dlc-console.html)
  - You can use the [SageMaker Neo](/_posts/aws/ml/sagemaker/edge#aws-sagemaker-neo) *<u>deep learning compiler</u>* to optimize the prediction efficiency of native machine learning inference models in Tensorflow, Apache MXNet, PyTorch, ONNX, and XGBoost frameworks for a smaller footprint and faster performance.


## Perform machine learning inference with Greengrass v2
- [Greengrass v2: Tutorial: Perform sample image classification inference using TensorFlow Lite](https://docs.aws.amazon.com/greengrass/v2/developerguide/ml-tutorial-image-classification.html)
  - This tutorial shows you how to use the TensorFlow Lite image classification inference component to perform sample image classification inference on a Greengrass core device.
- [Greengrass v2: Perform sample image classification inference on images from a camera using TensorFlow Lite](https://docs.aws.amazon.com/greengrass/v2/developerguide/ml-tutorial-image-classification-camera.html)
  - This tutorial shows you how to use the TensorFlow Lite image classification inference component to perform sample image classification inference on images from a camera locally on a Greengrass core device.

## Further References
- [ROS2 Docker Sample Application with AWS IoT Greengrass 2.0](https://github.com/aws-samples/greengrass-v2-docker-ros-demo)
- [Deploy and Manage ROS Robots with AWS IoT Greengrass 2.0 and Docker](https://aws.amazon.com/blogs/robotics/deploy-and-manage-ros-robots-with-aws-iot-greengrass-2-0-and-docker/)



  
