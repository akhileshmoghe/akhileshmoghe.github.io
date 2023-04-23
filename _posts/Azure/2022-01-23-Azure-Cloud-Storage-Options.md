---
title:  "Azure Cloud Storage options for IoT scenarios"
date:   2022-01-23 12:33:22 +0530
permalink: /_posts/azure/storage/azure-cloud-storage-options-for-iot-scenarios
categories:
  - Cloud Computing
  - Azure
  - IoT
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Cloud Computing
  - Azure
  - IoT
author: Akhilesh Moghe
show_author_profile: true
sidebar:
 nav: IoT
---

## Data Storage requirements overview in IoT scenarios:
- IoT analysis is all about getting insights from raw telemetry data.
- One of the reasons to store telemetry data is to enable the training of machine learning models. Typically, these models require a vast amount of data to work on, before meaningful insights are made.
- It's common to use the familiar terms __*hot*__, __*warm*__, __*cool*__, and __*cold*__ in data analysis.
  - *<u>Hot</u>* clearly means a *<u>real-time</u>* approach is needed.
  - *<u>Warm</u>* can have the same meaning, though perhaps the data is *<u>near-real-time</u>*, or *<u>recent</u>*.
  - *<u>Cool</u>* means the *<u>flow of data is slow</u>*.
  - *<u>Cold</u>* means that the *<u>data is stored and not flowing</u>*. The cooler the path, the more the data can be batched.
- In short, in IoT scenarios, some data is continuously Streaming that needs to be acted upon immediately, whereas some data is being uploaded in batch manner.
- One of the easiest ways of handling this data duality from the device, is to send data at different frequencies.
  - The first kind of messages contain only the telemetry data that needs analyzed in real time.
  - The second type of messages, sent at a lower frequency, contain a batch of the telemetry data, and the other metadata that might be needed, for deeper analysis or archiving.
  - The IoT Hub routes these two messages to different resources.
- *<u>Hot Data Path</u>*:
  - The IoT device sends *<u>specific telemetry data</u>* in its own message, which is routed by the IoT Hub for instant analysis and visualization say, using [*<u>Azure Time Series Insights</u>*](https://azure.microsoft.com/en-in/services/time-series-insights/#documentation).
  - Alternatively, the analysis could be handled by [*<u>Azure Stream Analytics</u>*](https://azure.microsoft.com/en-in/services/stream-analytics/#documentation), which supports simple *<u>SQL language queries</u>*, and is *<u>extensible</u>* via __C#__ or __JavaScript__ <u>UDFs (User-defined functions)</u>.
  - The hot path requires storage optimized for data availability.
- *<u>Cold Data Path</u>*:
  - The IoT device sends out *<u>batched telemetry</u>*, and *<u>logging data</u>*.
  - The IoT Hub directs these messages down a route to an [*<u>Azure storage account</u>*](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-overview).
  - Cold path storage is *<u>optimized for size</u>* (that is, it's compressed), *<u>long-term storage</u>*, and *<u>low cost</u>*.
  - The cold path is *<u>not optimized for availability</u>*.
  - If data consists of *<u>files, images, recordings</u>*, and similar disparate items, then it's considered *<u>unstructured storage</u>*.
  - If data neatly divides into similar *<u>database-like objects</u>*, then it's considered *<u>structured</u>*.

  ---

## Azure Cloud Storage options for IoT scenarios:
### *<u>Blob Storage</u>* (Unstructured Data):
- Unstructured Data: Blob Storage: (default storage in Azure Storage account)
- It's referred to as unstructured storage, that means each entry in the storage doesn't conform to any particular model.
- For example, one entry might be *<u>video</u>*, another an *<u>audio recording</u>*, a third a group of *<u>text files</u>*, and so on.
- Blob storage is *<u>similar to the files and folder's structure</u>* you're used to on your laptop or desktop computer.
- Blob storage, __*by default*__, has a *<u>general-purpose setting</u>* applied. Whatever data you route to the account is stored with *<u>reasonable access settings</u>*.
- Blob storage can be *<u>accessed via API calls</u>*. The APIs are available via __*REST*__ calls, __*Azure PowerShell*__, or the __*Azure CLI*__. Client libraries are available for .NET, Java, Python, Node.js, and other languages.
- Blob storage is easily accessible, secure, and low-cost.
- 3 Roles for Blobs:
  - __*<u>Block blob</u>*__:
    - When you have a *<u>large volume of data</u>*, it can be more efficient to access that data if it's divided into blocks.
    - *<u>Each block has a unique ID</u>*. You have access to this ID.
    - Access IDs can use it to *<u>read from, and write to, a specific block</u>*.
    - Block blobs can be *<u>written to in parallel</u>*, and can be *<u>uploaded in any order</u>*.
    - Basically, block blobs are for handling large amounts of data over a network.
  - __*<u>Page blob</u>*__:
    - Page blobs are there for data that needs *<u>frequent read/write access</u>*.
    - Consider a page blob to be like a remote hard disk. For any *<u>data that is a work-in-progress</u>*, a page blob is the ideal cloud storage.
    - __*High performance*__, and __*low latency*__, are the key assets of page blobs.
  - __*<u>Append blob</u>*__:
    - An append blob, as its name implies, *<u>can only be appended to</u>*, and is ideal for __*log files*__. A log file is *<u>never edited</u>*, and just grows and grows! There's plenty of space in the cloud.
- __*Data Security*__:
  - Azure Blob storage is *<u>automatically </u>*__*<u>encrypted</u>*__, *<u>no extra cost, without extra setup</u>*.
  - The system used is called [*<u>Storage Service Encryption (SSE)</u>*](https://docs.microsoft.com/en-us/azure/storage/common/storage-service-encryption).
  - Data can be *<u>secured in-transit</u>*, between an app and Azure, using *<u>Client-Side Encryption</u>*, *<u>HTTPS</u>*, or *<u>SMB 3.0</u>*.

  ---

### *<u>Data Lake Storage</u>* (Mass Unstructured to be backed up data):
- Unstructured Data: [*<u>Data Lake Storage</u>*](https://docs.microsoft.com/en-in/azure/storage/blobs/data-lake-storage-introduction):
- Upgrade data storage from Azure Blob to Azure Data Lake storage comes when you have an *<u>enormous amount of data</u>*.
- To help *<u>organize data</u>*, a concept called [*<u>hierarchical namespaces</u>*](https://docs.microsoft.com/en-in/azure/storage/blobs/data-lake-storage-namespace) is available in a Data Lake.
  - A hierarchical namespace can be used to *<u>encapsulate a collection, large or small, of data objects and files</u>*.
  - It basically *<u>adds another level of reference</u>*, that is *<u>used to make access to the data more efficient</u>*.
- __*Security*__ in Azure Data Lake is *<u>on the file, or folder, level, or greater granularity</u>* if needed. All the security, and API access, features of Blob storage apply to Data Lake storage.
- Finally, [*<u>Data Lake analytics</u>*](https://azure.microsoft.com/en-in/services/data-lake-analytics/#documentation), available through __*REST APIs*__, are *<u>optimized for big data</u>*. Your queries should still run in a decent amount of time, even if they're trawling through a sea of data.
- Blob storage is your go to solution for cloud IoT storage. Upgrade to Data Lake Storage if data organization, security, or analytics performance, become an issue with your Blob storage.

  ---

### *<u>Cosmos DB</u>* (Structured Data):
- Structured Data: [*<u>Cosmos DB</u>*](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction):
- An example of structured storage would be a *<u>large database</u>*, each entry in the database containing similar information, and each entry *<u>accessible by a set of similar API</u>* calls.
- Cosmos DB resource is a *<u>well-structured storage</u>*. At the lowest level, a *<u>Cosmos DB consists of </u>*__*JSON objects*__.
- *<u>Access to the data</u>* in a Cosmos DB resource is made through *<u>queries</u>* built from API calls. Cosmos DB supports a range of __*APIs*__, including __*SQL API*__, __*Mongo API*__, __*Gremlin (graph) API*__, __*Azure Table API*__, and the __*Cassandra API*__.

#### *<u>Data Consistency</u>*:
- We can create *<u>multiple replicase across the regions for Cosmos DB</u>*. e.g., one region writes and other regions just reads the data. *<u>Data replication and consistency of the data is handled by Cosmos DB</u>*. There are following types of data consistency:
- __*Strong Consistency*__:
  - In this case, the read operation will fetch same results every time irrespective of the fact that data is written in one region and others are just reading it.
  - In this case, the *<u>system waits for an acknowledgment from all locations that they've received the update, before giving the all clear to make the data readable</u>*.
  - This process ensures *<u>worldwide consistency</u>*, but comes at the cost of *<u>all locations having to wait</u>* for the slowest to receive the update. This __*latency*__ may only be seconds, but the latency exists so should be considered.
  - In Strong consistency, *<u>every location will get identical data on every read</u>*.
- __*Eventual Consistency*__:
  - In this scenario, *<u>each location gets the update when it arrives</u>*. This process clearly means some locations might have stale data for a short while, before the local data is updated.
  - Note that, *<u>updates can arrive out of order</u>*.
- __*Bounded Consistency*__:
  - With Bounded consistency, you set a __*time threshold*__, or __*version update count threshold*__.
  - This *<u>threshold is the tolerance of each location for stale data</u>*. If a location reads data, only to find the data is outside of the threshold, then the *<u>system will wait until a value is available that is within the threshold</u>*. For example, if a threshold is set at 20 seconds, then only data that is stale by 20 seconds or less, is acceptable.
  - Set this threshold to zero, and you have Strong consistency.
- __*Session Consistency*__: (default)
  - In this scenario, the *<u>write location has immediate access to the updated data</u>*.
  - The *<u>read locations get the data in the right order</u>*, but there will be a *<u>different latency for each read location</u>*.
- __*Prefix Consistency*__:
  - With this setting, all locations receive the updates in the *<u>correct order</u>*, with *<u>no update being skipped over</u>*.
  - The Session consistency level, uses this Prefix consistency for all read locations.

#### *<u>Use cases for Cosmos DB</u>*:
- A Cosmos DB resource is usually a *<u>more expensive option than Blob storage</u>*.
- Create a Cosmos DB resource when you have *<u>a mass of well-structured, time critical data</u>*.
- The case for a Cosmos DB is stronger still, if the *<u>data needs to be available in several locations across the globe</u>*.

  ---

### *<u>Time Series Insights</u>* Service:
- A key feature of *<u>telemetry</u>* is that it's *<u>time-stamped</u>*: *<u>time and order are critical</u>* elements of the data.
- Built-in [*<u>Azure Time Series Insights</u>*](https://azure.microsoft.com/en-in/services/time-series-insights/#documentation) service is for time-based analytics.
- This service both enables you to *<u>visualize your data without writing any code</u>*, and enables you to perform *<u>regular expression-based analytics</u>*.
- The routing, visualization, and analytics, can all be done via the Azure portal. For *<u>simpler analytics</u>*, the *<u>UI</u>* can be used to create the expressions. For *<u>deeper analysis</u>*, and for integration with programming languages, there's a __*REST-based API*__.
- Check out the out-of-the-box abilities and features of Time Series Insights, before engaging in more costly options.
- Azure Time Series Insights is a *<u>managed service</u>* that enables you to *<u>store, visualize, and query a large amount of time series data</u>*.
- *<u>Features</u>*:
  - *<u>Parsing JSON from messages and structures</u>* in clean rows and columns.
  - Join metadata with IoT device-generated data.
  - Manages *<u>events data storage</u>*:
    - With a __*column-store database*__
    - and both warm and cold storage.
  - *<u>Query</u>* your data directly in the *<u>Time Series Insights Explorer</u>*.
  - Use __*APIs*__ that to *<u>embed time series data into custom applications</u>*.


