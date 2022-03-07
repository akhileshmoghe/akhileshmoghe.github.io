---
title:  "AWS Sagemaker Machine Learning Service, Sagemaker Neo, Sagemaker Edge Manager"
date:   2021-11-11 12:33:22 +0530
permalink: /_posts/aws/ml/sagemaker/edge
categories:
  - On-Premise Cloud
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
  - On-Premise Cloud
  - Cloud
  - AWS
  - AWS IoT
  - Edge IoT
  - IoT
  - Machine Learning
author: Akhilesh Moghe
show_author_profile: true
---

This write-up will only focus on AWS Sagemaker ML service with respect to ML model deployment to Edge devices and Cloud.

## AWS Sagemaker Neo
- Amazon SageMaker Neo enables developers to *<u>optimize machine learning (ML) models</u>* for inference on SageMaker on the *cloud* and *supported devices at the edge* for the specific underlying hardware.
- Optimizes machine learning models for inference on cloud instances and edge devices to run faster with no loss in accuracy.
- Amazon SageMaker __*<u>Neo runtime</u>*__ is supported on *<u>Android</u>*, *<u>iOS</u>*, *<u>Linux</u>*, and *<u>Windows</u>* operating systems.
- Sagemaker neo can optimize the ML model to run on *<u>target hardware platform</u>* of Edge devices based on processors from *<u>Ambarella</u>*, *<u>Apple</u>*, *<u>ARM</u>*, *<u>Intel</u>*, *<u>MediaTek</u>*, *<u>Nvidia</u>*, *<u>NXP</u>*, *<u>Qualcomm</u>*, *<u>RockChip</u>*, *<u>Texas Instruments</u>*, or *<u>Xilinx</u>*.
- Compiles it into an *<u>executable</u>*.
- For inference in the cloud, SageMaker Neo speeds up inference and saves cost by creating __*<u>an inference optimized container</u>*__ that include *<u>MXNet</u>*, *<u>PyTorch</u>*, and *<u>TensorFlow</u>* integrated with Neo runtime for SageMaker hosting.
- Amazon SageMaker Neo supports optimization for a model from the framework-specific format of *<u>DarkNet</u>*, *<u>Keras</u>*, *<u>MXNet</u>*, *<u>PyTorch</u>*, *<u>TensorFlow</u>*, *<u>TensorFlow-Lite</u>*, *<u>ONNX</u>*, or *<u>XGBoost</u>*.
- Amazon SageMaker *<u>Neo runtime</u>* occupies 1MB of storage and 2MB of memory, which is many times smaller than the storage and memory footprint of a framework, while providing a simple common *<u>API to run a compiled model</u>* originating in any framework.
- Amazon SageMaker Neo takes advantage of partner-provided <u>accelerator libraries</u> to deliver the best available performance for a deep learning model on heterogeneous hardware platforms with a <u>hardware accelerator</u> as well as a CPU. Acceleration libraries such as *<u>Ambarella CV Tools</u>*, *<u>Nvidia Tensor RT</u>*, and *<u>Texas Instruments TIDL</u>* each support a specific set of functions and operators. SageMaker Neo automatically partitions your model so that the part with operators supported by the accelerator can run on the accelerator while the rest of the model runs on the CPU.
- Amazon SageMaker Neo now compiles models for Amazon SageMaker __*<u>INF1 instance</u>*__ targets. SageMaker hosting provides a managed service for inference on the INF1 instances, which are based on the __*<u>AWS Inferentia chip</u>*__.

## AWS Sagemaker Edge Manager
- AWS Sagemaker Edge Manager consists of a *<u>Service running in AWS cloud</u>* and an *<u>Agent running on Edge devices</u>*.
- Sagemaker Edge Manager *<u>deploys a ML model optimized with SageMaker Neo</u>* automatically so you don’t need to have Neo runtime installed on your devices in order to take advantage of the model optimizations.

### *<u>Agent</u>*
- Use the agent to *<u>make predictions</u>* with models loaded onto your edge devices.
- The agent also *<u>collects model metrics</u>* and *<u>captures data</u>* at specific intervals.
- Sample data is stored in your __*<u>Amazon S3</u>*__ bucket.
- 2 methods of installing and deploying the Edge Manager agent onto your edge devices:
  - Download the *<u>agent as a binary</u>* from the Amazon S3 release bucket.
  - [Deploy aws.greengrass.SageMakerEdgeManager to __*<u>AWS IoT Greengrass V2</u>*](https://docs.aws.amazon.com/greengrass/v2/developerguide/use-sagemaker-edge-manager.html)
    - Use AWS Greengrass v2 Console
    - or Use AWS CLI

### *<u>Monitoring deployments across fleets</u>*
- SageMaker Edge Manager also *<u>collects prediction data and sends</u>* a sample of the data to the cloud for *<u>monitoring, labeling, and retraining</u>*.
- All data can be viewed in the __*<u>SageMaker Edge Manager dashboard</u>*__ which reports on the operation of deployed models.
- The dashboard is useful to *<u>understand the performance of models running on each device</u>* across your fleet, understand overall *<u>fleet health</u>* and *<u>identify problematic model</u>*s and particular devices.
- If quality declines are detected, you can quickly spot them in the dashboard and also *<u>configure alerts</u>* through __*<u>Amazon CloudWatch</u>*__.

### *<u>Signed and Verifiable ML deployments</u>*
- SageMaker Edge Manager also *<u>cryptographically signs your models</u>* so you can verify that it was not tampered with as it moves from the cloud to edge devices.

### *<u>Integration with device applications</u>*
- SageMaker Edge Manager *<u>supports</u>* [gRPC](https://grpc.io/), an open source *<u>remote procedure call</u>*, which allows you to *<u>integrate SageMaker Edge Manager with your existing edge applications through APIs</u>* in common programming languages, such as Android Java, C# / .NET, Dart, Go, Java, Kotlin/JVM, Node.js, Objective-C, PHP, Python, Ruby, and Web.
- *<u>Manages models separately</u>* from the rest of the application, so that updates to the model and the application are independent.

### *<u>Multiple ML models serve on edge devices</u>* [*upcoming feature*]
- ML applications usually require hosting and running multiple models concurrently on a device.
- SageMaker Edge Manager will soon allow you to write *simple application logic* to send one or more queries (i.e. load/unload models, run inference) *<u>independently to multiple models</u>* and *<u>rebalance hardware resource utilization</u>* when you add or update a model.

### *<u>Model Registry and Lifecycle</u>* [*upcoming feature*]
- SageMaker Edge Manager will soon be able to automate the build-train-deploy workflow from cloud to edge devices in Amazon SageMaker Edge Manager, and trace the lifecycle of each model.

## AWS Outpost and AWS Sagemaker Edge Manager
  - [Machine Learning at the Edge with AWS Outposts and Amazon SageMaker](https://aws.amazon.com/blogs/machine-learning/machine-learning-at-the-edge-with-aws-outposts-and-amazon-sagemaker/)
  ![aws-outpost-sagemaker-edge-manager](/assets/images/aws/aws-outpost-sagemaker-edge-manager.png)

###### Reference
- [Amazon SageMaker Neo](https://aws.amazon.com/sagemaker/neo/)
- [Sagemaker Edge Manager YouTube](https://www.youtube.com/watch?v=zS0Q3bdsLiU&t=3s&ab_channel=AmazonWebServices)
- [Developer Guide: SageMaker Edge Manager](https://docs.aws.amazon.com/sagemaker/latest/dg/edge.html)
- [Developer Guide: Edge Manager Agent](https://docs.aws.amazon.com/sagemaker/latest/dg/edge-device-fleet-about.html)
- [SageMaker Edge Manager Example](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker_edge_manager/sagemaker_edge_example/sagemaker_edge_example.html?highlight=edge)



