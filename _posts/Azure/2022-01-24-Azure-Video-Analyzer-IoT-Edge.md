---
title:  "Azure Video Analyzer for IoT Edge"
date:   2022-01-24 12:33:22 +0530
permalink: /_posts/azure/iot/azure-video-analyzer-iot-edge
categories:
  - Azure IoT
  - IoT
  - Edge IoT
  - Cloud
  - Video Analytics
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Azure IoT
  - IoT
  - Edge IoT
  - Cloud
  - Video Analytics
author: Akhilesh Moghe
show_author_profile: true
---

## Azure Video Analyzer on IoT Edge:
- extensible platform, enabling you to connect different video analysis edge modules.
  - [Cognitive services containers](https://docs.microsoft.com/en-us/azure/cognitive-services/cognitive-services-container-support)
  - [Custom ML edge modules built by you](https://docs.microsoft.com/en-us/azure/iot-edge/tutorial-machine-learning-edge-01-intro?view=iotedge-2020-11)
- Can be used in conjunction with *<u>computer vision SDKs</u>* and toolkits

### <u>Video Analyzer Pipeline</u>:
  - Provides a way to create Video Pipeline.
  - A pipeline is where media is actually processed. Pipelines can be *<u>associated with individual cameras</u>*.

#### <u>Pipeline Sources</u>:
- __*RTSP*__ source:
  - An RTSP source node enables you to capture media from a RTSP server.
  - The RTSP source node in a pipeline *<u>act as a client</u>* and can establish a session with an RTSP server.
  - Many devices such as most *<u>IP cameras have a built-in RTSP server</u>*.
  - __*ONVIF*__ (Open Network Video Interface Forum) mandates RTSP to be supported in its definition of Profiles G, S & T compliant devices.
  - The RTSP source node requires you to specify an *<u>RTSP URL</u>*, along with *<u>credentials</u>* to enable an *<u>authenticated connection</u>*.
- __*IoT Hub message*__ source:
  - Messages can be sent from *<u>other modules</u>*, or *<u>apps</u>* running on the Edge device, or from the *<u>cloud</u>*. Such messages can be delivered (routed) to a named input on the video analyzer module.

#### <u>Pipeline Processors</u>:
- __*Motion detection*__ processor:
  - Enables you to detect motion in live video.
  - Examines incoming video frames and *<u>determines if there is movement</u>* in the video. If motion is detected, it *<u>passes on the video frame to the next node</u>* in the pipeline and *<u>emits an event</u>*.
  - The motion detection processor node (in conjunction with other nodes) can be used to *<u>trigger recording of the incoming video when there is motion detected</u>*.
- __*HTTP extension*__ processor:
  - Enables you to extend the pipeline to your own custom IoT Edge module.
  - This node takes decoded video frames as input, and *<u>relays such frames to an HTTP REST endpoint exposed by your module</u>*, where you can analyze the frame with an AI model and return inference results back.
  - Node has a *<u>built-in image formatter</u>* for scaling and encoding of video frames before they are relayed to the HTTP endpoint.
  - Manage encoder supports JPEG, PNG, BMP, and RAW formats.
  - Enables extensibility scenarios using the [HTTP extension protocol](https://docs.microsoft.com/en-us/azure/azure-video-analyzer/video-analyzer-docs/http-extension-protocol), to *<u>expose your own AI to a pipeline via an HTTP REST endpoint</u>*.
  - <u>Usage Scenarios</u>:
    - You want better interoperability with existing HTTP Inferencing systems.
    - Low-performance data transfer is acceptable.
    - You want to use a simple *<u>request-response interface to Video Analyzer</u>*.
    - Used with Low frame rates usually due to performance reasons.
- __*gRPC extension*__ processor:
  - Takes decoded video frames as the input and *<u>relays such frames to a gRPC endpoint exposed by your module</u>*.
  - [gRPC extension protocol](https://docs.microsoft.com/en-us/azure/azure-video-analyzer/video-analyzer-docs/grpc-extension-protocol) offers __*high content transfer performance*__ using:
    - *<u>shared memory</u>* or
    - directly *<u>embedding the frame into the body of gRPC messages</u>*.
  - Ideal for scenarios where *<u>performance and/or optimal resource utilization</u>* is a priority.
  - A gRPC session is a single connection from the gRPC client to the gRPC server over the TCP/TLS port.
  - In a single session: The client sends a *<u>media stream descriptor</u>* followed by *<u>video frames</u>* to the server as a [protobuf message](https://github.com/Azure/video-analyzer/tree/main/contracts/grpc) over the gRPC stream session.
  - The server validates the stream descriptor, analyses the video frame, and returns inference results as a protobuf message.
  - <u>Usecase</u>: use a gRPC extension processor node when you:
    - Want to use a structured contract (for example, structured messages for requests and responses)
    - Want to use [Protocol Buffers (protobuf)](https://developers.google.com/protocol-buffers) as its underlying message interchange format for communication.
    - You want to communicate with a gRPC server in a single stream session instead of the traditional request-response model needing a custom request handler to parse incoming requests and call the right implementation functions.
    - Want *<u>low latency and high throughput</u>* communication between Video Analyzer and your module.
    - Used with *<u>High frame rates</u>* usually for performance reasons.
- __*Cognitive Services extension*__ processor:
  - Enables you to extend the pipeline to *<u>Spatial Analysis IoT Edge module</u>*.
  - <u>Usage Scenarios</u>:
    - You want better interoperability with existing Spatial Analysis operations.
    - Want to use all the benefits of gRPC protocol, accuracy, and performance of Microsoft built and supported AI.
    - Analyze multiple camera feeds at low latency and high throughput.
- __*Signal gate*__ processor:
  - Enables you to *<u>conditionally forward media from one node to another</u>*.
  - The signal gate processor node must be *<u>immediately followed by</u>* a __*video sink*__ or __*file sink*__.
- __*Object tracker*__ processor:
  - Node comes in handy when you need to *<u>detect objects in every frame</u>*, but the edge device does not have the necessary compute power to be able to apply the AI model on every frame.
  - One common use of the object tracker processor node is to *<u>detect when an object crosses a line</u>*.
- __*Line crossing*__ processor:
  - Enables you to detect when an object crosses a line defined by you.
  - Also maintains a *<u>count of the number of objects that cross the line</u>*.
  - This node must be used downstream of an object tracker processor node.

#### <u>Pipeline Sinks</u>:
- __*Video*__ Sink:
  - Node *<u>saves video</u>* and associated metadata to your *<u>Video Analyzer cloud resource</u>*.
  - Video can be recorded continuously or discontinuously (based on events).
  - Video sink node can *<u>cache video on the edge device</u>* if connectivity to cloud is lost and *<u>resume uploading</u>* when connectivity is restored.
- __*File*__ Sink:
  - Node writes video to a location on the local file system of the edge device.
  - There can *<u>only be one file sink node in a pipeline</u>*, and it must be *<u>downstream from a signal gate</u>* processor node.
  - This limits the duration of the output files to values specified in the signal gate processor node properties.
  - You can also set the maximum size that the Video Analyzer edge module can use to cache data.
- __*IoT Hub message*__ sink:
  - Publish events to IoT Edge hub.
  - The IoT Edge hub can be configured to route the data to other modules or apps on the edge device, or to IoT Hub in the cloud.
  
#### <u>Pipeline Limitations</u>:
- Only IP Cameras with RTSP support.
- Configure these cameras to use H.264 video and AAC audio.
- only supports RTSP with [interleaved RTP streams](https://datatracker.ietf.org/doc/html/rfc2326#section-10.12). In this mode, RTP traffic is tunneled through the RTSP TCP connection.
- RTP traffic over UDP is not supported.

#### <u>Pipeline Extensions</u>:
- Extension plugin can make use of traditional *<u>image-processing</u>* techniques or *<u>computer vision AI models</u>*.
- Extension processor node relays video frames to the configured endpoint and acts as the interface to your extension.
- The connection can be made to a local or remote endpoint, and it can be secured by authentication and TLS encryption, if necessary.
- We have to provide a HTTP/gRPC Server, to which a Video Analyzer module will act as HTTP/gRPC client.
- HTTP extension processor
- gRPC extension processor
- Cognitive Services extension processor
- Custom Inferencing model:
  - Run inference models of your choice on any available inference runtime, such as ONNX, TensorFlow, PyTorch, or others in your own docker container.
  - Invoked via the HTTP/gRPC/Cognitive extension processor.

- The Video Analyzer cloud service enables you to play back the video and video analytics from such workflows.

#### <u>Use-cases</u>:
##### [<u>Detect motion and record video on edge devices</u>](https://docs.microsoft.com/en-us/azure/azure-video-analyzer/video-analyzer-docs/detect-motion-record-video-edge-devices?pivots=programming-language-csharp)
  ![detect-motion-video-analyzer-iotedge](/assets/images/azure/iot/edge/detect-motion-video-analyzer-iotedge.png)

##### [<u>Detect motion, record video to Video Analyzer in the cloud</u>](https://docs.microsoft.com/en-us/azure/azure-video-analyzer/video-analyzer-docs/detect-motion-record-video-clips-cloud)
  ![detect-motion-video-analyzer-cloud](/assets/images/azure/iot/edge/detect-motion-video-analyzer-cloud.png)

##### [<u>Track objects in a live video</u>](https://docs.microsoft.com/en-us/azure/azure-video-analyzer/video-analyzer-docs/track-objects-live-video)
  ![track-objects-in-a-live-video](/assets/images/azure/iot/edge/track-objects-in-a-live-video.png)

&nbsp;

###### References:
- [Video Analyzer Pipeline](https://docs.microsoft.com/en-us/azure/azure-video-analyzer/video-analyzer-docs/pipeline)
- [Pipeline extension](https://docs.microsoft.com/en-us/azure/azure-video-analyzer/video-analyzer-docs/pipeline-extension)
- [Video Sink - Video Recording to Azure Storage with Intermittent Connectivity](https://docs.microsoft.com/en-us/azure/azure-video-analyzer/video-analyzer-docs/continuous-video-recording)
- __Video Analytics on Azure IoT Edge__:
  - [Azure-Samples/Custom-vision-service-iot-edge-raspberry-pi](https://github.com/Azure-Samples/Custom-vision-service-iot-edge-raspberry-pi)


