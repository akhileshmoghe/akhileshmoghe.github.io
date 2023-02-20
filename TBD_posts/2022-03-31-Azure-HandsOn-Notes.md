---
title:  "Hands On with Azure Services"
date:   2022-03-31 12:33:22 +0530
permalink: /_posts/azure/azure-services-hands-on-notes
categories:
  - Azure IoT
  - IoT
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Azure IoT
  - IoT
author: Akhilesh Moghe
show_author_profile: true
---

## Creating and Deploying Azure IoT Edge Device
### Install Azure IoT extension
- You need to add the Azure IoT extension to the Cloud Shell instance Azure CLI.

{% highlight shell linenos %}

  $ az extension add --name azure-iot

{% endhighlight %}

### Create a resource group
- Create a resource group called "IoTEdgeResources" using the following command:

{% highlight shell linenos %}

  $ az group create --name IoTEdgeResources --location eastus2

{% endhighlight %}

### Create an IoT hub
- The following code creates a free F1 tier hub in the resource group "IoTEdgeResources". Replace {hub_name} with a unique name for your IoT Hub.

{% highlight shell linenos %}

  $ az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 --partition-count 2
  $

{% endhighlight %}

### Register an IoT Edge device - Create a device identity
- IoT Edge devices behave and can be managed differently compared to typical IoT devices, declare this identity to be for an IoT Edge device with the --edge-enabled flag.

{% highlight shell linenos %}

  $ az iot hub device-identity create --hub-name {hub_name} --device-id myEdgeDevice --edge-enabled
  $

{% endhighlight %}

### Retrieve the connection string
- To retrieve the connection string for your device, which links your physical device with its identity in IoT Hub, use this command:
- The resulting output should be similar to this:
  
  `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

{% highlight shell linenos %}

  $ az iot hub device-identity connection-string show --device-id myEdgeDevice --hub-name {hub_name} --output table
  $

{% endhighlight %}

### Deploy the IoT Edge device
- I tried, Asymmetric TLS certificates way for device identity, but it did not get succeed and device could not connect to IoT Hub, got error `403: Access denied`.
- So, reverted back to Symmetric key way of connection.
- Also, IoT Edge v1.2 did not run successfully, and I had to try v1.1 which got successfully connected to IoT Hub.
- Follow this link to install and provision IoT Edge device:
  - [Create and provision an IoT Edge device on Linux using symmetric keys](https://docs.microsoft.com/en-in/azure/iot-edge/how-to-provision-single-device-linux-symmetric?view=iotedge-2018-06&preserve-view=true&tabs=azure-portal%2Cubuntu)

### Check if the IoT Edge device is configured
- To verify that the IoT Edge security daemon is running as a system service, we use `iotedge` commands.

{% highlight shell linenos %}

  $ sudo systemctl status iotedge

{% endhighlight %}

- If you need to troubleshoot the service, retrieve the service logs.

{% highlight shell linenos %}

  $ journalctl -u iotedge

{% endhighlight %}

- View all the modules running on your IoT Edge device.

{% highlight shell linenos %}

  $ sudo iotedge list

{% endhighlight %}

- View the messages being sent from the temperature sensor module using the command:

{% highlight shell linenos %}

  $ sudo iotedge logs SimulatedTemperatureSensor -f

{% endhighlight %}

### Exercise - Deploy a pre-built module to the IoT Edge
- [Exercise - Deploy a pre-built module to the IoT Edge](https://docs.microsoft.com/en-us/learn/modules/deploy-prebuilt-module-edge-device/6-exercise-deploying-prebuilt-module)


## Creating and deploying Azure machine learning module
### Specify parameters
{% highlight shell linenos %}
  # Enter the resource group in Azure where you want to provision the resources 
  resource_group_name="AI-Edge-Engineer-LP"

  # Enter Azure region where your services will be provisioned, for example "eastus2"
  azure_region="eastus2"

  # Enter your Azure IoT Hub name 
  # If you don't have an IoT Hub, pick a name to make a new one 
  iot_hub_name="AI-Edge-Engineer-LP-IoT-Hub"

  # Enter your IoT Edge device ID 
  # If you don't have an IoT Edge device registered, pick a name to create a new one 
  # This is NOT the name of your VM, but it's just an entry in your IoT Hub, so you can pick any name
  iot_device_id="AI-Edge-Engineer-LP-Edge-Device"

  # Provide your Azure subscription ID to provision your services 
  subscription_id="21838765-8682-403a-b6ec-3c5663b07df2"

  # Provide your Azure ML service workspace name 
  # If you don't have a workspace, pick a name to create a new one
  aml_workspace_name="AI-Edge-Engineer-LP-AML-workspace"

  # DO NOT CHANGE THIS VALUE for this tutorial
  # This is the name of the AML module you deploy to the device
  module_name = "machinelearningmodule"

{% endhighlight %}

### Load Azure IoT Extension
{% highlight shell linenos %}
  # Load the IoT extension for Azure CLI
  $ az extension add --name azure-iot
{% endhighlight %}

### Login to Azure
{% highlight shell linenos %}
  $ az login
  
  To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code HJAYXHXQX to authenticate.
  After Successful Authentication, you will get below, similar response on terminal:
  [
    {
      "cloudName": "AzureCloud",
      "homeTenantId": "1f4beacd-b7aa-49b2-aaa1-b8525cb257e0",
      "id": "21838765-8682-403a-b6ec-3c5663b07df2",
      "isDefault": true,
      "managedByTenants": [],
      "name": "Free Trial",
      "state": "Enabled",
      "tenantId": "1f4beacd-b7aa-49b2-aaa1-b8525cb257e0",
      "user": {
        "name": "akhilesh_moghe@persistent.com",
        "type": "user"
      }
    }
  ]
  
{% endhighlight %}

### Set the Azure Subscription
{% highlight shell linenos %}
  $ az account set --subscription $subscription_id
{% endhighlight %}

### Install the Azure Machine Learning library
{% highlight shell linenos %}
  $ sudo pip3 install --upgrade azureml-sdk

  -- This step may take longer time, than usual.
{% endhighlight %}

### Check core SDK version number
{% highlight python linenos %}
  # Check core SDK version number
  import azureml.core
  from azureml.core import Workspace

  print("SDK version:", azureml.core.VERSION)
  ------------
  >>> SDK version: 1.40.0
  ------------
  
{% endhighlight %}

### Create the Machine learning Workspace
- The [*<u>workspace</u>*](https://docs.microsoft.com/en-us/azure/machine-learning/concept-workspace) is the top-level resource for Azure Machine Learning, providing a centralized place to work with all the artifacts you create when you use Azure Machine Learning. The workspace keeps a history of all training runs, including logs, metrics, output, and a snapshot of your scripts. You use this information to determine which training run produces the best model.

{% highlight python linenos %}
  # Create ML WorkSpace 
  ws = Workspace.create(subscription_id = subscription_id,
                        resource_group = resource_group_name,
                        name = aml_workspace_name,
                        location = azure_region)

  ws.write_config()
  
{% endhighlight %}

### Install the Libraries
{% highlight shell linenos %}
  $ pip3 install pandas
  $ pip3 install sklearn
{% endhighlight %}

### Load and read data set
- :x:  Yet to learn, [Panda](https://pandas.pydata.org/), [Sklearn](https://scikit-learn.org/stable/index.html), [numpy](https://numpy.org/), [pickle](https://docs.python.org/3/library/pickle.html) libraries, their usage...
- For Reference: [temperature_data.csv](/assets/docs/azure/azure-ml/temperature_data.csv)

{% highlight python linenos %}
  # Load and read data set
  import pandas
  import numpy
  import pickle

  from sklearn import tree
  from sklearn.model_selection import train_test_split
  
  temp_data = pandas.read_csv('temperature_data.csv')
  temp_data
  
{% endhighlight %}

### Load features and labels
{% highlight python linenos %}
  # Load features and labels
  X, Y = temp_data[['machine_temperature', 'machine_pressure', 'ambient_temperature', 'ambient_humidity']].values, temp_data['anomaly'].values

{% endhighlight %}

### Train The Model
- :x:  Yet to learn these ML, Data Science concepts like: LogisticRegression, DecisionTreeClassifier, etc.
{% highlight python linenos %}
  # Split data 65%-35% into training set and test set
  X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.35, random_state=0)

  # Change regularization rate and you will likely get a different accuracy.
  reg = 0.01

  # Train a decision tree on the training set
  #clf1 = LogisticRegression(C=1/reg).fit(X_train, Y_train)
  clf1 = tree.DecisionTreeClassifier()
  clf1 = clf1.fit(X_train, Y_train)
  print (clf1)

{% endhighlight %}

### Evaluate the accuracy
- :x:  Yet to learn these ML, Data Science concepts
{% highlight python linenos %}
  # Evaluate the test set
  accuracy = clf1.score(X_test, Y_test)

  print ("Accuracy is {}".format(accuracy))
  
{% endhighlight %}

### Serializing and testing the model
- For Reference: [model.pkl](/assets/docs/azure/azure-ml/model.pkl)

{% highlight python linenos %}
  # Serialize the model and write to disk
  f = open('model.pkl', 'wb')
  pickle.dump(clf1, f)
  f.close()
  print ("Exported the model to model.pkl")
  
  # Test the model by importing it and providing a sample data point
  print("Import the model from model.pkl")
  f2 = open('model.pkl', 'rb')
  clf2 = pickle.load(f2)

  # Normal (not an anomaly)
  #X_new = [[24.90294136, 1.44463889, 20.89537849, 24]]
  #X_new = [[33.40859853, 2.413637808, 20.89162813, 26]]
  #X_new = [[34.42109181, 2.528985143, 21.23903786, 25]]

  # Anomaly
  X_new = [[33.66995566, 2.44341267, 21.39450979, 26]]
  #X_new = [[105.5457931, 10.63179922, 20.62029994, 26]]

  print ('New sample: {}'.format(X_new))

  pred = clf2.predict(X_new)
  print('Predicted class is {}'.format(pred))
  
{% endhighlight %}

### Register Model to cloud
- The model registry lets you *<u>keep track of all the models</u>* in your Azure Machine Learning workspace.
- Models are identified by *<u>name</u>* and *<u>version</u>*.
- Each time you register a model with the same name as an existing one, the registry assumes that it's a new version. The version is incremented, and the new model is registered under the same name.
- When you register the model, you can provide *<u>additional metadata tags</u>* and then use the tags when you search for models.
- You can't delete a registered model that is being used by an active deployment.

{% highlight python linenos %}
  #Register Model to cloud
  from azureml.core.model import Model

  model = Model.register(model_path = "model.pkl",
                         model_name = "model.pkl",
                         tags = {'area': "anomaly", 'type': "classification"},
                         description = "Sample anomaly detection model for IOT tutorial",
                         workspace = ws)
  
  #print model 
  print(model.name, model.description, model.version, sep = '\t')
  
{% endhighlight %}

### iot_score.py for Inferencing
- The script contains two functions that load and run the model:
  - __*init()*__: Typically, this function *<u>loads the model into a global object</u>*. This function is *<u>run only once</u>* when the Docker container for your web service is started.
  - __*run(input_data)*__: This function *<u>uses the model to predict a value</u>* based on the input data.
    - *<u>Inputs and outputs</u>* of the run typically use *<u>JSON for serialization and deserialization</u>*.
    - You can also work with *<u>raw binary data</u>*. You can transform the data before sending it to the model or before returning it to the client.

{% highlight python linenos %}
  # This script generates the scoring file
  # with the init and run functions needed to 
  # operationalize the anomaly detection sample

  import pickle
  import json
  import pandas
  import joblib
  from sklearn.linear_model import Ridge
  from azureml.core.model import Model

  def init():
    global model
    # this is a different behavior than before when the code is run locally, even though the code is the same.
    model_path = Model.get_model_path('model.pkl')
    # deserialize the model file back into a sklearn model
    model = joblib.load(model_path)

  # note you can pass in multiple rows for scoring
  def run(input_str):
    try:
      input_json = json.loads(input_str)
      input_df = pandas.DataFrame([[input_json['machine']['temperature'],input_json['machine']['pressure'],input_json['ambient']['temperature'],input_json['ambient']['humidity']]])
      pred = model.predict(input_df)
      print("Prediction is ", pred[0])
    except Exception as e:
      result = str(e)
        
    if pred[0] == 1:
      input_json['anomaly']=True
    else:
      input_json['anomaly']=False
        
    return [json.dumps(input_json)]
      
{% endhighlight %}

### Loading Azure ML workspace
{% highlight python linenos %}
  # Initialize a workspace object from persisted configuration
  from azureml.core import Workspace

  ws = Workspace.from_config()
  print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')

{% endhighlight %}

### Create docker Image
- For Reference: [myenv.yml](/assets/docs/azure/azure-ml/myenv.yml)
- [iot_score.py](/assets/docs/azure/azure-ml/iot_score.py)
- This section creates a Docker Container image and pushes it to [Azure Container Registry](https://azure.microsoft.com/en-in/services/container-registry/)

{% highlight python linenos %}
  # Create docker Image This specifies the dependencies to include in the environment
  from azureml.core.conda_dependencies import CondaDependencies 

  myenv = CondaDependencies.create(
    conda_packages=['pandas', 'scikit-learn', 'numpy', 'pip=20.1.1'],
    pip_packages=['azureml-sdk'])

  with open("myenv.yml","w") as f:
    f.write(myenv.serialize_to_string())

  from azureml.core.image import Image, ContainerImage

  image_config = ContainerImage.image_configuration(runtime= "python",
                                                    execution_script="iot_score.py",
                                                    conda_file="myenv.yml",
                                                    tags = {'area': "iot", 'type': "classification"},
                                                    description = "IOT Edge anomaly detection demo")


  image = Image.create(name = "tempanomalydetection",
                      # this is the model object 
                      models = [model],
                      image_config = image_config, 
                      workspace = ws)

  image.wait_for_creation(show_output = True)
  
  for i in Image.list(workspace = ws,tags = ["area"]):
    print('{}(v.{} [{}]) stored at {} with build log {}'.format(i.name, i.version, i.creation_state, i.image_location, i.image_build_log_uri))
  
{% endhighlight %}

### Testing the model on Azure container Instance
- This section creates an [Azure Container Instance](https://azure.microsoft.com/en-in/services/container-instances/)
- It defines th configuration of the instance as: cpu_cores = 1, memory_gb = 1
- Then it deploys the container as a web-service.

{% highlight python linenos %}
  #test Model on Azure Container

  from azureml.core.webservice import AciWebservice

  aciconfig = AciWebservice.deploy_configuration(cpu_cores = 1, 
                                                 memory_gb = 1, 
                                                 tags = {'area': "iot", 'type': "classification"}, 
                                                 description = 'IOT Edge anomaly detection demo')
  
  from azureml.core.webservice import AciWebservice

  aci_service_name = 'tempsensor-iotedge-ml-1'
  print(aci_service_name)
  aci_service = Webservice.deploy_from_image(deployment_config = aciconfig,
                                             image = image,
                                             name = aci_service_name,
                                             workspace = ws)
  aci_service.wait_for_deployment(True)
  print(aci_service.state)
  
{% endhighlight %}

### Testing web service
{% highlight python linenos %}
  #testing Web Service 

  import json

  # Anomaly
  #test_sample = json.dumps({ "machine": { "temperature": 33.66995566, "pressure": 2.44341267 }, \
  #                          "ambient": { "temperature": 21.39450979, "humidity": 26 },\
  #                          "timeCreated": "2017-10-27T18:14:02.4911177Z" })

  # Normal
  test_sample = json.dumps({ "machine": { "temperature": 31.16469009, "pressure": 2.158002669 }, \
                            "ambient": { "temperature": 21.17794693, "humidity": 25 },\
                            "timeCreated": "2017-10-27T18:14:02.4911177Z" })

  test_sample = bytes(test_sample,encoding = 'utf8')

  prediction = aci_service.run(input_data = test_sample)
  print(prediction)
  
{% endhighlight %}

### Getting container details
{% highlight python linenos %}
  # Getting your container details
  container_reg = ws.get_details()["containerRegistry"]
  reg_name=container_reg.split("/")[-1]
  container_url = "\"" + image.image_location + "\","
  subscription_id = ws.subscription_id
  print('{}'.format(image.image_location))
  print('{}'.format(reg_name))
  print('{}'.format(subscription_id))
  from azure.mgmt.containerregistry import ContainerRegistryManagementClient
  from azure.mgmt import containerregistry
  client = ContainerRegistryManagementClient(ws._auth,subscription_id)
  result= client.registries.list_credentials(resource_group_name, reg_name, custom_headers=None, raw=False)
  username = result.username
  password = result.passwords[0].value

{% endhighlight %}

### Deploying container to the Edge device
- For Reference: [iot-workshop-deployment-template.json](/assets/docs/azure/azure-ml/iot-workshop-deployment-template.json)

{% highlight python linenos %}
  #Deploying Container to Edge 

  file = open('iot-workshop-deployment-template.json')
  contents = file.read()
  contents = contents.replace('__MODULE_NAME', module_name)
  contents = contents.replace('__REGISTRY_NAME', reg_name)
  contents = contents.replace('__REGISTRY_USER_NAME', username)
  contents = contents.replace('__REGISTRY_PASSWORD', password)
  contents = contents.replace('__REGISTRY_IMAGE_LOCATION', image.image_location)
  with open('./deployment.json', 'wt', encoding='utf-8') as output_file:
    output_file.write(contents)
  
{% endhighlight %}

### Push the deployment JSON to the Azure IoT Hub

{% highlight shell linenos %}

  # Push the deployment JSON to the IOT Hub
  $ az iot edge set-modules --device-id $iot_device_id --hub-name $iot_hub_name --content deployment.json
  $

{% endhighlight %}


