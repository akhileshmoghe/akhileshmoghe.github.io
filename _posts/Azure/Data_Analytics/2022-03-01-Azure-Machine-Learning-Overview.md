---
title:  "Azure Machine Learning Overview"
date:   2022-03-01 12:33:22 +0530
permalink: /_posts/azure/data-analytics/azure-machine-learning
categories:
  - Cloud Computing
  - Azure
  - Edge Computing
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Cloud Computing
  - Azure
  - Edge Computing
author: Akhilesh Moghe
show_author_profile: true
---

- Azure machine learning represents a suite of products to *<u>build, train, and deploy machine learning models</u>* in Azure. Data and analytics solutions from Azure represent a set of solutions to *<u>store and analyze IoT data</u>*.
  - [*<u>Azure Stream Analytics</u>*](https://azure.microsoft.com/en-in/services/stream-analytics/)
  - [*<u>Azure HDInsight</u>*](https://azure.microsoft.com/en-in/services/hdinsight/)
  - [*<u>Azure Data Lake</u>*](https://azure.microsoft.com/en-in/solutions/data-lake/)
  - [*<u>Azure Cosmos DB</u>*](https://azure.microsoft.com/en-us/services/cosmos-db/) and
  - [*<u>Azure Data Fabric</u>*](https://azure.microsoft.com/en-in/services/data-factory/)
- You can also build your system using [*<u>Azure functions</u>*](https://azure.microsoft.com/en-in/services/functions/) to enable a *<u>serverless application</u>*. Azure Functions enables you to run small pieces of code, or "functions," in the cloud. These functions can then be combined to create a more comprehensive application. You pay only for the time your code runs (serverless application).

## Azure ML model management
- Building the machine learning is only the first step towards deploying it in production on edge devices. The workflow for deploying models production is:

  ### *<u>Register the model</u>*
  - A registered model is a logical grouping for one or more files that make up your model.

  ### *<u>Prepare to deploy</u>* (Specify assets, usage, compute target.)
  - The model is then packaged into a Docker image.
  - To deploy the model, you need the following items:
  - __*<u>An entry script</u>*__: This script accepts requests, scores the requests by using the model and returns the results. The entry script is specific to your model.
    - The entry script receives data submitted to a deployed web service and passes it to the model. It then takes the response returned by the model and returns that to the client.
    - The script contains two functions that load and run the model:
      - __*init()*__: Typically, this function *<u>loads the model into a global object</u>*. This function is *<u>run only once</u>* when the Docker container for your web service is started.
      - __*run(input_data)*__: This function *<u>uses the model to predict a value</u>* based on the input data.
        - *<u>Inputs and outputs</u>* of the run typically use *<u>JSON for serialization and deserialization</u>*.
        - You can also work with *<u>raw binary data</u>*. You can transform the data before sending it to the model or before returning it to the client.
  - __*<u>Dependencies</u>*__, like helper *<u>scripts</u>* or *<u>Python/Conda packages</u>* required to run the entry script or model.
  - The __*<u>deployment configuration</u>*__ for the *<u>compute target</u>* that hosts the deployed model. This configuration describes things like *<u>memory and CPU requirements</u>* needed to run the model.
    - These items are encapsulated into an __*inference configuration*__ and a __*deployment configuration*__. The inference configuration references the entry script and other dependencies.
    - You define these configurations programmatically when you use the SDK to perform the deployment. You define them in JSON files when you use the CLI.

  ### *<u>Deploy the model to the compute target</u>*
  - Deploy the model as a web service in the cloud or locally.
  - Some additional steps may be needed during deployment include profiling to determine the *<u>ideal CPU and memory</u>* settings or *<u>model conversion to optimize performance</u>*.
  - You can use the following compute targets/resources to host your web service deployment.
    - Local web service
    - Azure Machine Learning compute instance web service
    - Azure Kubernetes Service (AKS)
    - [Azure Container Instances](https://azure.microsoft.com/en-in/services/container-instances/)
      - For Azure Container Instance, the *<u>azureml.core.image.ContainerImage class</u>* is used to create an image configuration. The image configuration is then used to create a new Docker image.
      - To deploy the image you created, you first need to specify the target you want to use. You then use the *<u>AciWebservice class</u>* to configure and deploy from an image.
    - Azure Machine Learning compute clusters
    - Azure Functions
    - Azure IoT Edge
    - Azure Data Box Edge

  ### *<u>Test the deployed model as a web service</u>*
  - You create a __*deployment.json*__ file that contains the modules you want to deploy to the device and the routes.
  - You should then push this file to the IoT Hub, which will then send it to the IoT Edge device.
  - The IoT Edge agent will then pull the Docker images and run/test them.
  - At this point, you should be able to monitor/test messages from your edge device to your IoT Hub.



