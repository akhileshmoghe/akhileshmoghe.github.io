---
title:  "AWS IoT: Device Management"
date:   2020-02-21 12:33:22 +0530
permalink: /_posts/aws/iot/device_management
categories:
  - IoT
  - Cloud Computing
  - AWS
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - IoT
  - Cloud Computing
  - AWS
author: Akhilesh Moghe
show_author_profile: true
sidebar:
 nav: IoT
---

# Device Management:
AWS IoT Device Management makes it easy to securely onboard, organize, monitor, and remotely manage IoT devices at scale. In this blog we will explore AWS IoT Device management in details.

## On-Boarding & Provisioning of Devices:
* Onboard connected devices in bulk.
* Onboard new devices by using:
  * the IoT management console or
  * APIs to upload templates that you populate with information like:
    * device manufacturer
    * serial number
    * X.509 identity certificates
    * security policies
* Devices may either connect directly to IoT Core or connect indirectly via an AWS Greengrass powered gateway.
* AWS IoT provides four ways to provision devices:
  * Single-thing provisioning
  * Bulk provisioning
  * Just-In-Time Provisioning (JITP)
  * just-In-Time Registration (JITR)

### Single device provisioning:
* <u>Ideal use-cases</u>:
  * When your IoT solution requires you to provision either one device or a small number of devices at a time.
  * If your company only has a handful of devices and you expect this number to grow slowly.
* <u>Ways</u>:
  * Using Management Console
  * Using AWS CLI Commandline interface
  ![AWS CLI: IoT Thing creation](/assets/images/aws/aws-cli-iot-thing-create.jpg)
  ![AWS CLI: IoT Thing Certs and Policy creation](/assets/images/aws/aws-cli-iot-cert-policy-create-attach.jpg)
  * Using [register-thing](https://docs.aws.amazon.com/iot/latest/apireference/API_RegisterThing.html) API call

  ---

### Bulk provisioning of devices:
* <u>Ideal use-cases:</u>
  * In situations such as migration of IoT devices.
  * In situations where your IoT device provisioning needs may be sporadic or unpredictable.
    * An example of this operation might be a situation where your company has purchased or produced a large number of new devices that require immediate provisioning.
  * In a situation where the devices that sat on the shelf may have expired certificates, for instance.
  
* Details:
  * [AWS IoT Device Management Workshop: Bulk Device Provisioning](https://github.com/aws-samples/aws-iot-device-management-workshop/blob/master/AWS_IoT_Device_Management_Workshop.md#Bulk_Device_Provisioning)
  * [Bulk registration Documentation](https://docs.aws.amazon.com/iot/latest/developerguide/bulk-provisioning.html)

  ---

### Just-in-time-provisioning:
![AWS IoT Device Provisioning JITP](/assets/images/aws/aws-device-provisioning-jitp.png)
* <u>Ideal use-cases</u>:
  * When you have a reached a point where it makes sense to consider provisioning in advance!
    * Like in cases of Consumer Products, which are produced in bulk but for a long time are not purchased.
    * Such products need activation whenever they connect for the first time to cloud.
    * The manufacturer cannot predict when a device will be used first-time and act on provisioning process.
    * So, in such situations JITP is an ideal way.
* Just-In-Time Provisioning (JITP) allows you to register devices dynamically.
* In order to use JITP:
  * You register your own CA with AWS IoT,
  * *<u>Enable automatic registration</u>* within AWS IoT
  * *<u>Associate the provisioning template to your CA</u>*.
    * __*Provisioning Template*__
      * There are *<u>two types of provisioning templates</u>* in AWS IoT.
        * One is used for just-in-time provisioning (JITP) and bulk registration and
        * The second is used for fleet provisioning.
      * [Template example for JITP and bulk registration](https://docs.aws.amazon.com/iot/latest/developerguide/provision-template.html#bulk-template-example)
  * You will also generate your own public key infrastructure (PKI) keys and certificates
  * And then copy them to your devices.

  #### Register your CA certificate, to create your own client/device certificates:
  * AWS IoT supports client/device certificates signed by other root certificate authorities (CA).
  * You can register client/device certificates signed by another root CA; however, if you want the device or client to register its client certificate when it first connects to AWS IoT, the root CA must be registered with AWS IoT.
  * Reference Documentation:
    * [Manage your CA certificates](https://docs.aws.amazon.com/iot/latest/developerguide/manage-your-CA-certs.html)
      * [Create a CA certificate](https://docs.aws.amazon.com/iot/latest/developerguide/create-your-CA-cert.html)
      * [Register your CA certificate](https://docs.aws.amazon.com/iot/latest/developerguide/register-CA-cert.html)
      * [Deactivate a CA certificate](https://docs.aws.amazon.com/iot/latest/developerguide/deactivate-ca-cert.html)
    * [Create a client/device certificate using your CA certificate](https://docs.aws.amazon.com/iot/latest/developerguide/create-device-cert.html)

* When your device connects to AWS IoT for the first time, AWS IoT automatically calls the __*RegisterThing*__ API, its certificate is registered, and a notification is published to an internal topic `$aws/events/certificates/registered/certificateID`.
* By using the __*AWS IoT Rules engine*__, the registration event can trigger an __*AWS Lambda*__ function that takes the appropriate steps you need to meet your workflow requirements—for example, merging content from other resources (such as an on-premises database).
* Detail Steps for JITP:
  * [AWS IoT Device Management Workshop: JITP - Just-in-Time Provisioning](https://github.com/aws-samples/aws-iot-device-management-workshop/blob/master/AWS_IoT_Device_Management_Workshop.md#JITP)

  ---

### Just-in-time registration (JITR)
![AWS IoT Device Provisioning JITR](/assets/images/aws/aws-device-provisioning-jitr.png)
* <u>Ideal use-cases</u>:
  * Just-in-time registrations makes the most sense when your IoT solution requires flexibility.
  * If your company produces several different types of IoT devices, it may make the most sense to use just-in-time registration to ensure that individual IoT devices are provisioned based on their specific type.
  * For instance, you may use different device templates for light bulbs and door locks using just-in-time registration.
* With JITR, when you connect to AWS IoT with the device certificate for the first time, the service will *<u>detect an unknown certificate signed by a registered CA</u>* and will *<u>auto-register the device certificate</u>*.
* On successful registration, AWS IoT will *<u>publish a registration message on a reserved MQTT topic</u>* and *<u>disconnect the client</u>*.
* This *<u>MQTT registration event</u>* will trigger the attached *<u>AWS Lambda rules engine action</u>*, which will complete the provisioning of the certificate.
* After these steps, your device certificate will be ready to connect and authenticate with AWS IoT.
* One of the key benefits to JITR is flexibility. While JITP only offers static provisioning of IoT devices (with __*Provisioning Template*__), JITR allows you to dynamically determine how to register and associate your certificates, policies, and things.
* Detail Steps for JITP:
  * [AWS IoT Device Management Workshop: JITR - Just-in-Time Registration](https://github.com/aws-samples/aws-iot-device-management-workshop/blob/master/AWS_IoT_Device_Management_Workshop.md#jitr---just-in-time-registration)
  * Check out this AWS blog post to get detailed steps on using JITR for your own IoT devices.
    * [Just-in-Time Registration of Device Certificates on AWS IoT](https://aws.amazon.com/blogs/iot/just-in-time-registration-of-device-certificates-on-aws-iot/)

  ---

### Comparison of JITP vs JITR
![JITP-JITR-Workflow](/assets/images/aws/iot/device-management/jitr-vs-jitp-workflow.png)

![JITP-JITR-comparison](/assets/images/aws/iot/device-management/jitr-vs-jitp-comparison.png)


## Organize Devices:
* group your device fleet into a hierarchical structure based on function, security requirements, or any other category.
* Use of Device Organization:
  * manage access policies
  * view operational metrics
  * perform actions on your devices across the entire group

### A Thing == A Device:
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

### *<u>Thing Groups</u>*
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
* Configure logging options for things in a group.
* Create jobs that are sent to and executed on each thing in a group and its child groups.
  #### *<u>Key Advantages of Thing Groups</u>*
  * Updating IoT Policies gets easier with thing groups, as Policies are applied to all child groups and things registered in child and root thing group.
  * When a thing is moved from a thing group to another thing group, new policies are dynamically applied w.r.t. new thing group.

  #### *<u>Limitations with Thing Groups</u>*
  * If a thing group will be a child of another group, the parent must be specified at the time of creation
  * <u>A thing can belong to one or more thing groups (up to 10 groups)</u>.
  * Cannot add a thing to more than one group in the same hierarchy. 
  * Groups cannot be renamed
  * Group names cannot contain international characters

  #### *<u>Dynamic Thing Groups</u>*
  - Dynamic thing groups update group membership through search queries. Using dynamic thing groups, you can change the way you interact with things depending on their connectivity, registry, or shadow data.
  - Because dynamic thing groups are tied to your fleet index, you must enable the fleet indexing service to use them.
  - With dynamic thing groups, you can specify a dynamic thing group as a target for a job. Only things that meet the criteria that define the dynamic thing group perform the job.
  - You can attach a policy to a dynamic thing group. The policy applies only to those things that meet the criteria that defines the dynamic thing group.
  * dynamic thing groups will automatically add devices that meet your specified criteria and remove the devices that no longer match the criteria.
    * overrideDynamicGropus:
      * Override dynamic thing groups with static thing groups when 10-group limit is reached. If a thing belongs to 10 thing groups, and one or more of those groups are dynamic thing groups, adding a thing to a static group removes the thing from the last dynamic group.
      * You can use the overrideDynamicGroups flag to make static groups take priority over dynamic groups.
  - __*<u>Dynamic Vs Static Thing Groups</u>*__:
    - Membership in dynamic thing groups is not explicitly defined
    - Dynamic thing groups cannot be part of a hierarchy
    - Different commands are used to create, update, and delete dynamic thing groups
  - *<u>Dynamic thing group creation is not instantaneous</u>*.
    - The dynamic thing group backfill takes time to complete. When a dynamic thing group is created, the status of the group is set to *<u>BUILDING</u>*.
    - When the backfill is complete, the status changes to *<u>ACTIVE</u>*.
    - One more status is *<u>REBUILDING</u>*.
  - APIs/Action for Dynamic Thing Groups:
    - CreateDynamicThingGroup
    - DescribeThingGroup
    - UpdateDynamicThingGroup
    - DeleteDynamicThingGroup
- Detail steps for Thing Groups:
  - [AWS IoT Device Management Workshop: Thing Groups](https://github.com/aws-samples/aws-iot-device-management-workshop/blob/master/AWS_IoT_Device_Management_Workshop.md#thing-groups)


### *<u>Fleet indexing and search</u>*
- Fleet Indexing is a *<u>managed service</u>* that enables you to index and *<u>search</u>* your *<u>registry data</u>*, *<u>shadow data</u>*, and *<u>device connectivity data (device life-cycle events)</u>* in the cloud.
- After you set up your fleet index, the service manages the indexing of updates for your thing groups, thing registries and thing shadows.
- You can use a *<u>simple query language</u>* to search across this data. You can also *<u>create a dynamic thing group with a search query</u>*.
- __*AWS_Things*__ is the index created for all of your things.
- __*AWS_ThingGroups*__ is the index that contains all of your thing groups.
- AWS IoT keeps indexes continuously updated with your latest data.
- Further References:
  - [Example thing queries](https://docs.aws.amazon.com/iot/latest/developerguide/example-queries.html)
  - [Example thing group queries](https://docs.aws.amazon.com/iot/latest/developerguide/example-thinggroup-queries.html)
- Fleet Indexing offers several *<u>options</u>* to index data from AWS IoT:
  - __OFF__: no indexing at all
  - __REGISTRY__: index registry data only
  - __REGISTRY_AND_SHADOW__: index registry data and shadow data
  - __REGISTRY_AND_CONNECTIVITY_STATUS__: index registry data and connectivity status
  - __REGISTRY_AND_SHADOW_AND_CONNECTIVITY_STATUS__: index registry data, shadow data and connectivity status
- Detail steps for fleet indexing:
  - [AWS IoT Device Management Workshop: Fleet Indexing](https://github.com/aws-samples/aws-iot-device-management-workshop/blob/master/AWS_IoT_Device_Management_Workshop.md#fleet-indexing)
- Use-cases:
  - You can use fleet indexing when you want to take an action on a specific subset of the IoT devices in your fleet.
  - You can also use fleet indexing to help you make informed decisions.
    - Suppose your company's products newly released have features that only work on specific versions of the software. You can use fleet indexing to figure out which devices are running outdated versions of the software.
    
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

## WORKSHOP AWS IOT DEVICE MANAGEMENT 
* In the workshop several features of AWS IoT Device Management are covered.
* To adopt the knowledge several hands-on exercises are also provided.
* [AWS Workshop Link](https://iot-device-management.workshop.aws/en/)
* [GitHub Link for workshop material: aws-samples/aws-iot-device-management-workshop](https://github.com/aws-samples/aws-iot-device-management-workshop)

