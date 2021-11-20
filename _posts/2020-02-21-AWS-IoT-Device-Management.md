---
title:  "AWS IoT: Device Management"
date:   2020-02-21 12:33:22 +0530
categories:
  - IoT
  - Cloud
  - AWS IoT
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - IoT
  - Cloud
  - AWS IoT
author: Akhilesh Moghe
show_author_profile: true
---

# Device Management:
AWS IoT Device Management makes it easy to securely onboard, organize, monitor, and remotely manage IoT devices at scale. In this blog we will explore AWS IoT Device management in details.

## A Thing == A Device:
* In AWS IoT, things are identified by a __*unique name*__.
* A thing is stored in the registry as *JSON* data.
  * :warning: <span style="color:grey">IoT Devices with valid certificates can connect to AWS IoT without a thing name registered for them.</span>.

### Thing Attributes:
* Things can have *Attributes*, as __*name-value pairs*__ that can be used to store information about the thing, such as its *serial number* or *manufacturer*.
* Thing name can be used as the default MQTT client ID (~though this is not mandatory, using this way, simplifies use of Shadow, Certificates).
* Attributes of a thing can be used to search all things with specific attribute value. 
  * :heavy_check_mark: e.g. let’s say Attribute is Company Name = “Persistent”

### Thing Types:
* <span style="color:green">Things with a thing type</span> can have up to __*50 attributes*__.
* <span style="color:grey">Things without a thing type</span> can have up to __*three attributes*__.
* A thing can be associated with only one thing type.
* Thing Types are __*Optional*__.
* There is <u>no limit</u> on the number of thing types you can create in your account.
* Thing types are __*immutable*__.
* *<u>Deprecate/Delete</u>* Thing Type:
  * We cannot change a thing type name after it has been created. 
  * But we can Deprecate a thing type at any time to prevent new things from being associated with it.
    * All existing things associated with the thing type are unchanged.
  * Deprecating a thing type is a *reversible* operation. You can undo a deprecation.
  * Delete thing types that have no things associated with them. 
  * Only deprecated thin types can be deleted. 

### Thing Groups:
* We cannot rename a group once created.
* Groups can have Hierarchy, i.e. one group in another.
  * <span style="color:grey">But thing group cannot be member of multiple thing groups.</span>
* Thing group can be mapped as child of another group <u>only at creation time</u>,
  * i.e., <u>Parent of a thing group cannot be changed after child group creation.</u>
* A group can have __*max 100 direct child groups*__.
  * group all floors within a building
    * group all rooms on the same floor
      * group all sensors in a room
* Each group can have up to __*50 Attributes*__ ().
* Attach a policy to a parent group and it is inherited by its child groups, and by all of the things in the group and in its child groups.
* <u>A thing can belong to one or more thing groups (up to 10 groups)</u>.
* Cannot add a thing to more than one group in the same hierarchy. 
* Configure logging options for things in a group.
* Create jobs that are sent to and executed on each thing in a group and its child groups. 
* Key Advantages of Thing Groups: 
  * Updating IoT Policies gets easier with thing groups, as Policies are applied to all child groups and things registered in child and root thing group.
  * When a thing is moved from a thing group to another thing group, new policies are dynamically applied w.r.t. new thing group.
* __*<u>Dynamic Thing Groups</u>*__:
  * Dynamic thing groups will automatically add devices that meet your specified criteria and remove the devices that no longer match the criteria.
  * overrideDynamicGropus:
    * Override dynamic thing groups with static thing groups when 10-group limit is reached. If a thing belongs to 10 thing groups, and one or more of those groups are dynamic thing groups, adding a thing to a static group removes the thing from the last dynamic group.


## On-Boarding & Provisioning of Devices:
* onboard connected devices in bulk.
* onboard new devices by using:
  * the IoT management console or
  * APIs to upload templates that you populate with information like:
    * device manufacturer
    * serial number
    * X.509 identity certificates
    * security policies
* Devices may either connect directly to IoT Core or connect indirectly via an AWS Greengrass powered gateway.
* AWS IoT provides four ways to provision devices:
  * single-thing provisioning
  * bulk provisioning
  * just-in-time provisioning (JITP)
  * just-in-time registration (JITR)

### Single device provisioning:
* <u>Ideal use-cases</u>:
  * when your IoT solution requires you to provision either one device or a small number of devices at a time.
  * If your company only has a handful of devices and you expect this number to grow slowly.
* <u>Ways</u>:
  * Using Management Console
  * Using AWS CLI Commandline interface
  ![AWS CLI: IoT Thing creation](/assets/images/aws/aws-cli-iot-thing-create.jpg)
  ![AWS CLI: IoT Thing Certs and Policy creation](/assets/images/aws/aws-cli-iot-cert-policy-create-attach.jpg)
  * Using register-thing API call

### Bulk provisioning of devices:
* <u>Ideal use-cases:</u>
  * In situations such as migration of IoT devices.
  * In situations where your IoT device provisioning needs may be sporadic or unpredictable.

  * In a situation where the devices that sat on the shelf may have expired certificates, for instance.
* Details:
  * [AWS IoT Device Management Workshop: Bulk Device Provisioning](https://github.com/aws-samples/aws-iot-device-management-workshop/blob/master/AWS_IoT_Device_Management_Workshop.md#Bulk_Device_Provisioning)

### Just-in-time-provisioning:
![AWS IoT Device Provisioning JITP](/assets/images/aws/aws-device-provisioning-jitp.png)
* <u>Ideal use-cases</u>:
  * When you have a reached a point where it makes sense to consider provisioning in advance!
    * Like in cases of Consumer Products, which are produced in bulk but for a long time are not purchased.
    * Such products need activation whenever they connect for the first time to cloud.
    * the manufacturer cannot predict when a device will be used first-time and act on provisioning process.
    * So, in such situations JITP is an ideal way.
* Just-In-Time Provisioning (JITP) allows you to register devices dynamically.
* In order to use JITP:
  * you register your own CA with AWS IoT,
  * enable automatic registration within AWS IoT
  * associate the provisioning template to your CA.
  * You will also generate your own public key infrastructure (PKI) keys and certificates
  * and then copy them to your devices.
* When your device connects to AWS IoT for the first time, AWS IoT automatically calls the __*RegisterThing*__ API, its certificate is registered, and a notification is published to an internal topic (*$aws/events/certificates/registered/certificateID*).
* By using the __*AWS IoT Rules engine*__, the registration event can trigger an __*AWS Lambda*__ function that takes the appropriate steps you need to meet your workflow requirements—for example, merging content from other resources (such as an on-premises database).

## Organize Devices:
* group your device fleet into a hierarchical structure based on function, security requirements, or any other category.
* Use of Device Organization:
  * manage access policies
  * view operational metrics
  * perform actions on your devices across the entire group
### Dynamic Thing Groups:
* dynamic thing groups will automatically add devices that meet your specified criteria and remove the devices that no longer match the criteria.

* find device records for devices in the registry based on any combination of __*device attribute*__ or __*state*__ so that you can perform actions across the device group.
* use __*device connectivity indexing*__ to quickly discover which devices are currently connected or disconnected to AWS IoT.


## Monitoring Devices:
### events capabilities:
* Whenever a thing, group or type resource is __*created*__, __*updated*__, or __*deleted*__, IoT Device Management will post an __*MQTT message*__ to a topic. We can match these messages via rules to take whatever actions we would like to make.
* Configure different __*log levels*__, such as, *info*, *debug*, *warnings*, and *errors* for different principles, such as thing groups.
* These logs will be published into __*CloudWatch*__ in *JSON* format, that can easily *parse* and *search* through your logs.


## Remote Management of devices:
* Push __*software and firmware updates*__ to devices in the field to patch security vulnerabilities and improve device functionality.
* Execute __*bulk updates*__, __*control deployment*__ velocity, __*set failure thresholds*__, and define __*continuous jobs*__ to update device software automatically so they are always running the latest version.
* Send *actions* such as device __*reboots*__ or __*factory resets*__ remotely to fix software issues in the device or restore the device to its original settings.
* __*Digitally Sign*__ and __*Verify*__files that you send to your devices.
* 
