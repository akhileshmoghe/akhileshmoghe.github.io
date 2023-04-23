---
title:  "Ways of managing data in Docker Containers"
permalink: /_post/devops/containers/docker-container-storage-volumes
date:   2022-10-22 1:33:22 +0530
categories:
  - Containers
  - Docker
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Containers
  - Docker
author: Akhilesh Moghe
show_author_profile: true
sidebar:
 nav: Containers
---

In Microservices and container orchestrators world, all Containers are essentially *<u>ephemeral</u>* in nature, that means the are not suppose to last long and even if they lasted long, they are not suppose to retain or hold the data in their life cycles. Container orchestrators like *<u>Kubernetes</u>* replaces the crashed or exited container service with just another instance of it. Even if that's how the containers are used, applications themselves needs to maintain it's own state & preserve some kind of application data across the life cycles of the containers either in some databases or on the host file systems. That's where managing the data in and out-side container comes into picture.

- Docker provides two options for containers to store files on the host machine, so that the data or files are persisted even after the container stops/exits: __*<u>Volumes</u>*__, and __*<u>Bind mounts</u>*__.
- Docker also supports containers storing __*<u>files in-memory</u>*__ on the host machine.
    - Such files are not persisted.
    - If you’re running *<u>Docker on Linux</u>*, __*<u>tmpfs mount</u>*__ is used to store files in the host’s system memory.
    - If you’re running *<u>Docker on Windows</u>*, __*<u>named pipe</u>*__ is used to store files in the host’s system memory.

### *<u>Difference between Volumes, Bind mounts & tmpfs mounts</u>*:

![docker-types-of-mounts](/assets/images/devops/container/docker-types-of-mounts.png){:.shadow}

#### *<u>Volumes</u>*:
- *<u>Stored in a part of the host filesystem which is managed by Docker</u>* (/var/lib/docker/volumes/ on Linux).
- A given volume *<u>can be mounted into multiple containers simultaneously</u>*.
- When no running container is using a volume, the volume is still available to Docker and is <u>not removed automatically</u>.
- :x: Non-Docker processes should not modify this part of the filesystem.
- :heavy_check_mark: __*<u>Volumes are the best way to persist data in Docker</u>*__.
- Volumes also support the use of __*<u>volume drivers</u>*__, which allow you to *<u>store your data on remote hosts</u>* or *cloud providers*, among other possibilities.
- Volumes work on both Linux and Windows containers.
- <u>Use-cases</u>:
    - :heavy_check_mark: Sharing data among multiple running containers. Multiple containers can mount the same volume simultaneously, either read-write or read-only.
    - :heavy_check_mark: When you want to store your container’s data on a remote host or a cloud provider, rather than locally.
    - :heavy_check_mark: When you need to *<u>back up, restore, or migrate data from one Docker host to another</u>*. You can stop containers using the volume, then back up the volume’s directory (such as /var/lib/docker/volumes/<volume-name>).
    - :heavy_check_mark: When your application requires fully native file system behavior on Docker Desktop. For example, a database engine requires precise control over disk flushing to guarantee transaction durability.

#### *<u>Bind mounts</u>*:
- May be *<u>stored anywhere on the host system</u>*.
- :warning: They may even be important system files or directories.
- :warning: Non-Docker processes on the Docker host or a Docker container can modify them at any time.
- :warning: <u>cautions</u>:
    - One side effect of using bind mounts, for better or for worse, is that you *<u>can change the host filesystem via processes running in a container</u>*, including creating, modifying, or deleting important system files or directories. This is a powerful ability which *<u>can have security implications</u>*, including impacting non-Docker processes on the host system.
- *<u>Use-cases</u>*:
    - :heavy_check_mark: Sharing configuration files from the host machine to containers. Docker provides DNS resolution to containers by default, by mounting /etc/resolv.conf from the host machine into each container.
    - :heavy_check_mark: Sharing source code or build artifacts between a development environment on the Docker host and a container.

#### *<u>tmpfs mounts</u>*:
- *<u>Stored in the host system’s memory only</u>*, and are never written to the host system’s filesystem.
- It can be *<u>used by</u>* a container *<u>during the lifetime of the container</u>*, to <u>store non-persistent state or sensitive information</u>.
    - When the container stops, the tmpfs mount is removed, and files written there won’t be persisted.
- For instance, internally, swarm services use tmpfs mounts to mount secrets into a service’s containers.
- __*<u>Limitations of tmpfs mounts</u>*__:
    - Unlike volumes and bind mounts, you *<u>can’t share tmpfs mounts between containers</u>*.
    - This functionality is *<u>only available</u>* if you’re running *<u>Docker on Linux</u>*.
- *<u>Use-cases</u>*:
    - :heavy_check_mark: When you do not want the data to persist either on the host machine or within the container. This may be for security reasons or to protect the performance of the container when your application needs to write a large volume of non-persistent state data.
    - :heavy_check_mark: This is useful to temporarily store sensitive files that you don’t want to persist in either the host or the container writable layer.
- *<u>Use a tmpfs mount in a container</u>*:
    - To use a __*<u>tmpfs mount</u>*__ in a container, use the `--tmpfs` flag, or use the `--mount` flag with `type=tmpfs` and `destination` options.
    - with `--mount`:
    ```
    docker run -d -it --name <container-name> --mount type=tmpfs,destination=/<path-in-container> <image>:latest
    ```
        - <u>Optional Flags</u>:
            - Can only be used with `--mount`.
            - `tmpfs-size`:	Size of the tmpfs mount in bytes. Unlimited by default.
            - `tmpfs-mode`: File mode of the tmpfs in octal. For instance, `700` or `0770`. Defaults to `1777` or world-writable.\
&nbsp;
    - with `--tmpfs`:
    ```
    docker run -d -it --name <container-name> --tmpfs /<path-in-container> <image>:latest
    ```



---

### *<u>Create & Manage Volumes</u>*:
- <u>Create a volume</u>:\
```docker volume create <name-of-volume>```
- <u>List volumes</u>:\
```docker volume ls```
- <u>Inspect a volume</u>:\
```docker volume inspect <name-of-volume>```
- <u>Remove a volume</u>:\
```docker volume rm <name-of-volume>```

---

### `--volume` & `--mount` *<u>Docker flags difference</u>*:
- `--mount` is more explicit and verbose. `-v` syntax combines all the options together in one field, while the `--mount` syntax separates them.
- :pencil: *<u>If you need to specify</u>* __*<u>volume driver</u>*__ *<u>options, you must use</u>* `--mount`.
- :pencil: When using __*<u>volumes with services</u>*__, only `--mount` is supported.
- `--mount` consists of multiple key-value pairs, separated by commas and each consisting of a `<key>=<value>` tuple.
    - `type`: Can be `bind`, `volume`, or `tmpfs`.
    - `source` or `src`: For named volumes, this is the name of the volume. For anonymous volumes, this field is omitted.
    - `destination` or `dst` or `target`: Takes as its value the path where the file or directory is mounted in the container.
    - `readonly` or `ro`: If present, causes the bind mount to be mounted into the container as read-only.
        - At other times, the container only needs read access to the data. Remember that multiple containers can mount the same volume, and it can be mounted read-write for some of them and read-only for others, at the same time.
        - by default, `rw`.
    - `volume-opt`: Can be specified more than once, takes a key-value pair consisting of the option name and its value.
- *<u>Examples</u>*: *<u>Create volume while Starting the container</u>*
    - with `--mount`:
    ```
    $ docker run -d --name <container-name> --mount source=<volume-name>,target=/<destination-path-in-container> <image>:latest
    ```
    - with `--volume`:
    ```
    $ docker run -d --name <container-name> -v <volume-name>:/<destination-path-in-container> <image>:latest
    ```
    - Clean up the containers and volumes:
    ```
    $ docker container stop devtest
    $ docker container rm devtest
    $ docker volume rm myvol2
    ```

---

### *<u>Using volume with Docker Compose</u>*:
- Using Docker Compose service with a volume:
- On the first invocation of docker-compose up the volume will be created. The same volume will be reused on following invocations.
```
services:
  frontend:
    image: node:lts
    volumes:
      - myapp:/home/node/app
volumes:
  myapp:
```
- Referencing the already created volume inside docker-compose.yml as follows:
```
services:
  frontend:
    image: node:lts
    volumes:
      - myapp:/home/node/app
volumes:
  myapp:
    external: true
```
- Use a `bind mount` with Docker Compose:
```
version: "3.9"
services:
  frontend:
    image: node:lts
    volumes:
      - type: bind
        source: ./static
        target: /opt/app/static
volumes:
  myapp:
```

---

### *<u>Volume drivers to share data among machines or cloud providers</u>*:
- When building fault-tolerant applications, you might need to configure multiple replicas of the same service to have access to the same files.
- There are several ways to achieve this when developing your applications. One is to add logic to your application to store files on a cloud object storage system like Amazon S3. Another is to create volumes with a driver that supports writing files to an external storage system like NFS or Amazon S3.
- Volume drivers allow you to abstract the underlying storage system from the application logic. For example, if your services use a volume with an NFS driver, you can update the services to use a different driver, as an example to store data in the cloud, without changing the application logic.
- <u>Further Reading</u>:
    - [Use a volume driver](https://docs.docker.com/storage/volumes/#use-a-volume-driver)

---

###### References:
- [Docker: How to access files from another container from a given container?](https://stackoverflow.com/questions/63177219/docker-how-to-access-files-from-another-container-from-a-given-container)
- [Docker Volumes](https://docs.docker.com/storage/volumes/)

---
