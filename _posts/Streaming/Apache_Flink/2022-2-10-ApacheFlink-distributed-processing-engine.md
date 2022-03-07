---
title:  "Apache Flink: A Distributed, Stream Processing Framework"
permalink: /_post/streaming_data/apache_flink_distributed_stream_processing_framework
date:   2022-2-10 1:33:22 +0530
categories:
  - Apache Flink
  - Streming Data
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Apache Flink
  - Streming Data
author: Akhilesh Moghe
show_author_profile: true
---

- Apache Flink is a framework and __*<u>distributed</u>*__*<u> processing engine</u>* for __*<u>stateful</u>*__*<u> computations</u>* over *<u>unbounded and bounded</u>* data streams.
- Flink has been designed to run in all common cluster environments, perform *<u>computations at in-memory speed</u>* and at any scale.
- Flink easily *<u>maintains application state</u>*. Its asynchronous and incremental checkpointing algorithm ensures *<u>minimal impact on processing latencies</u>*.
- Flink *<u>guarantees exactly-once state consistency</u>* in case of failures by *<u>periodically and asynchronously checkpointing the local state to durable storage</u>*.
- Flink always maintains the *<u>task state in memory</u>* or, if the state size exceeds the available memory, in access-efficient on-disk data structures, in [RocksDB](https://rocksdb.org/), an efficient embedded on-disk data store.

## Flink Integration and Deployment
- Flink integrates with all common *<u>cluster resource managers</u>* such as [Hadoop YARN](https://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/YARN.html), [Apache Mesos](https://mesos.apache.org/), and [Kubernetes](https://kubernetes.io/) but *<u>can also be setup to run as a stand-alone cluster</u>*.
- Flink automatically *<u>identifies the required resources based on the application’s configured </u>*[*<u>parallelism</u>*](https://nightlies.apache.org/flink/flink-docs-release-1.14/docs/dev/datastream/execution/parallel/) and requests them from the resource manager. In case of a failure, Flink *<u>replaces the failed container by requesting new resources</u>*.
- All communication to submit or control an application happens via *<u>REST calls</u>*. This eases the integration of Flink in many environments.

## Flink Features useful for Stream Processing Applications
### *<u>Stream Types</u>*:
- __*<u>Unbounded streams</u>*__
  - Unbounded streams have a start but no defined end. They do not terminate, but keep providing data as it is generated.
  - Unbounded streams must be *<u>continuously processed</u>*, i.e., events must be promptly handled after they have been ingested. It is not possible to wait for all input data to arrive because the input is unbounded and will not be complete at any point in time.
  - Processing unbounded data often requires that events are ingested in a *<u>specific order</u>*, such as the order in which events occurred, to be able to reason about result completeness.
- __*<u>Bounded streams</u>*__
  - Bounded streams have a defined start and end.
  - Bounded streams can be processed by *<u>ingesting all data before performing any computations</u>*.
  - *<u>Ordered ingestion is not required</u>* to process bounded streams because a bounded data set can always be sorted.
  - Processing of bounded streams is also known as __*batch processing*__.
  ![Bounded and Unbonded Streams](/assets/images/streaming_data/apache_flink/bounded-unbounded.png)

### [*<u>Application State</u>*](https://nightlies.apache.org/flink/flink-docs-release-1.14/docs/dev/datastream/fault-tolerance/state/):
- __*Multiple State Primitives*__: Flink provides state primitives for different data structures, such as *<u>atomic values</u>*, *<u>lists</u>*, or *<u>maps</u>*. Developers can *<u>choose the state primitive that is most efficient based on the access pattern of the function</u>*.
- __*Pluggable State Backends*__: Application state is managed in and checkpointed by a pluggable state backend. Flink features different state backends that *<u>store state </u>*__*<u>in memory</u>*__ or in [RocksDB](https://rocksdb.org/), an efficient embedded *<u>on-disk data store</u>*. Custom state backends can be plugged in as well.
- __*Exactly-once state consistency*__: Flink’s checkpointing and recovery algorithms guarantee the consistency of application state in case of a failure. Hence, failures are transparently handled and do not affect the correctness of an application.
- __*Very Large State*__: Flink is able to maintain application state of *<u>several terabytes in size</u>* due to its asynchronous and incremental checkpoint algorithm.
- __*Scalable Applications*__: Flink supports scaling of stateful applications by redistributing the state to more or fewer workers.

### [*<u>Time Sensitivity</u>*](https://nightlies.apache.org/flink/flink-docs-release-1.14/docs/concepts/time/):
- __*Event-time Mode*__: Applications that process streams with __*event-time semantics*__ *<u>compute results based on </u>*__*<u>timestamps</u>*__*<u> of the events</u>*. Thereby, event-time processing allows for accurate and consistent results regardless whether recorded or real-time events are processed.
- [*Watermark Support*](https://nightlies.apache.org/flink/flink-docs-release-1.14/docs/dev/datastream/event-time/generating_watermarks/#introduction-to-watermark-strategies): Flink employs watermarks to reason about time in event-time applications. Watermarks are also a flexible *<u>mechanism to trade-off the latency and completeness of results</u>*.
- [*Late Data Handling*](https://nightlies.apache.org/flink/flink-docs-release-1.14/docs/dev/datastream/operators/windows/#allowed-lateness): When processing streams in event-time mode with watermarks, it can happen that a <u>computation has been completed before all associated events have arrived</u>. Such events are called *<u>late events</u>*. Flink features multiple options to handle late events, such as *<u>rerouting them via </u>*[*<u>side outputs</u>*](https://nightlies.apache.org/flink/flink-docs-release-1.14/docs/dev/datastream/side_output/)*<u> and updating previously completed results</u>*.
- [*Processing-time Mode*](https://nightlies.apache.org/flink/flink-docs-release-1.14/docs/dev/datastream/operators/process_function/): In addition to its event-time mode, Flink also supports __*processing-time semantics*__ which *<u>performs computations as triggered by the wall-clock time of the processing machine</u>*. The processing-time mode can be suitable for certain applications with __*strict low-latency requirements*__ that can tolerate approximate results.
- [*Windowing*](https://nightlies.apache.org/flink/flink-docs-release-1.14/docs/dev/datastream/operators/windows/): A useful mechanism to *<u>deal with out-of-order data</u>* is windowing—a concept that can be thought of as *<u>grouping elements of an infinite stream of data into finite sets</u>* for further (and easier) processing, based on dimensions like event time.
  - Further Reference: [Introducing Stream Windows in Apache Flink](https://flink.apache.org/news/2015/12/04/Introducing-windows.html)

### *<u>Layered APIs</u>*:
- Flink provides three layered APIs. Each API *<u>offers a different trade-off between conciseness and expressiveness</u>* and targets different use cases.
  ![api-stack](/assets/images/streaming_data/apache_flink/api-stack.png)

### *<u>Libraries</u>*:
- Flink features several libraries for common data processing use cases. The <u>libraries are typically embedded in an API and not fully self-contained</u>. Hence, they can benefit from all features of the API and be integrated with other libraries.
- [*<u>Complex Event Processing (CEP)</u>*](https://nightlies.apache.org/flink/flink-docs-stable/dev/libs/cep.html):
  - Pattern detection is a very common use case for event stream processing. Flink’s CEP library provides an API to specify patterns of events (think of regular expressions or state machines).
  - The CEP library is *<u>integrated with Flink’s DataStream API</u>*, such that patterns are evaluated on DataStreams.
  - Applications for the CEP library include *<u>network intrusion detection</u>*, *<u>business process monitoring</u>*, and *<u>fraud detection</u>*.
- [*<u>DataSet API</u>*](https://nightlies.apache.org/flink/flink-docs-stable/dev/batch/index.html):
  - The DataSet API is Flink’s __*core API*__ for *<u>batch processing applications</u>*.
  - The primitives of the DataSet API include __*map*__, __*reduce*__, (outer) __*join*__, __*co-group*__, and __*iterate*__.
  - All operations are backed by algorithms and data structures that operate on serialized data in memory and spill to disk if the data size exceed the memory budget.
  - The data processing algorithms of Flink’s DataSet API are inspired by traditional database operators, such as hybrid hash-join or external merge-sort.
- [*<u>Gelly</u>*](https://nightlies.apache.org/flink/flink-docs-stable/dev/libs/gelly/index.html):
  - Gelly is a library for scalable *<u>graph processing and analysis</u>*.
  - Gelly is implemented on top of and integrated with the DataSet API. Hence, it benefits from its scalable and robust operators.
  - Gelly features built-in algorithms, such as __*label propagation*__, __*triangle enumeration*__, and __*page rank*__, but provides also a *<u>Graph API that eases the implementation of custom graph algorithms</u>*.

### [*<u>Application Consistency & Recovery</u>*](https://nightlies.apache.org/flink/flink-docs-release-1.14/docs/dev/datastream/fault-tolerance/checkpointing/):
- __*Consistent Checkpoints*__: Flink’s recovery mechanism is based on consistent checkpoints of an application’s state. In case of a failure, the application is restarted and its state is loaded from the latest checkpoint. In combination with resettable stream sources, this feature can guarantee exactly-once state consistency.
- __*Efficient Checkpoints*__: Checkpointing the state of an application can be quite expensive if the application maintains terabytes of state. Flink’s can perform asynchronous and incremental checkpoints, in order to keep the impact of checkpoints on the application’s latency SLAs very small.
- __*High-Availability Setup*__: Flink feature a high-availability mode that eliminates all single-points-of-failure. The HA-mode is based on [Apache ZooKeeper](https://zookeeper.apache.org/), a battle-proven service for reliable distributed coordination.

### *<u>Savepoints to Migrate, Suspend, Resume and Update Application</u>*:
- A savepoint is a consistent *<u>snapshot of an application’s state</u>* and therefore very similar to a checkpoint. However in contrast to checkpoints, *<u>savepoints need to be manually triggered</u>* and are *<u>not automatically removed</u>* when an application is stopped.
- A savepoint can be used to start a state-compatible application and initialize its state. Savepoints enable the following features in Flink:
  - __*Application Update*__: Savepoints can be used to update applications. A fixed or improved version of an application can be restarted from a savepoint that was taken from a previous version of the application. It is also possible to start the application from an earlier point in time (given such a savepoint exists) to repair incorrect results produced by the flawed version.
  - __*Cluster Migration*__: Using savepoints, applications can be *<u>migrated (or cloned) to different clusters</u>*.
  - __*Flink Version Updates*__: An application can be migrated to run on a new Flink version using a savepoint.
  - __*Application Scaling*__: Savepoints can be used to increase or decrease the [*<u>parallelism of an application</u>*](https://nightlies.apache.org/flink/flink-docs-release-1.14/docs/dev/datastream/execution/parallel/).
  - __*A/B Tests*__ and What-If Scenarios: The performance or quality of two (or more) different versions of an application can be compared by starting all versions from the same savepoint.
  - __*Pause and Resume*__: An application can be paused by taking a savepoint and stopping it. At any later point in time, the application can be resumed from the savepoint.
  - __*Archiving*__: Savepoints can be archived to be able to reset the state of an application to an earlier point in time.

### *<u>Monitoring & Control</u>*
- Flink integrates nicely with many common logging and monitoring services and provides a REST API to control applications and query information.
  - __*Web UI*__: Flink features a web UI to inspect, monitor, and debug running applications. It can also be used to submit executions for execution or cancel them.
  - __*Logging*__: Flink implements the popular [slf4j](https://www.slf4j.org/) __logging interface__ and integrates with the logging frameworks __*log4j*__ or __*logback*__.
  - __*Metrics*__: Flink features a sophisticated metrics system to collect and report system and user-defined metrics. Metrics can be exported to several reporters, including [JMX](https://en.wikipedia.org/wiki/Java_Management_Extensions), Ganglia, [Graphite](https://graphiteapp.org/), [Prometheus](https://prometheus.io/), [StatsD](https://github.com/etsy/statsd), [Datadog](https://www.datadoghq.com/), and [Slf4j](https://www.slf4j.org/).
  - __*REST API*__: Flink exposes a REST API to submit a new application, take a savepoint of a running application, or cancel an application. The REST API also exposes meta data and collected metrics of running or completed applications.

## Flink Usecases:
### Event-driven Applications
  - [Fraud detection](https://sf-2017.flink-forward.org/kb_sessions/streaming-models-how-ing-adds-models-at-runtime-to-catch-fraudsters/)
  - [Anomaly detection](https://sf-2017.flink-forward.org/kb_sessions/building-a-real-time-anomaly-detection-system-with-flink-mux/)
  - [Rule-based alerting](https://sf-2017.flink-forward.org/kb_sessions/dynamically-configured-stream-processing-using-flink-kafka/)
  - [Business process monitoring](https://jobs.zalando.com/tech/blog/complex-event-generation-for-business-process-monitoring-using-apache-flink/)
  - [Web application (social network)](https://berlin-2017.flink-forward.org/kb_sessions/drivetribes-kappa-architecture-with-apache-flink/)
  
### Data Analytics Applications
  - [Quality monitoring of Telco networks](http://2016.flink-forward.org/kb_sessions/a-brief-history-of-time-with-apache-flink-real-time-monitoring-and-analysis-with-flink-kafka-hb/)
  - [Analysis of product updates & experiment evaluation in mobile applications](https://techblog.king.com/rbea-scalable-real-time-analytics-king/)
  - [Ad-hoc analysis of live data in consumer technology](https://eng.uber.com/athenax/)
  - Large-scale graph analysis
  
### Data Pipeline Applications
  - [Real-time search index building in e-commerce](https://ververica.com/blog/blink-flink-alibaba-search)
  - [Continuous ETL in e-commerce](https://jobs.zalando.com/tech/blog/apache-showdown-flink-vs.-spark/)


###### References
- [Apache Flink](https://flink.apache.org/)
- [IoT Data Processing With Apache Flink: A Game Changer?](https://www.iotforall.com/streaming-iot-data-processing-apache-flink)



