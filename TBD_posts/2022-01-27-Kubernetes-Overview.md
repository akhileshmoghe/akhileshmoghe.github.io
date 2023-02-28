---
title:  "Kubernetes Overview"
#permalink: /_posts/kubernetes/kubernetes-overview
date:   2022-01-27 12:33:22 +0530
categories:
  - DevOps
  - Edge IoT
  - Microservices
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
#full_width: true
tags:
  - DevOps
  - Edge IoT
  - Microservices
author: Akhilesh Moghe
show_author_profile: true
---

## Challenges addressed by Kubernetes
- __*Connecting containers*__ across multiple hosts/devices/machines.
- __*Scaling*__ containers:
  - In a *traditional environment*, an application (such as a <u>web server</u>) would be a __*monolithic application*__ placed on a *<u>dedicated server</u>*. As the web traffic increases, the application would be tuned, and perhaps moved to bigger and bigger hardware. After a couple of years, a lot of customization may have been done in order to meet the current web traffic needs.
  - Kubernetes approaches the same issue by *<u>deploying a large number of small web servers</u>*, or *<u>microservices</u>*.
- __*Load Balancing*__.
- __*Deploying applications*__ *<u>without downtime</u>*.
- __*Service Discovery*__
- __*Decoupling of services*__/aspects/features:
  - In server-client architecture, clients expect the server processes to die but be replaced, leading to a transient server deployment.
  - The transient nature of smaller services also allows for decoupling (independency).
- __*Continuous Integration*__ for building, testing, verifying container images.
- __*Logging & Monitoring*__ of deployed containers (Non-functional requirements).
- __*Re-deploy*__ failed containers.
- Rolling __*Updates*__ to containers.
- Container __*Rollbacks*__
- __*Stop*__, __*Delete*__ Containers after use
- *<u>flexible, scalable</u>*, and easy-to-use __*Network*__ and __*Storage*__:
  - Containers launched on any worker node/hosts, the network must join the resource to other containers, while still *<u>keeping the traffic </u>*__*<u>secure</u>*__*<u> from others</u>*.
  - Need a *<u>storage structure</u>* which provides and keeps or recycles storage in a seamless manner.
- __*Cloud Agnostic*__ Microservice Application.

## Features of Kubernetes
- Kubernetes is an *<u>Open-source Container Orchestration tool</u>* with many features like:
- __*Automating Deployment*__ (*<u>bin packing</u>*):
  - You provide Kubernetes with a cluster of nodes that it can use to run containerized tasks. You tell Kubernetes how much CPU and memory (RAM) each container needs. Kubernetes can fit containers onto your nodes to make the best use of your resources.
  - Kubernetes automatically schedules containers based on resource needs and constraints, to maximize utilization without sacrificing availability.
- __*Horizontal Scaling*__:
  - With Kubernetes applications are scaled manually or automatically, across the nodes in a cluster, based on CPU or custom metrics utilization.
- __*Management of containerized applications*__
- __*Service Discovery*__ - for discovering too many services, which need to talk to each other.
- __*Load balancing*__:
  - As containers run on different hosts/nodes, which themselves can go down as any container instance, so needs an orchestrator to balance/distribute the load.
  - *<u>Containers receive their own IP addresses</u>* from Kubernetes, while it assigns a __*single Domain Name System (DNS)*__ name *<u>to a set of containers</u>*, *<u>to aid in load-balancing requests</u>*, *<u>across the containers of the set</u>*.
- __*Storage orchestration*__:
  - Kubernetes automatically *<u>mounts</u>* __*software-defined storage (SDS)*__ solutions *<u>to containers</u>* from *<u>local storage</u>*, external *<u>cloud providers</u>*, *<u>distributed storage</u>*, or *<u>network storage systems</u>*.
- __*Automated Rollouts and Rollbacks*__:
  - Kubernetes seamlessly *<u>rolls out</u>* and *<u>rolls back</u>* application *<u>updates</u>* and *<u>configuration changes</u>*, constantly monitoring the application's health to prevent any downtime.
- __*Self-Healing*__:
  - Kubernetes automatically *<u>replaces and reschedules containers from failed nodes</u>*.
  - It kills and *<u>restarts containers unresponsive to health checks</u>*, based on existing *<u>rules/policy</u>*.
  - It also *<u>prevents traffic from being routed to unresponsive containers</u>*.
- __*Secrets and Configuration Management*__:
  - Kubernetes manages *<u>sensitive data and configuration</u>* details for an application *<u>separately from the container image</u>*, in order to avoid a re-build of the respective image.
  - Secrets consist of sensitive/confidential information passed to the application without revealing the sensitive content to the stack configuration, like on GitHub.
- __*Batch Execution*__:
  - Kubernetes supports batch execution, long-running jobs, and replaces failed containers.
- __*RBAC*__:
  - The RBAC API declares four kinds of Kubernetes object: __*Role*__, __*ClusterRole*__, __*RoleBinding*__ and __*ClusterRoleBinding*__.
- Kubernetes approaches the issue of monolithic applications by deploying a large number of small web servers, or microservices.
- In k8s, __*Configuration*__ information is *<u>stored in a JSON</u>* format, but is most often *<u>written in YAML</u>*. Kubernetes agents convert the YAML to JSON prior to persistence to the database.
- Kubernetes is written in __*Go Language*__.

## Architecture of Kubernetes
- 
