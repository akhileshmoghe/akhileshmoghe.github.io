---
title:  "Azure Functions Overview"
date:   2022-04-12 12:33:22 +0530
permalink: /_posts/azure/azure-functions-overview
categories:
  - Cloud Computing
  - Azure
  - Serverless
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Cloud Computing
  - Azure
  - Serverless
author: Akhilesh Moghe
show_author_profile: true
---


- __*<u>Azure Functions</u>*__ for IoT enable developers to create scalable IoT applications that can be rapidly deployed without provisioning a fixed infrastructure capacity in advance.
- IoT solutions especially benefit from the ability to scale infrastructure using Azure Functions.
- Azure Functions is a part of the Azure serverless solution. Azure Serverless solution comprises of three elements:
  - Serverless Kubernetes
  - Serverless functions
  - Serverless application environments

## Function App
- Azure Functions are hosted in an execution context called a __function app__.
- You define function apps to *<u>logically group and structure your functions and a compute resource</u>* in Azure.
- There are a few decisions that need to be made to create the function app; you need to choose a service plan and select a compatible storage account.
- __*<u>Service plan</u>*__
  - Function apps may use one of two types of service plans:
  - __*Consumption plan*__:
    - When using the Azure serverless application platform, choose the Consumption plan.
    - This plan provides *<u>automatic scaling*</u>* and bills you only when your functions are running.
    - The Consumption plan comes with a *<u>configurable timeout period for executing a function</u>*. By default, it's __*<u>five (5) minutes</u>*__, but may be configured to have a timeout as long as __*10 minutes*__.
  - __*Azure App Service plan*__:
    - The Azure App Service plan enables you to *<u>avoid timeout periods</u>* by having your *<u>function run continuously on a VM that you define</u>*.
    - When using an App Service plan, *<u>you are responsible for managing the app resources the function runs on</u>*, so this is technically not a serverless plan.
    - It may be a better choice if your functions are used continuously, or if your functions require more processing power or longer execution time than the Consumption plan can provide.
- __*<u>Storage account</u>*__ requirements:
  - When you create *<u>a function app, it must be linked to a storage account</u>*.
  - The function app uses this storage account for internal operations, such as *<u>logging function executions</u>* and *<u>managing execution triggers</u>*.
  - On the Consumption plan, this is also where the *<u>function code and configuration file are stored</u>*.

## Triggers and Bindings in Azure Functions:
- An Azure function has two parts:
  - __*Triggers*__:
    - A function has a specific *<u>single trigger</u>*.
    - The trigger *<u>invokes the function</u>*.
    - Triggers *<u>provide data</u>* needed by the function.
    - Templates for Azure function triggers:
      - __HTTP Trigger__: When you want the code to execute in response to *<u>a request sent through the HTTP protocol</u>*.
      - __Timer Trigger__: When you want the code to execute according to a *<u>schedule</u>*.
      - __Blob Trigger__: When you want the code to execute when *<u>a new blob is added to an </u>*__*<u>Azure Storage account</u>*__.
      - __CosmosDB Trigger__: When you want the code to execute in response to *<u>new or updated documents in a NoSQL database</u>*.
      - __*Event Grid Trigger*__:	Starts a function when an event is received from Event Grid.
      - __*Queue Storage Trigger*__: Starts a function when *<u>a new item is received on a queue</u>*. The *<u>queue message is provided as input</u>* to the function.
      - __*Service Bus Trigger*__: Starts a function in response to messages from a Service Bus queue.
      - __*Microsoft Graph Events Trigger*__:	Starts a function in response to an *<u>incoming webhook from the Microsoft Graph</u>*. Each instance of this trigger can react to one Microsoft Graph resource type.
  - __*Bindings*__:
    - A trigger is a type of input binding that has the ability to initiate execution of some code.
    - Bindings connect Azure resources to the Azure functions.
    - Data from bindings is provided to the function as parameters.
    - Bindings interact with various data sources, which means *<u>you don't have to write the code in your function to connect to data sources and manage connections</u>*. The platform takes care of that complexity for you as part of the binding code.
    - Each binding has a direction, you could have input and output bindings for a function. Your code reads data from input bindings, and writes data to output bindings.
    - You can mix and match different bindings to suit your needs.
    - Bindings are optional and a function might have zero or multiple input and/or output bindings.
    - Triggers and bindings help you to avoid hardcoding access to other services.
  - Further details:
    - [Supported Bindings for various Azure services](https://docs.microsoft.com/en-us/azure/azure-functions/functions-triggers-bindings?tabs=csharp#supported-bindings)
    - [Azure Functions triggers and bindings concepts](https://docs.microsoft.com/en-us/azure/azure-functions/functions-triggers-bindings)
    - [Bindings code examples](https://docs.microsoft.com/en-us/azure/azure-functions/functions-triggers-bindings?tabs=python#bindings-code-examples)

## Example Scenario for Azure Functions for IoT
<object data="/assets/docs/azure/azure-functions/Azure_Functions_IoT_Usecase.pdf" type="application/pdf" width="850px" height="1000px">
  <embed src="/assets/docs/azure/azure-functions/Azure_Functions_IoT_Usecase.pdf">
      <p>This browser does not support PDFs. Please download the PDF to view it: <a href="/assets/docs/azure/azure-functions/Azure_Functions_IoT_Usecase.pdf">Download PDF</a>.</p>
  </embed>
</object>

## Considerations for Azure Function in IoT Solution
- Using the serverless computing approach, you can *<u>focus on the core logic</u>* of your solutions. You *<u>avoid the need to manage the underlying infrastructure</u>* that runs your solution. In the serverless application model, the cloud service provider automatically provisions, scales, and manages the underlying infrastructure required to run the code.
- The serverless model has two aspects: __*function as a service*__ and __*backend as a service*__.
  - Developers write the application as functions. Typically, these *<u>functions are </u>*__*<u>stateless</u>*__ and __*<u>short-lived</u>*__.
  - Azure functions enable you to __*<u>chain your functions</u>*__*<u> to create the complete solution</u>*. Hence, as a developer, you're concerned only with writing functions that interact with each other to solve the business problem.
  - Cloud service provider manages these deployed functions from the perspective of resources like processors, storage, and bandwidth (backend as a service). The resources are provided on an ‘as-needed’ basis and scaled as required.
  - You (the developer) are *<u>charged</u>* for the function *<u>only when the function is actually running</u>* (function as service).
- IoT applications suit many of these characteristics. In the case of IoT applications, you can create a larger application by amalgamating/chaining many functions and scaling them dynamically as needed.

### Merits of using Azure Functions for IoT solutions:
- Scalable applications where you're charged only for the resources you use.
- Your solution can *<u>scale up or down dynamically and instantly</u>* depending on the business requirements. Ddon't have to manage the infrastructure and to allocate the resources in advance.
- *<u>Faster time to market</u>*
- *<u>Flexibility to use multiple programming languages</u>*
- Internet of Things solutions are typically __*event driven*__ that is, you need to define a specific trigger that causes the function to run.
- If your IoT solution could potentially scale from a small number of devices to millions of devices – you should consider Azure Functions.
- Similarly, if your solution could see spikes of events to millions of events, you should consider Azure functions.

### Constraints of Azure Functions to consider:
- __*Execution time*__:
  - By default, *<u>functions have a timeout of </u>*__*<u>five (5) minutes</u>*__. This timeout is configurable to a __*<u>maximum of 10 minutes</u>*__.
  - If your function requires more than 10 minutes to execute, you can host it on a VM.
  - :x: Additionally, if your *<u>service is initiated through an HTTP request</u>* and you expect that value as an HTTP response, the timeout is further restricted to __*2.5 minutes*__.
  - :heavy_check_mark: But, there's also an option called [Durable Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable/) that enables you to *<u>orchestrate the executions of multiple functions without any timeout</u>*.
- __*Execution frequency*__:
  - If you expect your *<u>function to be executed continuously by multiple clients</u>*, it would be prudent/sensible to estimate the usage and calculate the cost of using functions accordingly. It *<u>might be cheaper to host your service on a VM</u>*.
  - :x: While scaling, __*<u>only one function app instance</u>*__ can be created __*<u>every 10 seconds</u>*__, for __*<u>up to 200 total instances</u>*__.
    - Keep in mind, *<u>each instance can service multiple concurrent executions</u>*, so there's no set limit about how much traffic a single instance can handle.
  - *<u>Different types of triggers have different scaling requirements</u>*, so research your choice of trigger and investigate its limits.


### Programming Language support with Azure Functions
- [Languages by runtime version](https://docs.microsoft.com/en-us/azure/azure-functions/supported-languages#languages-by-runtime-version)
  - Supported: Python, JavaScript, C#, Java, PowerShell, TypeScript
- [Azure Functions runtime constraints](https://docs.microsoft.com/en-us/azure/azure-functions/supported-languages#language-support-details)
  - Whether Azure function runtime is supported on __Linux__ or __Windows__.
  - Whether *<u>in-portal editing</u>* is supported for the runtime or not.
- [Custom Handlers](https://docs.microsoft.com/en-us/azure/azure-functions/supported-languages#custom-handlers)
  - Custom handlers are lightweight web servers that receive events from the Azure Functions host.
  - Any language that supports HTTP primitives can implement a custom handler.
  - :heavy_check_mark: This means that custom handlers can be used to create functions in languages that aren't officially supported.

## Best practice guidelines for Azure Functions
- __*Avoid long running functions*__:
  - Large, long-running functions can cause unexpected *<u>timeout issues</u>*.
- __*Write functions to be stateless*__:
  - Functions should be __*stateless*__ and __*idempotent*__ if possible.
  - Idempotence is the property of certain operations in mathematics and computer science whereby they can be applied multiple times without changing the result beyond the initial application. For example, the number 1 is idempotent with respect to the multiplication operation because 1 x 1 = 1.
  - Associate any required state information with your data.
  - It's possible to achieve __*cross function communication*__ using [Durable Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable/) and [Azure Logic Apps](####) that *<u>manage state transitions</u>* and *<u>communication between multiple functions</u>*.
- __*Write defensive functions*__:
  - Design your functions with the *<u>ability to continue from a previous fail point during the next execution</u>*.
- __*Scalability best practices*__:
  - Share and manage connections
  - Avoid sharing storage accounts
  - Manage function memory usage


## Durable Functions:
- Durable Functions is an extension of __*Azure Functions*__ that lets you write *<u>stateful functions</u>* in a serverless compute environment.
- The extension lets you define stateful workflows by writing [orchestrator functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-orchestrations?tabs=python) and stateful entities by writing [entity functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-entities?tabs=python) using the Azure Functions programming model. Behind the scenes, the extension manages state, checkpoints, and restarts for you, allowing you to focus on your business logic.

### Durable Orchestration Functions
- Durable Functions is an extension of Azure Functions. You can use an orchestrator function *<u>to orchestrate the execution of other Durable functions</u>* within a function app. Orchestrator functions have the following characteristics:
  - Orchestrator functions define function workflows using *<u>procedural code</u>*. No declarative schemas or designers are needed.
  - Orchestrator functions can *<u>call other durable functions </u>*__*<u>synchronously and asynchronously</u>*__. Output from called functions can be reliably saved to local variables.
  - Orchestrator functions are durable and reliable. *<u>Execution progress is automatically checkpointed when the function "awaits" or "yields"</u>*. *<u>Local state is never lost when the process recycles or the VM reboots</u>*.
  - Orchestrator functions can be *<u>long-running</u>*. The total lifespan of an orchestration instance can be *<u>seconds, days, months, or never-ending</u>*.
  
### Entity Functions
- Entity functions define operations for *<u>reading and updating small pieces of state</u>*, known as __*durable entities*__.
- Like orchestrator functions, entity functions are functions with *<u>a special trigger type</u>*, the __*entity trigger*__.
- Unlike orchestrator functions, entity functions *<u>manage the state of an entity explicitly</u>*, rather than implicitly representing state via control flow.
- Entities provide a means for scaling out applications by distributing the work across many entities, each with a modestly sized state.



