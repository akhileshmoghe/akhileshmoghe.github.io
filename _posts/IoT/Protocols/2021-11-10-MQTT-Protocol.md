---
title:  "MQTT Protocol"
date:   2021-11-10 12:33:22 +0530
permalink: /_posts/iot/protocols/mqtt
categories:
  - IoT
  - Edge IoT
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - IoT
  - Edge IoT
author: Akhilesh Moghe
show_author_profile: true
---

# MQTT Protocol
  - MQTT = Message Queuing Telemetry Transport
	  Telemetry = Tele-Metering = Remote Measurements
  - Originally Developed by IBM, now Open Sourced.
  - Though MQ stands for 'Message Queuing', actually there's NO Messages being Queued.
  - It's a __*<u>Publish/Subscribe</u>*__ mechanism.
    - Sensor Devices Publish data to the Topics/Servers/Brokers.
    - Topics are subscribed by other devices such as mobile devices those are looking for data from Sensors.
  - *<u>Low Bandwidth Protocol</u>*
    - Messages being sent by devices are very small in bytes.
    - e.g. Temperature sensor sending 100 degree reading.
  - Small Code Footprint
    - The code used for this is very small.
  - Used in:
    - Facebook Messenger for iOS and Android
    - PubNub

## MQTT Ports
  - Protocol: TCP/IP
  - Ports:
    - 1883 - non-encrypted communication
    - 8883 - encrypted communication
    
## Maximum Payload Size
  - MQTT Protocol Max Payload - 256 MBs
  - AWS IoT MQTT Max Payload - 128 KBs

## Quality of Service
  - Levels:\
		__*<u>0 = At Most Once</u>*__ (Best effort, No Ack)\
		  - Sender does not store messages, neither the receiver sends any acknowledgement.\
		  - This method requires only one message and once the message is sent to the broker by the client it is deleted from the message queue.\
		  - Therefore QoS 0 nullifies the chances of duplicate messages, which is why it is also known as the __*<u>fire and forget</u>*__ method.\
		  - It provides a minimal and most __*<u>unreliable</u>*__ message transmission level that offers the __*<u>fastest delivery effort</u>*__.\
		  ![mqtt-qos-0](/assets/images/mqtt/mqtt-qos-0.png)\
\
		__*<u>1 = At Least Once</u>*__ (Acknowledged, Retransmitted if Ack not received)\
		  - Using QoS 1, the delivery of a message is guaranteed (at least once, but the *<u>message may be sent more than once</u>* , if necessary).\
		  - This method needs two messages.\
		  - Here, the sender sends a message and waits to receive *<u>an acknowledgment</u>* (__*<u>PUBACK</u>*__ message).\
		  - If it receives an acknowledgment from the client then it deletes the message from the outward-bound queue.\
		  - In case, it does not receive a PUBACK message, it resends the message with the *<u>duplicate flag (DUP flag) enabled</u>*.\
		  ![mqtt-qos-1](/assets/images/mqtt/mqtt-qos-1.png)\
\
		__*<u>2 = Exactly Once</u>*__\
		  - The QoS 2 level setting guarantees exactly-once delivery of a message.\
		  - This is the *<u>slowest</u>* of all the levels and needs four messages.\
		  - In this level, the *<u>sender</u>* sends a message (__*<u>PUBLISH</u>*__) and waits for an *<u>acknowledgment</u>* (__*<u>PUBREC</u>*__ message).\
		  - The *<u>receiver</u>* also sends a PUBREC message.\
		  - If the *<u>sender</u>* of the message fails to receive an acknowledgment (PUBREC), it sends the message again with the *<u>DUP flag enabled</u>*.\
		  - Upon receiving the acknowledgment message PUBREC, the *<u>sender</u>* transmits the *<u>message release message</u>* (__*<u>PUBREL</u>*__).\
		  - If the *<u>receiver</u>* does not receive the PUBREL message it resends the PUBREC message.\
		  - Once the *<u>receiver</u>* receives the PUBREL message, It forwards the message to all the subscribing clients.\
		  - Note: *<u>For Sender Broker is the receiver and for Receiver Broker is the sender</u>*.\
		  - Thereafter the *<u>receiver</u>* sends *<u>a publish complete</u>* (__*<u>PUBCOMP</u>*__) message.\
		  - In case the *<u>sender</u>* does not receive the PUBCOMP message, it resends the PUBREL message.\
		  - Once the sending client receives the PUBCOMP message, the transmission process is marked as completed and the message
can be deleted from the outbound queue.\
      ![mqtt-qos-2](/assets/images/mqtt/mqtt-qos-2.png)

  - these messages are same as used in WiFi communication.
			
## Retained Messages
  - Server keeps messages even after sending it to all Subscribers.
  - New Subscribers get the retained messages.
    - We had seen this behavior in PubNub, where last message Published to Channel was retained till the next message is Published to that channel.
    - This Last message was available to new subscriber.
    - *<u>Only One Message</u>* is retained per Topic.
    - Usecase:
      - Sensor periodically sending a message on a topic. New subscriber will get the last state of the sensor with Retained message.
    - Reference: [MQTT Retained Messages Explained](/assets/docs/mqtt/MQTT_Retained_Messages_Explained.pdf)
	
## Clean Session and Durable Session
  - *<u>Clean Session</u>*:
    - Clean Session Flag = 1	[Optional]
    - All of the client’s subscriptions are removed when it disconnects from the server.
  - *<u>Durable Session</u>*:
    - Clean Session Flag = 0	[Optional]
    - The client’s subscriptions remain in effect after any disconnection.
    - In this event, subsequent messages that arrive carrying a High QoS designation are stored for delivery after the connection is reestablished.

### Retained messages, Clean Session and QoS inter-related behavior
  ![Retained messages, Clean Session and QoS inter-related behavior](/assets/images/mqtt/RetainedMessages_CleanSession_QoS.png)\
  Reference: [MQTT Retained Messages Explained](http://www.steves-internet-guide.com/mqtt-retained-messages-example/)

## Wills
  - A Will or a Message is informed by client with server that should be published to a specific Topic or Topics in the event of an unexpected disconnection.
  - A Will is an alarm or security settings where system when a remote sensor has lost contact with the network.
	
## Keep Alive Messages
  - Periodically sent
	
## Topic Trees, Strings
  - Topics are organized Hierarchically into Topic Trees, using the '/' character to create subtopics in the Topic String.
  - *<u>Topic String</u>*
    - A character string that identifies the Topic of a publish/subscribe message.
    - Topic strings can contain either of two Wildcards:
    - These __*<u>WildCards</u>*__ Allows subscribers to match patterns within strings defined by message publishers
    - Wildcard: `#`:
      - Multilevel
      - used to match any number of levels within a Topic.
        - e.g. subscribers to `truck/contents/#` receive all messages that are designated for the topics:
        - `truck/contents`
        - `truck/contents/rfid`
    - Wildcard: `+`:
      - Single Level
      - used to match Just ONE Topic Level.
        - e.g. `truck/+` <u>matches</u> `truck/contents` *<u>but not</u>* `truck/contents/rfid`

## MQTT Essentials - All key features of MQTT
<object data="/assets/docs/iot/protocols/mqtt/hivemq-ebook-mqtt-essentials.pdf" type="application/pdf" width="850px" height="1200px">
  <embed src="/assets/docs/iot/protocols/mqtt/hivemq-ebook-mqtt-essentials.pdf">
      <p>This browser does not support PDFs. Please download the PDF to view it: <a href="/assets/docs/iot/protocols/mqtt/hivemq-ebook-mqtt-essentials.pdf">Download PDF</a>.</p>
  </embed>
</object>

## MQTT Brokers Comparison
- [Stress-Testing MQTT Brokers: A Comparative Analysis of Performance Measurements:](/assets/docs/mqtt/Stress-Testing-MQTT-Brokers.pdf)
  - Tested Borkers:
    - [*<u>Mosquitto</u>*](https://mosquitto.org/) - [*<u>Bevywise MQTT Route</u>*](https://www.bevywise.com/mqtt-broker/) - [*<u>ActiveMQ</u>*](https://activemq.apache.org/) - [*<u>HiveMQ CE</u>*](https://www.hivemq.com/) - [*<u>VerneMQ</u>*](https://vernemq.com/) - [*<u>EMQ X</u>*](https://www.emqx.io/)
  - __*<u>Mosquitto</u>*__ outperforms the other considered solutions in most metrics;
  - __*<u>ActiveMQ</u>*__ is the best performing one in terms of *<u>Scalability</u>* due to its multi-threaded implementation.
  - __*<u>Bevywise MQTT Route</u>*__ has promising results for *<u>resource-constrained</u>* scenarios.
  - __*<u>ActiveMQ</u>*__ scales well in distributed/multi-core environment to beat all other brokers’ performance.
  - If the hardware is *<u>resource-constrained</u>* (CPU/Memory/IO/Performance), then __*<u>Mosquitto</u>*__ or __*<u>Bevywise MQTT Route</u>*__ can be taken as better choices.

## Testing Scenarios for an MQTT Broker
- [Top 10 IoT Scalability Tests for an MQTT Broker](https://www.hivemq.com/blog/mqtt-broker-scalability-tests/)

## MQTT Broker Selection Criteria:
- [MQTT Broker Comparison – Which is the Best for Your IoT Application?](https://www.hivemq.com/blog/mqtt-broker-comparison-iot-application/)

## Running MQTT broker in Containerized mode
  - [eclipse-mosquitto](https://hub.docker.com/_/eclipse-mosquitto?tab=description)
    - Maintained by: the Eclipse Foundation
    - Supported architectures: (more info) amd64, arm32v6, arm64v8, i386, ppc64le, s390x


