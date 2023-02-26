---
title:  "Pravega: A Distributed Streaming, Messaging System"
permalink: /_post/streaming_data/pravega_distributed_streaming_messaging_system
date:   2021-12-14 1:33:22 +0530
categories:
  - Pravega
  - Streaming Data
  - IoT
  - Edge Computing
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Pravega
  - Streaming Data
  - IoT
  - Edge Computing
author: Akhilesh Moghe
show_author_profile: true
---

- Pravega is a __*Distributed messaging systems*__ such as __*Kafka*__ and __*Pulsar*__ which provide modern *<u>Pub/Sub infrastructure</u>* suitable for *<u>data-intensive applications</u>*.
- __*Pravega streams*__ are *<u>durable</u>*, *<u>consistent</u>*, and *<u>elastic</u>*, while natively supporting *<u>long-term data retention</u>*.
- Pravega enhances the range of supported applications by efficiently handling both *<u>small events as in IoT</u>* and *<u>larger data as in </u>*__*<u>videos for computer vision/video analytics</u>*__.
- Pravega also enables *<u>replicating application state</u>* and *<u>storing key-value pairs</u>*.

## Features
- __*Distributed Messaging System*__
  - Supports messaging between processes (IPC like mechanism)
  - Supports *<u>Leader Election</u>*, to select the master node in a cluster of machines.
- __*Pub/Sub infrastructure*__
- __*Streams*__
  - Pravega Streams are [*<u>Durable</u>*](https://youtu.be/1fVYVxclU3U), meaning that Pravega creates *<u>3 Replicas of a data</u>*, on *<u>3 discs</u>* and it acknowledges the write-client only after it has written to all 3 dicsc. It's *<u>NOT an in-memory replication</u>*, but on disc, and then acknowledgement.
  - Consistent
  - Elastic
  - Append-only
  - Event Streams
    - *<u>Event is delivered & processed </u>*[*<u>Exactly Once</u>*](https://youtu.be/KPk1TsQDb5U). Every event gets an *<u>ID</u>* and an *<u>Acknowledgement</u>* is sent back to client. *<u>Same event sent by client is rejected</u>* by Pravega system.
- [*<u>Long term data retention</u>*](https://youtu.be/Cx_tTtlzyDo)
  - Pravega provides Data Storage mechanism, that *<u>Retain data streams forever</u>*.
  - Data storage supports *<u>Real-time & Historical Data use-cases</u>*
  - Pravega supports both *<u>File</u>* or *<u>Object store</u>*
    - __*HDFS*__
    - __*NFS*__
    - __*Dell ECS S3*__
- __*Data Types variations*__
  - Supports *<u>IoT Data</u>* handling (Small Events)
  - Also, *<u>Video/Audio</u>* (Large Data)
- __*Replication of Application State, State Synchronizer*__
  - Used for coordinating processes in a distributed computing environment.
  - Uses a Pravega Stream to provide a synchronization mechanism for state shared between multiple processes running in a cluster.
  - Used to maintain a *<u>single, shared copy of an application's configuration property</u>* across all instances.
- Storing __*Key-Value Pairs*__
- [*<u>Auto Scaling</u>*](https://youtu.be/yhUWNiHeB8I)
  - Supported for every Individual data Streams, As per data ingestion rate
  - Multi-tenancy supported
  - Supports High Partitions count
- __*Fault Tolerant*__
  - Provega guarantees events *<u>Ordering</u>*, even in-case of client/server/network failure.
- Milliseconds of __*Write frequency*__ is supported by Pravega, which is ideal and many time a requirement for IoT sensors streams & Time Sensitive Applications.
  - 1000s of clients Read/Write supported.
  - Pravega sents a *<u>Write acknowledgement</u>* to clients, *<u>only after data is stored/persisted & protected</u>*.
  - [*<u>Tiered Storage</u>*](https://youtu.be/NhmDEerda-k)
    - *<u>Tier-1 storage</u>* is [*<u>write efficient</u>*](https://youtu.be/oeQTN-OkZBE) storage, data is ingested first here. It's a *<u>Log Data Structure</u>* implemented with Open-source __*Apache BookKeeper*__ Project. Tier-1 storage is typically implemented on: *<u>faster SSDs</u>*, *<u>non-volatile RAM</u>*.
      - [Log Data Structure](https://scaling.dev/storage/log#:~:text=Log%20is%20the%20simplest%20data,end%20of%20the%20log%20file.&text=Log%20file%20is%20always%20written,thread%20only%20appends%20new%20records.): Log is the simplest data structure that represents append-only sequence of records. Log records are immutable and appended in strictly sequential order to the end of the log file.
      - Log is a fundamental data structure used for write-intensive applications.
    - *<u>Tier-2 storage</u>* is __*throughput efficient*__, uses spinning discs. It's implemented with __*HDFS*__ model.
    - Pravega *<u>asynchronously migrates Events from Tier-1 to Tier-2</u>* to reflect the different access patterns to Stream data.
- __*Data Pipelines*__
  - Real-time for data processing
  - Batching processing
- [*<u>Transactions</u>*](https://youtu.be/KMdrLPQ7JnQ)
  - With Pravega Transactions, either *<u>Complete Data is written to streams</u>*, Or *<u>Complete Data is trashed</u>*, and the transaction is aborted.
  - Useful when a Writer can "batch" up a bunch of Events and commit them as a unit into a Stream.
  - Transactions need to be created explicitly, when writing events.
  - *<u>Transaction Segments</u>*:
    - Transactions have their own segments exactly similar (one-to-one matching) to *<u>Stream segments</u>*.
    - Events are first published to Transaction segments and the writer-client is acknowledged.
    - On commit, all of the Transaction Segments are written to Stream Segments.
    - *<u>Events published into a Transaction are visible to the Reader only after the Transaction is committed</u>*.
    - On error, all Transaction segments and events in them are discarded.


## Pravega Events
- In Pravega, Data being Written/Read to Streams are all Events.
- Java Serializer, deserializer is used to make sense/parse of events.
- __*Routing Key*__
  - Used to group similar events, e.g., consumer-id, machine-id, sensor-id
  - Routing Keys are important in defining the read and write semantics that Pravega guarantees.
  - When an Event is written into a Stream, it is *<u>stored in one of the Stream Segments based on the Event's Routing Key</u>*.
  - Segments consists of a range of routing keys, e.g., like Postal ZIP codes for various orders.
  - *<u>Routing Keys are hashed to form a "key space"</u>*.
- __*Ordering Guarantee*__
  - *<u>Events with the same Routing Key are consumed in the order they were written</u>*.
  - If an Event has been acknowledged to its Writer or has been read by a Reader, it is guaranteed that it will continue to exist in the same location or position for all subsequent reads until it is deleted.


## Pravega Streams
- __*Scaling*__
  - Streams are *<u>NOT Statically Partitioned</u>*.
  - Streams are *<>Unbounded</u>*, i.e., *<u>No limit on number of events/bytes/size</u>*.
  - __*Segments*__
    - Every stream is made of segments.
    - Number of segments in a stream varies over a lifetime of the stream.
    - Number of segments in a stream depends upon volume of data ingestion.
    - *<u>Segments of the same stream can be placed on different machines/nodes</u>*, __*Horizontal Scaling*__.
    - *<u>Segments can be merged back when volume of data is low</u>*.
    - Stream Segments are configurable.
    - An Event written to a Stream is written to any single Stream Segment.
    - __*Segment Store Service*__
      - It buffers the incoming data in a very fast and durable append-only medium (__*Tier-1*__)
      - It syncs the data to a high-throughput (but not necessarily low latency) system (__*Tier-2*__) in the background.
      - It supports *<u>arbitrary-offset reads</u>*.
      - Stream Segments are unlimited in length.
      - Supports *<u>Multiple concurrent writers to the same Stream Segment</u>*.
      - *<u>Order is guaranteed within the context of a single Writer</u>*.
      - Appends from multiple concurrent Writers will be added in the order in which they were received.
      - It supports Writing to and reading from a Stream Segment concurrently.
    - __*Scaling Policies*__:
      - Scaling Policies can be configured in following 3 types:
      - __*Fixed*__:
        - The *<u>number of Stream Segments does not vary with load</u>*.
      - __*Data-based*__:
        - It *<u>splits a Stream Segment</u>* into multiple ones (i.e., *<u>Scale-up Event</u>*) if the __*<u>number of bytes per second</u>*__*<u> written to that Stream Segment increases beyond a defined threshold</u>*.
        - Similarly, Pravega *<u>merges two adjacent Stream Segments</u>* (i.e., *<u>Scale-down Event</u>*) if the number of bytes written to them fall below a defined threshold.
        - Note that, even if the load for a Stream Segment reaches the defined threshold. Pravega does not immediately trigger a Scale-up/down Event. Instead, the load should be satisfying the scaling policy threshold for a sufficient amount of time.
      - __*Event-based*__:
        - *<u>Uses number of events</u>* instead of bytes as threashold, otherwise it's similar to Data-based policy.
- __*Append-only Log data structure*__
  - With this, Pravega only supports *<u>write to front (~tail)</u>*.
  - But, *<u>Read from any point</u>*, developers have control over the Reader's start position in the Stream.
  - Supports *<u>Read/Write in parallel</u>*.
- __*Stream Scopes*__
  - Pravega supports *<u>Stream organization</u>*, equivalent to Namespaces.
  - *<u>Stream Name MUST be Unique within a Scope</u>*.
  - It is used to *<u>classify streams by tenants, departments, geo locations</u>*.

## Deployment
- [Pravega Deployment Overview](https://github.com/pravega/pravega/blob/master/documentation/src/docs/deployment/deployment.md)
  - Deployment modes:
    - Standalone
    - Distributed
  - Local/testing Deployment
    - [Local Standalone Mode](https://github.com/pravega/pravega/blob/master/documentation/src/docs/deployment/run-local.md#from-installation-package)
    - [Docker Compose, Distributed mode](https://github.com/pravega/pravega/blob/master/documentation/src/docs/deployment/run-local.md#docker-compose-distributed-mode)
  - Production Deployment
    - [Manual Installation](https://github.com/pravega/pravega/blob/master/documentation/src/docs/deployment/manual-install.md)
    - [Kubernetes Operators](https://github.com/pravega/pravega/blob/master/documentation/src/docs/admin-guide/operators.md)
    - [Docker Swarm](https://github.com/pravega/pravega/blob/master/documentation/src/docs/deployment/docker-swarm.md)
    - [DC/OS](https://github.com/pravega/pravega/blob/master/documentation/src/docs/deployment/dcos-install.md)
    - [Cloud (AWS)](https://github.com/pravega/pravega/blob/master/documentation/src/docs/deployment/aws-install.md)
- Configuration and Provisioning Guide for production clusters.
  - [Configuration and Provisioning](https://github.com/pravega/pravega/blob/master/documentation/src/docs/admin-guide/cluster-dependencies.md)

## Pravega Data Flow Solution Overview
![Pravega Data Flow Solution Overview](/assets/images/streaming_data/pravega/Data-Flow-from-Sensors-to-the-Edge-and-the-Cloud-using-Pravega-Overview-of-the-Solution-1-1024x690.png){:.shadow}
- [Complete Blog: Data Flow from Sensors to the Edge and the Cloud using Pravega](https://pravega.io/?p=1353&preview=true)

## Apache Flink connectors for Pravega
- The Flink Connector enables building end-to-end *<u>stream processing pipelines</u>* with Pravega in [Apache Flink](https://flink.apache.org/). This also allows reading and writing data to external data sources and sinks via Flink Connector.

- __*Introduction to Pravega Flink Connector*__:
  <iframe width="665" height="374" src="https://www.youtube.com/embed/4Qk2i6XKeh0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

- [Apache Flink connectors for Pravega](https://github.com/pravega/flink-connectors)
- [Flink Connector Examples for Pravega](https://github.com/pravega/pravega-samples/tree/master/flink-connector-examples)
- [Pravega Flink Tools](https://github.com/pravega/flink-tools)

### Usecase: Real-Time Object Detection with Pravega and Flink
  <iframe width="665" height="374" src="https://www.youtube.com/embed/BzjEX3Sh2C8?list=PL_tj-4JhM0ekiqwDU972KzJr31SbPL9Zh" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Apache Spark Connector:
- Connector to read and write Pravega Streams with [Apache Spark](http://spark.apache.org/), a high-performance analytics engine for batch and streaming data.
- The connector can be used to build end-to-end *<u>stream processing pipelines</u>* that use Pravega as the stream storage and message bus, and Apache Spark for computation over the streams.
- [Spark Connector](https://github.com/pravega/spark-connectors)

## Hadoop Connector:
- Implements both the input and the output format interfaces for [Apache Hadoop](https://hadoop.apache.org/).
- It leverages Pravega batch client to read existing events in parallel; and uses write API to write events to Pravega streams.
- [Hadoop Connector](https://github.com/pravega/hadoop-connectors)

## Presto Connector:
- [Presto](https://prestodb.io/) is a distributed SQL query engine for big data.
- Presto uses connectors to query storage from different storage sources. This connector allows Presto to query storage from Pravega streams.
- [Presto Connector](https://github.com/pravega/presto-connector)

## Pravega: Rethinking Storage for Streams
<object data="/assets/docs/streaming_data/pravega/Pravega-Rethinking-Storage-for-Streams.pdf" type="application/pdf" width="900px" height="570px">
  <embed src="/assets/docs/streaming_data/pravega/Pravega-Rethinking-Storage-for-Streams.pdf">
      <p>This browser does not support PDFs. Please download the PDF to view it: <a href="/assets/docs/streaming_data/pravega/Pravega-Rethinking-Storage-for-Streams.pdf">Download PDF</a>.</p>
  </embed>
</object>

  ---

## Dell Streaming Data Platform (SDP)
- An Enterprise solution to address a wide range of use cases to reunite the Operational Technology (OT) world and the IT world with following key features:
  - *<u>Ingestion of data</u>*
    - IoT sensor data
    - Video data
    - log files
    - High Frequency Data
  - *<u>Data Collection</u>*
    - at the edge or
    - at the core data center.
  - *<u>Analyze data</u>*
    - in real-time
    - in batch
    - generate alert based on that specific data set.
  - *<u>Data Sync</u>*
    - Synchronize data from the edge to the core data center
    - for analysis or ML model development (training).

### Pravega Usage in SDP
![Pravega Usage in SDP](/assets/images/streaming_data/pravega/Pravega_in_SDP.png)

  ---

### Complete Dell SDP Architecture:
<object data="/assets/docs/streaming_data/pravega/streaming-data-platform-architecture.pdf" type="application/pdf" width="900px" height="1000px">
  <embed src="/assets/docs/streaming_data/pravega/streaming-data-platform-architecture.pdf">
      <p>This browser does not support PDFs. Please download the PDF to view it: <a href="/assets/docs/streaming_data/pravega/streaming-data-platform-architecture.pdf">Download PDF</a>.</p>
  </embed>
</object>

  ---

### [*<u>SDP Code Hub</u>*](https://streamingdataplatform.github.io/code-hub/index.html)
- An essential collection of Code sample, Demos, and Connectors To kisck-start your projects on the DELL EMC Streaming Data Platform.
- [SDP Code Hub: Sample Code](https://streamingdataplatform.github.io/code-hub/catalog.html)
- [SDP Code Hub: Connectors](https://streamingdataplatform.github.io/code-hub/connectors.html)
- [SDP Code Hub: Demos](https://streamingdataplatform.github.io/code-hub/demos.html)
- [StreamingDataPlatform/code-hub](https://github.com/StreamingDataPlatform/code-hub)

###### References:
- [Pravega vs Kafka vs Pulsar](https://cncf.pravega.io/)
- [Pravega Deployment Overview](https://github.com/pravega/pravega/blob/master/documentation/src/docs/deployment/deployment.md)
- [Data Flow from Sensors to the Edge and the Cloud using Pravega](https://pravega.io/?p=1353&preview=true)
- [Predictive Maintenance for IoT](https://cncf.pravega.io/predictive-maintenance-for-iot/)
- [Exactly-Once Processing Using Apache Flink and Pravega Connector](https://cncf.pravega.io/blog/2019/06/10/exactly-once-processing-using-apache-flink-and-pravega-connector/)
- [Sample Applications for Pravega](https://github.com/pravega/pravega-samples)



