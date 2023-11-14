---
title:  "Why Docker Compose Safeguards Container Runtime Changes Through Host Reboots: A Comprehensive Exploration"
date:   2023-07-12 12:33:22 +0530
permalink: /_posts/containers/container_lifecycle_with_docker_compose
categories:
  - Containers
  - Docker
  - Docker-Compose
  - DevOps
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Containers
  - Docker
  - Docker-Compose
  - DevOps
author: Akhilesh Moghe
show_author_profile: true
sidebar:
 nav: Containers
---

In the container's lifecycle, a container undergoes various stages from creation to deletion, mainly __*Created*__, __*Running*__, __*Paused*__, __*Stopped*__, and __*Deleted*__. Managing containers involves considering different conditions for creation/recreation, restarting, or deletion based on specific requirements & conditions. In a microservices architecture, where multiple containers coexist, the decision to recreate a running container or simply restarting it can significantly impact the overall performance of the deployment or application.\
&nbsp;
Containers can be created and started using the Docker CLI but they can also be efficiently managed through orchestration tools such as Docker Compose. These tools are designed to optimize the container lifecycle, incorporating predefined rules for efficient container management. Whether using the Docker CLI or orchestration tools, understanding these rules and the underlying mechanisms is crucial for effective containerization. Docker Compose, as a powerful orchestration tool, streamlines the process of container management, ensuring a seamless and optimized experience for users. Exploring the intricacies of these tools and their hidden efficiencies can enhance your overall container orchestration strategy.\
&nbsp;
While leveraging orchestration tools like Docker Compose, discrepancies may arise between our anticipated container state in a given situation and the actual state managed by these tools. Such mismatches can lead to unexpected behaviors and operational challenges.\
&nbsp;
In this concise blog, I'll shed light on a specific condition I encountered during my Docker Compose usage, aiming to clarify and provide insights into handling such scenarios effectively.

---

## Container lifecycle Transitions Illustration:
- Below is a fine representation of the Container lifecycle, this is how the container transitions from creation to deletion.
![docker-containers-lifecycle-stages](/assets/images/devops/container/docker-containers-lifecycle-stages.png)

---

## Unraveling the Confusion: Misunderstanding due to one of Docker Compose's Feature
- We know, Containers are ephemeral, they are not suppose to preserve the data or the changes to the filesystem after running. Also, any tempororily copied files should not be expected to be there during the container restarts.
- That's fine, but while working with Docker Compose, I ran into a situation where it was evident that during multiple restarts of the container, the container was able to preserve the runtime changes to it's filesystem and data.
- The extent of the conceptual misunderstanding became more complex and hampering, when I even restarted the host device on which the Docker compose and the containers were running. During multiple reboots also the restarted container had preserved the filesystem and data changes.
- This was completely out-of-line about what I had learnt about container lifecycle, that on restarts, the temporory changes made in a container will no longer be there.
- But with Docker Compose, I was observing a completely different behaviour of containers. Chnages to the container persists during restarts of container.
- HOW????

---

## Decoding the Source of Confusion: Understanding the Docker Compose Feature Behind the Misunderstanding
- [Only recreate containers that have changed](https://docs.docker.com/compose/features-uses/#only-recreate-containers-that-have-changed)
  - Docker Compose caches the configuration used to create a container.
  - When you restart a service that has not changed, Compose re-uses the existing containers.
  - Re-using containers means that you can make changes to your environment very quickly.
- It's clear from above documentation, that Docker compose does not create the container from the container images everytime. It preserves the containers during multiple invocations of the containers.
- This is why the temporary changes made to the container are available even after container and the host it is running on are restarted multiple times.

---

## <u>docker compose up</u> behaviour and options
- Docker Compose __Builds__, *(re)*__creates__, __starts__, and __attaches__ to containers for a service.
- Unless they are already running, this command also __starts any linked services__.
- <u>By default, Docker Compose does not re-create the existing containers for a service</u>.
- Docker Compose <u>only re-creates a container if the service’s configuration or image was changed after the container’s creation</u>. `docker compose up` picks up the changes by stopping and recreating the containers (preserving mounted volumes).
- <u>Options for </u>`docker compose up`:
  - `--force-recreate`:
    - If you want to force Compose to stop and recreate all containers, use the `--force-recreate` flag.
  - `--no-recreate`:
    - `docker compose up` picks up the changes by stopping and recreating the containers (preserving mounted volumes). To prevent Compose from picking up changes, use the `--no-recreate` flag.
  - `--always-recreate-deps`
    - Recreate dependent containers.
    - This option is incompatible with `--no-recreate`.


### Other options for docker compose up
- `--abort-on-container-exit`\
  Stops all containers if any container was stopped.\
  Incompatible with `-d`
- `--attach`\
  Attach to service output.
- `--attach-dependencies`\
  Attach to dependent containers.
- `--build`\
  Build images before starting containers.
- `--detach` , `-d`\
  Detached mode: Run containers in the background
- `--exit-code-from`\
  Return the exit code of the selected service container.\
  Implies `--abort-on-container-exit`
- `--force-recreate`\
  Recreate containers even if their configuration and image haven’t changed.
- `--no-attach`\
  Don’t attach to specified service.
- `--no-build`\
  Don’t build an image, even if it’s missing.
- `--no-color`\
  Produce monochrome output.
- `--no-deps`\
  Don’t start linked services.
- `--no-log-prefix`\
  Don’t print prefix in logs.
- `--no-recreate`\
  If containers already exist, don’t recreate them.\
  Incompatible with `--force-recreate`.
- `--no-start`\
  Don’t start the services after creating them.
- `--pull`\
  Missing	Pull image before running (“always”,”missing”,”never”)
- `--quiet-pull`\
  Pull without printing progress information.
- `--remove-orphans`\
  Remove containers for services not defined in the Compose file.
- `--renew-anon-volumes` , `-V`\
  Recreate anonymous volumes instead of retrieving data from the previous containers.
- `--scale`\
  Scale SERVICE to NUM instances.\
  Overrides the scale setting in the Compose file if present.
- `--timeout` , `-t`\
  Use this timeout in seconds for container shutdown when attached or when containers are already running.
- `--timestamps`\
  Show timestamps.
- `--wait`\
  Wait for services to be running|healthy.\
  Implies detached mode.
- `--wait-timeout`\
  Timeout waiting for application to be running|healthy.
- `--dry-run`\
  Execute command in dry run mode

---

###### References:
- [Docker Compose: Key features and use cases](https://docs.docker.com/compose/features-uses/)
- [docker compose up](https://docs.docker.com/engine/reference/commandline/compose_up/#:~:text=If%20there%20are%20existing%20containers,the%20%2D%2Dno%2Drecreate%20flag.)
- [Docker Architecture, Life Cycle of Docker Containers and Data Management](https://dev.to/docker/docker-architecture-life-cycle-of-docker-containers-and-data-management-1a9c)