---
title:  "Microservices Introduction"
date:   2022-01-11 12:33:22 +0530
permalink: /_posts/system_design_architecture/microservices
categories:
  - Microservices
  - Docker
  - Containers
  - Architecture & System Design
  - Cloud Computing
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Microservices
  - Docker
  - Containers
  - Architecture & System Design
  - Cloud Computing
author: Akhilesh Moghe
show_author_profile: true
sidebar:
 nav: Containers
---

## Monolithic Applications
- Is a large, single piece of software which continuously grows,
- Has to run on a single system which has to satisfy its compute, memory, storage, and networking requirements.
- Runs as a single process.
- Scaling of individual features of the monolith is almost impossible. However, scaling the entire application can be achieved by manually deploying a new instance of the monolith on another server, typically behind a load balancing appliance.
- During upgrades, patches or migrations of the monolith application downtime is inevitable and maintenance windows have to be planned well in advance. While there are solutions to minimize downtime by setting up monolith applications in a highly available active/passive configuration, they introduce new challenges to keep all systems at the same patch level and may introduce new possible licensing costs.

## Modern Microservice Applications
- They are usually carved out of the monolith legacy applications or monolithic approach, separated from one another, becoming distributed components each described by a set of specific characteristics.
- Usually loosely coupled microservices, each performing a specific business function.
- Deployed individually on separate servers provisioned with fewer resources - only what is required by each service and the host system itself, helping to lower compute resource expenses.
- Microservices-based architecture is aligned with __*Event-driven Architecture*__ and __*Service-Oriented Architecture (SOA)*__  principles, where complex applications are composed of *<u>small independent processes</u>* which *<u>communicate with each other through APIs</u>* over a network.
- Each microservice is developed and written in a modern programming language, selected to be the best suitable for the type of service and its business function. That means, different services can be written in different programming languages and they can interact with the APIs exposed by each service.
- One of the greatest benefits of microservices is __*scalability*__.
- With microservices architecture, the overall application becomes modular and each microservice can be scaled individually, either manually or automated through demand-based autoscaling.
- Seamless upgrades and patching processes are possible with microservices architecture. There is virtually no downtime and no service disruption as upgrades are usually rolled out - one service at a time.
- No need to re-compile, re-build and re-start an entire monolithic application. As a result, rolling-out new features and updates becomes a lot faster.
- Separate teams can focus on separate features, bringing agility in more productive and cost-effective development.

### Stateless Vs Stateful Microservices
- Microservices can be __*Stateless*__ or __*Stateful*__.
- Stateless indicates microservice does not preserve any session data, or any other memory of previous events, between consecutive transaction of same or differnt instance.
- In stateless, every transaction happens as if it had been the first one.
- Stateful microservices store session data, or logs of every interction in some database.

## Migration from Monolith to Microservices
- Usually an Incremental Refactorig approach is preferred instead of a complete re-development at once as a big-bang.
- An incremental refactoring approach makes it possible to implement new features as modern microservices which are able to communicate with the monolith through APIs, without touching the monolith's code. In the meantime, refactored old features makes way to replace monolithic application completely over the migration period. This incremental approach offers a gradual transition from a legacy monolith to modern microservices architecture and allows for phased migration of application features into the cloud.
- Few of the applications might not be right candidate for migration to Microservices like Mainframe applications written in older programming lamguages like Cobol or Assembler. Best way is to rebuild such application as new cloud native applications.

## Why Containerize the Microservices
- Choosing runtimes is one of the challenge in microservices architecture, usually different runtimes like Java, NodeJS, Python co-exists.
- Another problem is deploying many modules on a single physical or virtual server, chances are that different libraries and runtime environment may conflict with one another causing errors and failures.
- Application containers are the solution, providing encapsulated lightweight runtime environments for application modules.
- Containers promise consistent software environments for developers, testers, all the way from Development to Production.
- Wide support of containers ensured application portability from physical bare-metal to Virtual Machines.
- Multiple applications can be deployed on the very same server, each running in their own execution environments isolated from one another, thus avoiding conflicts, errors, and failures.
- Few other features of containerized application environments are higher server utilization, individual module scalability, flexibility, interoperability and easy integration with automation tools.
