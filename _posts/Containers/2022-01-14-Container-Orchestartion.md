---
title:  "Container Orchestration"
date:   2022-01-14 12:33:22 +0530
permalink: /_posts/containers/container_orchestration
categories:
  - Containers
  - Docker
  - Kubernetes
  - DevOps
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Containers
  - Docker
  - Kubernetes
  - DevOps
author: Akhilesh Moghe
show_author_profile: true
sidebar:
 nav: Containers
---

## What are Containers?
- Containers are an application-centric method to deliver high-performing, scalable applications on any infrastructure of your choice.
- Containers are best suited to deliver *<u>microservices</u>* by providing *<u>portable</u>*, *<u>isolated virtual environments</u>* for applications to run *<u>without interference from other running applications</u>*.
- Containers encapsulate microservices and their dependencies but do not run them directly. Containers run container images.
- A __*container image*__ bundles the *<u>application</u>* along with its *<u>runtime</u>*, *<u>libraries</u>*, and *<u>dependencies</u>*, and it represents the source of a container deployed to offer an isolated executable environment for the application.
- Containers can be deployed from a specific image on many platforms, such as workstations, Virtual Machines, public cloud, etc.


## Container Orchestration
- Container runtimes like [runC](https://opensource.com/life/16/8/runc-little-container-engine-could), [containerd](https://containerd.io/), or [cri-o](https://cri-o.io/) we can use those pre-packaged images, to create one or more containers. All of these runtimes are good at running containers on a single host.
- Container orchestrators are tools which *<u>group systems together to form clusters where containers' deployment and management is automated at scale</u>* while meeting the requirements mentioned below:
  - Fault-tolerance
  - On-demand scalability
  - Optimal resource usage
  - Auto-discovery to automatically discover and communicate with each other
  - Accessibility from the outside world
  - Seamless updates/rollbacks without any downtime.
- A few different container orchestration tools and services:
  - [Amazon Elastic Container Service (ECS)](https://aws.amazon.com/ecs/)
  - [Azure Container Instances](https://azure.microsoft.com/en-us/services/container-instances/)
  - [Azure Service Fabric](https://azure.microsoft.com/en-us/services/service-fabric/)
  - [Kubernetes](https://kubernetes.io/)
    - Kubernetes is an open source orchestration tool, originally started by Google, today part of the Cloud Native Computing Foundation (CNCF) project.
  - [Marathon](https://mesosphere.github.io/marathon/)
    - Marathon is a framework to run containers at scale on [Apache Mesos](https://mesos.apache.org/).
  - [Nomad](https://www.nomadproject.io/)
    - Nomad is the container and workload orchestrator provided by [HashiCorp](https://www.hashicorp.com/).
  - [Docker Swarm](https://docs.docker.com/engine/swarm/)
    - Docker Swarm is a container orchestrator provided by Docker, Inc. It is part of [Docker Engine](https://docs.docker.com/engine/).
    
## Need for Container Orchestration
- Although we can manually maintain a couple of containers or write scripts to manage the lifecycle of dozens of containers, orchestrators make things much easier for operators especially when it comes to managing hundreds and thousands of containers running on a global infrastructure.
- Most container orchestrators can:
  - *<u>Group hosts together</u>* while creating a *<u>cluster</u>*.
  - Schedule containers to run on hosts in the cluster based on *<u>resources availability</u>*.
  - Enable containers in a cluster to *<u>communicate with each other regardless of the host</u>* they are deployed to in the cluster.
  - Bind containers and *<u>storage resources</u>*.
  - *<u>Group sets of similar containers</u>* and bind them to *<u>load-balancing</u>* constructs to simplify access to containerized applications by creating *<u>a level of abstraction between the containers and the user</u>*.
  - Manage and *<u>optimize resource usage</u>*.
  - Allow for implementation of *<u>policies to secure access to applications running inside containers</u>*.


## Container Orchestrator Deployments
- Most container orchestrators can be deployed on bare metal, Virtual Machines, on-premises, on public and hybrid cloud.
- Kubernetes, for example, can be deployed on a workstation, with or without a local hypervisor such as Oracle VirtualBox, inside a company's data center, in the cloud on AWS Elastic Compute Cloud (EC2) instances, Google Compute Engine (GCE) VMs, DigitalOcean Droplets, OpenStack, etc.
- There are *<u>ready-to-use solutions</u>* which allow Kubernetes clusters to be installed, with only a few commands, on top of cloud Infrastructures-as-a-Service, such as GCE, AWS EC2, Docker Enterprise, IBM Cloud, Rancher and multi-cloud solutions through IBM Cloud Private or StackPointCloud.
- Also there is the *<u>managed container orchestration as-a-Service</u>*, more specifically the managed Kubernetes as-a-Service solution, offered and hosted by the major cloud providers, such as:
  - [Amazon Elastic Kubernetes Service (Amazon EKS)](https://aws.amazon.com/eks/)
  - [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/en-us/services/kubernetes-service/)
  - [DigitalOcean Kubernetes](https://www.digitalocean.com/products/kubernetes/)
  - [Google Kubernetes Engine (GKE)](https://cloud.google.com/kubernetes-engine/)
  - [IBM Cloud Kubernetes Service](https://www.ibm.com/cloud/container-service)
  - [Oracle Container Engine for Kubernetes](https://cloud.oracle.com/containers/kubernetes-engine)
  - [VMware Tanzu Kubernetes Grid](https://tanzu.vmware.com/kubernetes-grid)
  

