---
title:  "IoT Design Patterns"
date:   2022-01-12 12:33:22 +0530
permalink: /_posts/iot/design/iot_design_patterns
categories:
  - Cloud
  - IoT
  - Edge IoT
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Cloud
  - IoT
  - Edge IoT
author: Akhilesh Moghe
show_author_profile: true
---

This article is just a reference guide for various IoT design patterns which are well-known and time tested. This blog is just a repository of design patterns explained on [IoT Atlas project](https://iotatlas.net/en/).

## Commands
<object data="/assets/docs/iot/design/iot-atlas/Command-IoT-Design-Pattern.pdf" type="application/pdf" width="875px" height="1000px">
  <embed src="/assets/docs/iot/design/iot-atlas/Command-IoT-Design-Pattern.pdf">
      <p>This browser does not support PDFs. Please download the PDF to view it: <a href="/assets/docs/iot/design/iot-atlas/Command-IoT-Design-Pattern.pdf">Download PDF</a>.</p>
  </embed>
</object>

  ---

## Device Bootstrap
<object data="/assets/docs/iot/design/iot-atlas/Device-Bootstrap-IoT-Design-Pattern.pdf" type="application/pdf" width="875px" height="1000px">
  <embed src="/assets/docs/iot/design/iot-atlas/Device-Bootstrap-IoT-Design-Pattern.pdf">
      <p>This browser does not support PDFs. Please download the PDF to view it: <a href="/assets/docs/iot/design/iot-atlas/Device-Bootstrap-IoT-Design-Pattern.pdf">Download PDF</a>.</p>
  </embed>
</object>

### Device On-boarding & Provioising with AWS IoT
- Refer Detail Blog here: [AWS IoT: On-Boarding & Provisioning of Devices](/_posts/aws/iot/device_management#on-boarding--provisioning-of-devices)

  ---

## Device State Replica
<object data="/assets/docs/iot/design/iot-atlas/Device-State-Replica-IoT-Design-Pattern.pdf" type="application/pdf" width="875px" height="1000px">
  <embed src="/assets/docs/iot/design/iot-atlas/Device-State-Replica-IoT-Design-Pattern.pdf">
      <p>This browser does not support PDFs. Please download the PDF to view it: <a href="/assets/docs/iot/design/iot-atlas/Device-State-Replica-IoT-Design-Pattern.pdf">Download PDF</a>.</p>
  </embed>
</object>

### AWS IoT Device Shadow
- [AWS IoT Device Shadow service](https://docs.aws.amazon.com/iot/latest/developerguide/iot-device-shadows.html)
  - Choosing to use named or unnamed shadows
    - [AWS IoT Core now supports multiple shadows for a single IoT device](https://aws.amazon.com/about-aws/whats-new/2020/07/aws-iot-core-now-supports-multiple-shadows-for-a-single-iot-device/)
    - A thing object can have multiple named shadows, and no more than one unnamed shadow.
    - Named Shadow:
      - With named shadows, you can create different views of a thing object’s state. For example, you could divide a thing object with many properties into shadows with logical groups of properties, each identified by its shadow name. You could also limit access to properties by grouping them into different shadows and using policies to control access.
    - Unnamed Shadow:
      - If you expect your IoT solution to have a limited need for shadow data.
      - Each AWS IoT thing object can have only one unnamed shadow.
  - Accessing shadows
  - Message order
  - Trim shadow messages
    - To reduce the size of shadow messages sent to your device, define a rule that selects only the fields your device needs then republishes the message on an MQTT topic to which your device is listening.
  - [Using shadows in devices](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-comms-device.html)
    - Processing messages while the device is connected to AWS IoT
    - Processing messages when the device reconnects to AWS IoT
  - [Using shadows in apps and services](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-comms-app.html)
    - Processing state changes while the app or service is connected to AWS IoT
    - Detecting a device is connected
  - [Interacting with shadows](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-data-flow.html)
    - Updating a shadow
    - Retrieving a shadow document
    - Deleting shadow data
  - Protocol support
    - [MQTT](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html)
      ```
      /get
      /get/accepted
      /get/rejected
      /update
      /update/delta
      /update/accepted
      /update/documents
      /update/rejected
      /delete
      /delete/accepted
      /delete/rejected
      ```
    - [REST APIs over HTTPS](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-rest-api.html)
      - GetThingShadow
      - UpdateThingShadow
      - DeleteThingShadow
      - ListNamedShadowsForThing
  - [Device Shadow service documents](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-document.html)
    - Shadow document examples
    - Document properties
    - Delta state
    - Versioning shadow documents
    - Client tokens in shadow documents
    - Empty shadow document properties
    - Array values in shadow documents
  - [Device Shadow error messages](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-error-messages.html)
  - [AWS IoT Device Shadow demo application with FreeRTOS](https://docs.aws.amazon.com/freertos/latest/userguide/shadow-demo.html)
  - [Retaining the device state while the device is offline](https://docs.aws.amazon.com/iot/latest/developerguide/iot-shadows-tutorial.html)
  - [Video: How to Get Started with Device Shadows for AWS IoT Core](https://www.youtube.com/watch?v=XsKGRA5FhiE&ab_channel=AmazonWebServices)


###### References
