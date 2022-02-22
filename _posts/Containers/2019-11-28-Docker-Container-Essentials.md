---
title:  "Docker Containers Essentials"
permalink: /_post/devops/containers/docker-container-essentials
date:   2019-11-28 1:33:22 +0530
categories:
  - DevOps
  - Containers
  - Docker
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - DevOps
  - Containers
  - Docker
author: Akhilesh Moghe
show_author_profile: true
---

## Linux Kernel Namespaces & Control Groups
- Containers use Linux __*namespaces*__ to provide *<u>isolation of system resources from other containers or the host</u>*.
- The PID namespace provides isolation for process IDs. If you run top while inside the container, you will notice that it shows the processes within the PID namespace of the container, which is much different than what you can see if you ran top on the host.
- Containers use kernel-level features to achieve isolation and they run on top of the kernel.
- Containers are just a group of processes running in isolation on the same host.
- Linux namespaces include:
  - __*PID*__: Processes Isolation
  - __*MNT*__: Mount and unmount *<u>directories</u>* without affecting other namespaces.
  - __*NET*__: Containers have their own *<u>network stack</u>*.
  - __*IPC*__: Isolated interprocess communication mechanisms such as *<u>message queues</u>*.
  - __*User*__: Isolated view of users on the system.
  - __*UTC*__: Set *<u>hostname and domain name</u>* per container.
- These namespaces provide the isolation for containers that allow them to run together securely and without conflict with other containers running on the same system.
- Containers are self-contained and isolated, which means you can avoid potential conflicts between containers with different system or runtime dependencies. Isolation benefits are possible because of Linux namespaces.
  - For example, you  can deploy an app that uses Java 7 and another app that uses Java 8 on the same host.
  - Or you can run multiple NGINX containers that all have port 80 as their default listening ports. (If you're exposing on the host by using the --publish flag, the ports selected for the host must be unique.)

## Containers on Windows and Mac:
- Namespaces are a feature of the Linux kernel, so to run the Docker Containers on Windows and Mac, Docker runtime has a Linux subsystem included which is now open-sourced as [LinuxKit](https://github.com/linuxkit/linuxkit) project. Being able to run containers on many different platforms is one advantage of using the Docker tooling with containers.
- In addition to running Linux containers on Windows by using a Linux subsystem, __*native Windows containers*__ are now possible because of the creation of *<u>container primitives on the Windows operating system</u>*.
- Native Windows containers can be run on *<u>Windows 10</u>* or *<u>Windows Server 2016</u>* or later.

- The container does not have its own kernel. It uses the kernel of the host and the Ubuntu image is used only to provide the file system and tools available on an Ubuntu system.

## Docker Layer Caching:
- Layers that change frequently, such as copying source code into the image, should be placed near the bottom of the file to take full advantage of the __*<u>Docker layer cache</u>*__.
- This allows you to avoid rebuilding layers that could otherwise be cached.
- For instance, if there was a change in the `FROM` instruction, it will *<u>invalidate the cache for all subsequent layers of the image</u>*.
- There is a *<u>caching mechanism in place for pushing layers too</u>*. Docker Hub already has all but one of the layers from an earlier push, so it only pushes the one layer that has changed.
- When you change a layer, every layer built on top of that will have to be rebuilt.
- Each line in a Dockerfile builds a new layer that is built on the layer created from the lines before it. This is why the order of the lines in your Dockerfile is important.
- You can optimized your Dockerfile so that the layer that is most likely to change is the last line of the Dockerfile. Generally for an application, your code changes at the most frequent rate.
- This optimization is particularly important for CI/CD processes where you want your automation to run as fast as possible.

## Dockerfile
- The Dockerfile is used to *<u>create reproducible builds</u>* for your application.
- A common workflow is to have your CI/CD automation run `docker image build` as part of its build process.
- After images are built, they will be sent to a central registry where they can be accessed by all environments (such as a test environment) that need to run instances of that application.

### Dockerfile Commands:
- __FROM__:
  - Every Dockerfile typically starts with a `FROM` line that is the starting image to build container layers on top of.
  - Selecting a smaller base image means it will download (deploy) much faster, and it is also more secure because it has a smaller attack surface.
  - Used to fetch image from registry.
  - Only one `FROM` command is allowed in a Docker file.
  - You can pull common lines of multiple Dockerfiles into a base Dockerfile, which you can then point to with each of your child Dockerfiles by using the `FROM` command.
  - `FROM` must be the *<u>1st line is Docker file</u>*.
  - Example: `FROM python:3.6.1-alpine`
    - `3.6.1-alpine` is a tag for the Python image.
    - It is a [best practice](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) to use a __*<u>specific tag</u>*__ when inheriting a parent image so that changes to the parent dependency are controlled.
    - If no tag is specified, the latest tag takes effect, which acts as a dynamic pointer that points to the latest version of an image.
- __COPY__:
  - Copy files from Host machine to the container.
  - This line copies the files kept in the local directory (where you will run docker image build) into a new layer of the image.
  - This instruction is the *<u>last line in the Dockerfile</u>*.
  - Layers that change frequently, such as copying source code into the image, should be placed near the bottom of the file to take full advantage of the __*<u>Docker layer cache</u>*__.
- __WORKDIR__:
  - Define the working directory within the container
- __RUN__:
  - Execute command within the container.
  - The `RUN` command executes commands needed to set up your image for your application, such as installing packages, editing files, or changing file permissions.
  - The `RUN` commands are executed at build time and are added to the layers of your image.
- __ENV__:
  - To set environmental variables.
- __CMD__:
  - *<u>Command to execute when run command is provided on the image to start the container</u>*.
  - `CMD` is the command that is executed when you start a container.
  - There can be *<u>only one `CMD` per Dockerfile</u>*. If you specify more than one CMD, then the last CMD will take effect.
- __LABEL__:
  - To provide metadata to the image.
- __EXPOSE__:
  - To expose container port.
- Further Reading:
  - [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)


## Union File System & Copy-on-Write
- Each of the lines in a Dockerfile is a layer.
- Each layer contains only the delta, or changes from the layers before it. To put these layers together into a single running container, Docker uses the __*<u>Union File System</u>*__ to overlay layers transparently into a single view.
- <u>Each layer of the image is read-only </u>*<u>except for the top layer</u>*, which is created for the container.
- The read/write container layer implements __*<u>copy-on-write</u>*__, which means that *<u>files that are stored in lower image layers are pulled up to the read/write container layer only when edits are being made to those files</u>*. Those changes are then stored in the container layer.
- The __*<u>copy-on-write</u>*__ function is very fast and in almost all cases, does not have a noticeable effect on performance. You can *<u>inspect which files have been pulled up to the container level with the </u>*[__*<u>docker diff</u>*__](https://docs.docker.com/engine/reference/commandline/diff/)*<u> command</u>*.

  ![container_layers](/assets/images/devops/container/container_layers.png){:.shadow}

- Because *<u>image layers are read-only, they can be shared by images and by running containers</u>*.
- For example, creating a new Python application with its own Dockerfile with similar base layers will share all the layers that it had in common with the first Python application.

```
  FROM python:3.6.1-alpine
  RUN pip install flask
  CMD ["python","app2.py"]
  COPY app2.py /app2.py
```

  ![container_layers_shared_example](/assets/images/devops/container/container_layers_shared_example.png){:.shadow}

- Because the containers use the same read-only layers, you can imagine that starting containers is very fast and has a very low footprint on the host.
- Image layering enables the docker caching mechanism for builds and pushes.

## Docker Swarm:
- Docker Swarm is the orchestration tool that is built in to the Docker Engine.
- When a node in the swarm goes down, it might take down running containers with it. The swarm will recognize this loss of containers and will attempt to reschedule containers on available nodes to achieve the desired state for that service.

### *<u>Initialize the swarm</u>* on node 1:
  ```
  docker swarm init --advertise-addr eth0
  ```
  - The `--advertise-addr` option specifies the address which the other nodes will use to join the swarm.
  - This docker swarm init command generates a join token. The token makes sure that no malicious nodes join the swarm. You need to use this token to join the other nodes to the swarm.

### *<u>Create a three-node swarm</u>*
- on both node2 and node3, copy and run the `docker swarm join` command that was outputted to your console by the last command.
- Back on node1, run docker node ls to verify your three-node cluster:
  ```
  $ docker node ls
  
  ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
  7x9s8baa79l29zdsx95i1tfjp     node3               Ready               Active
  x223z25t7y7o4np3uq45d49br     node2               Ready               Active
  zdqbsoxa6x1bubg3jyjdmrnrn *   node1               Ready               Active              Leader
  ```
  - This command outputs the three nodes in your swarm. The asterisk (*) next to the ID of the node represents the node that handled that specific command (docker node ls in this case).
  - Your node consists of one manager node and two workers nodes. Managers handle commands and manage the state of the swarm. Workers cannot handle commands and are simply used to run containers at scale. By default, managers are also used to run containers.
- Although you control the swarm directly from the node in which its running, you can __*<u>control a Docker swarm remotely</u>*__ *<u>by connecting to the Docker Engine of the manager by using the </u>*__*<u>remote API</u>*__ or *<u>by activating a </u>*__*<u>remote host</u>*__*<u> from your local Docker installation</u>* (using the `$DOCKER_HOST` and `$DOCKER_CERT_PATH` environment variables). This will become useful when you want to remotely control production applications, instead of using SSH to directly control production servers.

### *<u>Deploy first service on Swarm</u>*
- To run containers on a Docker Swarm, you need to create __*a service*__.
- A service is an abstraction that *<u>represents multiple containers of the same image</u>* deployed across a distributed cluster.
- Example: Deploy a NGINX service:
  ```
  $ docker service create --detach=true --name nginx1 --publish 80:80  --mount source=/etc/hostname,target=/usr/share/nginx/html/index.html,type=bind,ro nginx:1.12
pgqdxr41dpy8qwkn6qm7vke0q
  ```
  - This *<u>command statement is </u>*__*<u>declarative language</u>*__, and *<u>Docker Swarm will try to maintain the state declared in this command unless explicitly changed by another docker service command</u>*. This behavior is useful when nodes go down, for example, and containers are automatically rescheduled on other nodes.
  - The `--mount` flag is useful to have NGINX *<u>print out the hostname of the node it's running on</u>*, useful when you start load balancing between multiple containers of NGINX that are distributed across different nodes in the cluster and you want to see which node in the swarm is serving the request.
  - The `--publish` command uses the *<u>swarm's built-in </u>*__*<u>routing mesh</u>*__. In this case, port 80 is exposed on every node in the swarm. The routing mesh will route a request coming in on port 80 to one of the nodes running the container.

### *<u>Inspect the service</u>*
- Use the command `docker service ls` to inspect the service you just created:
  ```
  $ docker service ls
  ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
  pgqdxr41dpy8        nginx1              replicated          1/1                 nginx:1.12          *:80->80/tcp
  ```

### *<u>Check the running container of the service</u>*
- To take a deeper look at the running tasks, use the command `docker service ps`.
- __*<u>A task</u>*__*<u> is another abstraction in Docker Swarm that represents the running instances of a service</u>*.
- In this case, there is a 1-1 mapping between a task and a container.
  ```
  $ docker service ps nginx1
  ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE           ERROR               PORTS
  iu3ksewv7qf9        nginx1.1            nginx:1.12          node1               Running             Running 8 minutes ago
  ```
- If you know which node your container is running on (you can see which node based on the output from `docker service ps`), you can use the command `docker container ls` to see the container running on that specific node.

### *<u>Test the service</u>*
- Because of the __*<u>routing mesh</u>*__, you can *<u>send a request to any node of the swarm on port 80</u>*. This request will be *<u>automatically routed to the one node that is running the NGINX container</u>*.
- Try this command on each node:
  ```
  $ curl localhost:80
  node1
  ```
- Curling will output the *<u>hostname where the container is running</u>*. For this example, it is running on node1, but yours might be different.

### *<u>Scale the service with service update</u>*
- Use the `docker service` command to update the NGINX service to include 5 replicas. This is defining a new state for the service.
  ```
  $ docker service update --replicas=5 --detach=true nginx1
  nginx1
  ```
- When this command is run, the following events occur:
  - The state of the service is updated to 5 replicas, which is stored in the swarm's internal storage.
  - Docker Swarm recognizes that the number of replicas that is scheduled now does not match the declared state of 5.
  - Docker Swarm schedules 5 more tasks (containers) in an attempt to meet the declared state for the service.
- This swarm is actively checking to see if the desired state is equal to actual state and will attempt to reconcile if needed.
- After a few seconds, you should see that the swarm did its job and successfully started 9 more containers. Notice that the containers are scheduled across all three nodes of the cluster. The default placement strategy that is used to decide where new containers are to be run is the emptiest node, but that can be changed based on your needs.
  ```
  $ docker service ps nginx1
  ```
  ![docker-swarm-scale-services](/assets/images/devops/container/docker-swarm-scale-services.png)

### *<u>Test the service scaling</u>*
- The `--publish 80:80` parameter is still in effect for this service; that was not changed when you ran the docker service update command. However, now when you send requests on port 80, the routing mesh has multiple containers in which to route requests to. The routing mesh acts as a load balancer for these containers, alternating where it routes requests to.
- Try it out by curling multiple times. Note that it doesn't matter which node you send the requests. There is no connection between the node that receives the request and the node that that request is routed to.
- You should see which node is serving each request because of the useful `--mount` command you used earlier.
  ```
  $ curl localhost:80
  node3
  $ curl localhost:80
  node3
  $ curl localhost:80
  node2
  $ curl localhost:80
  node1
  $ curl localhost:80
  node1
  ```

### *<u>Limits of the routing mesh</u>*
- :warning: The routing mesh can publish only one service on port 80.
- If you want multiple services exposed on port 80, you can use an external __*application load balancer*__ *<u>outside of the swarm</u>* to accomplish this.

### *<u>Check the aggregated logs for the service</u>*
- Another easy way to see which nodes those requests were routed to is to check the aggregated logs.
- You can get aggregated logs for the service by using the command `docker service logs [service name]`.
- This aggregates the output from every running container, that is, the output from `docker container logs [container name]`.
  ```
  $ docker service logs nginx1
  ```
  ![docker-swarm-scale-services-2](/assets/images/devops/container/docker-swarm-scale-services-2.png)
- Based on these logs, you can see that each request was served by a different container.
- In addition to seeing whether the request was sent to node1, node2, or node3, you can also see which container on each node that it was sent to. For example, nginx1.5 means that request was sent to a container with that same name as indicated in the output of the command `docker service ps nginx1`.

### *<u>Apply rolling updates to Swarm</u>*
- Run the `docker service update` command to trigger a rolling update of the swarm.
  ```
  $ docker service update --image nginx:1.13 --detach=true nginx1
  ```
- fine-tune the rolling update by using these options:
  - `--update-parallelism`:
    - Specifies the number of containers to update immediately (defaults to 1).
  - `--update-delay`:
    - Specifies the delay between finishing updating a set of containers before moving on to the next set.
- After a few seconds, run the command `docker service ps nginx1` to see all the images that have been updated to `nginx:1.13`.
  ```
  $ docker service ps nginx1
  ```
  ![docker-swarm-rolling_updates](/assets/images/devops/container/docker-swarm-rolling_updates.png)

### *<u>Remove the node from the swarm cluster</u>*
- A typical way to leave the swarm:
  ```
  $ docker swarm leave
  ```

### *<u>Number of nodes in a Docker Swarm</u>*
- *<u>Manager nodes implement the </u>*__*<u>raft consensus algorithm</u>*__, which requires that *<u>more than 50% of the nodes agree on the state</u>* that is being stored for the cluster.
- :warning: If you don't achieve more than 50% agreement, the swarm will cease to operate correctly. For this reason, note the following guidance for node failure tolerance:
  - Three manager nodes tolerate one node failure.
  - Five manager nodes tolerate two node failures.
  - Seven manager nodes tolerate three node failures.
- You should have at least three manager nodes but typically no more than seven. However, the more manager nodes you have, the harder it is to achieve a consensus on the state of a cluster.
- *<u>Worker nodes can scale up into the thousands of nodes</u>*. Worker nodes communicate by using the __*<u>gossip protocol</u>*__, which is optimized to be perform well under a lot of traffic and a large number of nodes.

## Docker Hub:
- The [Docker Hub](https://hub.docker.com/) is the *<u>public central registry for Docker images</u>*. Anyone can share images here publicly. The Docker Hub contains community and official images.
- Hub images include content that has been *<u>verified and scanned for security vulnerabilities by Docker</u>*.
- __*Certified Official images*__ that are deemed enterprise-ready and are tested with Docker Enterprise Edition.
  - [Docker Official Images](https://hub.docker.com/u/library)
  - [docker-library/official-images](https://github.com/docker-library/official-images)
  - [docker-library/docs](https://github.com/docker-library/docs)
    - [Nginx](https://hub.docker.com/_/nginx)
    - [mongo](https://hub.docker.com/_/mongo)
- It is important to avoid using unverified content from the Docker Hub when you develop your own images that are intended to be deployed into the production environment. These unverified images might contain security vulnerabilities or possibly even malicious software.
