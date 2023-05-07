---
title:  "Enhancing Container Security: A Guide to Runtime Privileges and Linux Capabilities in Docker Containers"
permalink: /_posts/devops/containers/docker-container-runtime-privileges-linux-capabilities
date:   2023-04-19 1:33:22 +0530
categories:
  - Containers
  - Docker
  - Linux
  - DevOps
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Containers
  - Docker
  - Linux
  - DevOps
author: Akhilesh Moghe
show_author_profile: true
sidebar:
 nav: Containers
---

Docker containers are a popular way to package and deploy applications, but they can also pose security risks if not properly configured. One important aspect of container security is managing runtime privileges - that is, controlling the level of access and permissions that a container has when it is running.\
In Linux, one way to manage runtime privileges is through the use of Linux Capabilities. Capabilities are a granular way of assigning privileges to processes, allowing them to perform certain privileged operations without needing to run as the root user. This helps to reduce the attack surface of a system and improve security.\
In this blog, we'll explore the concept of runtime privileges and Linux capabilities in the context of Docker containers. We'll look at some common capabilities and how they can be used to enhance container security. We'll provide guidance on how to use Linux capabilities to grant additional privileges to Docker containers in a secure and controlled manner. Whether you're new to Docker security or an experienced user, this blog will provide valuable insights into managing runtime privileges and permissions in containerized environments.

## Options to run containers with priviliged access
- By default, Docker containers run as "__<u>unprivileged</u>__", which means they lack device access, preventing certain processes like Docker daemon from running within them.
- Easiest but most unsecured way to enable access to all devices for a container is to run the container with `--privileged` flag.
- The `--privileged` flag gives all capabilities to the container and enables access to all devices on the host as well as set some configuration in *AppArmor* or *SELinux*. This allows the container nearly all the same access to the host as processes running outside containers on the host.
- Other options which enable user to provide better granular access to running containers are explained in detail further in this blog with few examples:

|---
| Option | Description
|-|-
| `--cap-add` | :small_blue_diamond: Add Linux capabilities
| `--cap-drop` | :small_blue_diamond: Drop Linux capabilities
| `--privileged` | :small_blue_diamond: Give extended privileges to this container
| `--device=[]` | :small_blue_diamond: Allows you to run devices inside the container without the `--privileged` flag.

## Provide Host devices access to Containers
- In a Linux system, devices refer to physical or virtual components that can be accessed through the operating system. Types of devices: 
  - Character devices: These devices provide a stream of data in a sequential manner. Examples of character devices include Keyboards, Mouse, Serial Ports, and Audio devices.
  - Block devices: These devices provide access to fixed-size blocks of data, and are often used for storage devices such as hard disks, SSDs, and USB drives.
  - Network devices: which allow for communication with other devices on a network, like Network Interface Cards (NICs), Modems, Routers, Switches, Bridges.
  - Virtual devices: which are created by software for specific purposes like Virtual COM Port.
- By default, containers do not have access to any of the host devices.
- If you want to limit access to a specific device or few devices you can use the `--device` flag. It allows you to specify one or more devices that will be accessible within the container.

```shell
$ docker run --device=/dev/snd:/dev/snd ...
```

- By default, the container will be able to `read`, `write`, and `mknod` these devices.
- But, this can be *overridden* using a third `:rwm` set of options to each `--device` flag:

```shell
$ docker run --device=/dev/sda:/dev/xvdc --rm -it ubuntu fdisk  /dev/xvdc

$ docker run --device=/dev/sda:/dev/xvdc:r --rm -it ubuntu fdisk  /dev/xvdc
--> You will not be able to write the partition table.

$ docker run --device=/dev/sda:/dev/xvdc:w --rm -it ubuntu fdisk  /dev/xvdc
--> crash....

$ docker run --device=/dev/sda:/dev/xvdc:m --rm -it ubuntu fdisk  /dev/xvdc
--> fdisk: unable to open /dev/xvdc: Operation not permitted
```

## Linux Capabilities:
- Linux capabilities are a powerful feature that provides fine-grained access control over system resources to processes without granting them full root privileges.
- Capabilities can be used to restrict or enhance the access rights of a process to a specific set of resources, such as the ability to bind to privileged ports, modify system time, or read and write to system files.
- Each capability is a bit that can be set or cleared to indicate whether the corresponding privilege is allowed or not for a process. By default, a process has a set of basic capabilities, but additional capabilities can be added or removed using the `setcap` and `getcap` utilities.
- Capabilities are widely used in modern Linux systems to improve security, reduce the attack surface, and enforce the principle of least privilege.
- For a full list of Linux Capabilities and their details refer to below Linux man page:
  - [capabilities(7) — Linux manual page](https://man7.org/linux/man-pages/man7/capabilities.7.html)

## Default Linux Capabilities included with Docker Containers
- Instead of providing priviliged access to containers, with `--privileged` flag, the users can have fine grain control over the capabilities using `--cap-add` and `--cap-drop` flags.
- Docker keeps following Linux capability options allowed by default and but these can be dropped, if required:

|---
| Capability Key | Capability Description
| - | -
| `AUDIT_WRITE` | Write records to kernel auditing log.
| `CHOWN` | Make arbitrary changes to file UIDs and GIDs (see chown(2)).
| `DAC_OVERRIDE` | Bypass file read, write, and execute permission checks.
| `FOWNER` | Bypass permission checks on operations that normally require the file system UID of the process to match the UID of the file.
| `FSETID` | Don’t clear set-user-ID and set-group-ID permission bits when a file is modified.
| `KILL` | Bypass permission checks for sending signals.
| `MKNOD` | Create special files using mknod(2).
| `NET_BIND_SERVICE` | Bind a socket to internet domain privileged ports (port numbers less than 1024).
| `NET_RAW` | Use RAW and PACKET sockets.
| `SETFCAP` | Set file capabilities.
| `SETGID` | Make arbitrary manipulations of process GIDs and supplementary GID list.
| `SETPCAP` | Modify process capabilities.
| `SETUID` | Make arbitrary manipulations of process UIDs.
| `SYS_CHROOT` | Use chroot(2), change root directory.

## Providing additional capabilities to containers
- As an eample, for interacting with the network stack, instead of using `--privileged` one should use `--cap-add=NET_ADMIN` to modify the network interfaces.

```shell
$ docker run -it --rm  ubuntu:22.04 ifconfig
--> Allowed

$ docker run -it --rm  ubuntu:22.04 ifconfig lan2 down
--> SIOCSIFFLAGS: Operation not permitted

$ docker run -it --rm --cap-add=NET_ADMIN ubuntu:22.04 ifconfig lan2 down
--> Allowed
```

- Both `--cap-add` & `--cap-drop` flags support the value `ALL`, so to allow a container to use all capabilities except for `MKNOD`:

```shell
docker run --cap-add=ALL --cap-drop=MKNOD ...
```

- The `--cap-add` and `--cap-drop` flags accept capabilities to be specified with a `CAP_` prefix. The following examples are therefore equivalent:

```shell
docker run --cap-add=SYS_ADMIN ...
docker run --cap-add=CAP_SYS_ADMIN ...
```

- Following are the capabilities which are not granted by default and may be added with `--cap-add` and can be dropped with `--cap-drop` options.

|---
| Capability Key | Capability Description
| - | -
| `AUDIT_CONTROL` | Enable and disable kernel auditing; change auditing filter rules; retrieve auditing status and filtering rules.
| `AUDIT_READ` | Allow reading the audit log via multicast netlink socket.
| `BLOCK_SUSPEND` | Allow preventing system suspends.
| `BPF` | Allow creating BPF maps, loading BPF Type Format (BTF) data, retrieve JITed code of BPF programs, and more.
| `CHECKPOINT_RESTORE` | Allow checkpoint/restore related operations. Introduced in kernel 5.9.
| `DAC_READ_SEARCH` | Bypass file read permission checks and directory read and execute permission checks.
| `IPC_LOCK` | Lock memory (mlock(2), mlockall(2), mmap(2), shmctl(2)).
| `IPC_OWNER` | Bypass permission checks for operations on System V IPC objects.
| `LEASE` | Establish leases on arbitrary files (see fcntl(2)).
| `LINUX_IMMUTABLE` | Set the FS_APPEND_FL and FS_IMMUTABLE_FL i-node flags.
| `MAC_ADMIN` | Allow MAC configuration or state changes. Implemented for the Smack LSM.
| `MAC_OVERRIDE` | Override Mandatory Access Control (MAC). Implemented for the Smack Linux Security Module (LSM).
| `NET_ADMIN` | Perform various network-related operations.
| `NET_BROADCAST` | Make socket broadcasts, and listen to multicasts.
| `PERFMON` | Allow system performance and observability privileged operations using perf_events, i915_perf and other kernel subsystems
| `SYS_ADMIN` | Perform a range of system administration operations.
| `SYS_BOOT` | Use reboot(2) and kexec_load(2), reboot and load a new kernel for later execution.
| `SYS_MODULE` | Load and unload kernel modules.
| `SYS_NICE` | Raise process nice value (nice(2), setpriority(2)) and change the nice value for arbitrary processes.
| `SYS_PACCT` | Use acct(2), switch process accounting on or off.
| `SYS_PTRACE` | Trace arbitrary processes using ptrace(2).
| `SYS_RAWIO` | Perform I/O port operations (iopl(2) and ioperm(2)).
| `SYS_RESOURCE` | Override resource Limits.
| `SYS_TIME` | Set system clock (settimeofday(2), stime(2), adjtimex(2)); set real-time (hardware) clock.
| `SYS_TTY_CONFIG` | Use vhangup(2); employ various privileged ioctl(2) operations on virtual terminals.
| `SYSLOG` | Perform privileged syslog(2) operations.
| `WAKE_ALARM` | Trigger something that will wake up the system.

## Mounting FUSE based file systems to Docker containers:
- What are the __*FUSE*__ based filesystems?
  - __*<u>FUSE (Filesystem in Userspace)</u>*__ is a software interface that allows non-privileged users to create their own file systems without requiring any kernel-level programming. FUSE-based file systems are file systems that are implemented using the [FUSE interface](https://github.com/libfuse/libfuse).
  - FUSE-based file systems *operate in userspace*, which means that they run as ordinary user-level programs and do not require any special privileges or access to kernel resources. This makes it easy for developers to create new file systems and experiment with new ideas, without having to worry about the complexities of kernel-level programming.
  - FUSE-based file systems can provide a wide range of functionality, including virtual file systems, network file systems, encrypted file systems, and many others. They are used in a variety of applications, including cloud storage, backup and recovery, and distributed file systems.
  - There are many FUSE-based file systems available, here are a few examples:
    - <u>NTFS-3G</u>: A popular FUSE-based file system that provides read and write access to NTFS partitions on Linux.
    - <u>sshfs</u>: A FUSE-based file system that allows you to mount a remote file system over SSH.
    - <u>encfs</u>: A FUSE-based file system that provides an encrypted virtual file system.
    - <u>curlftpfs</u>: A FUSE-based file system that allows you to mount an FTP server as a local file system.\
&nbsp;
- To mount a __*FUSE*__ based filesystem, you need to combine both `--cap-add` and `--device`.

```shell
$ docker run --rm -it --cap-add SYS_ADMIN sshfs sshfs sven@10.10.10.20:/home/sven /mnt
--> fuse: failed to open /dev/fuse: Operation not permitted

$ docker run --rm -it --device /dev/fuse sshfs sshfs sven@10.10.10.20:/home/sven /mnt
--> fusermount: mount failed: Operation not permitted

$ docker run --rm -it --cap-add SYS_ADMIN --device /dev/fuse sshfs

# sshfs sven@10.10.10.20:/home/sven /mnt
The authenticity of host '10.10.10.20 (10.10.10.20)' can't be established.
ECDSA key fingerprint is 25:34:85:75:25:b0:17:46:05:19:04:93:b5:dd:5f:c6.
Are you sure you want to continue connecting (yes/no)? yes
sven@10.10.10.20's password:

# root@30aa0cfaf1b5:/# ls -la /mnt/src/docker
total 1516
drwxrwxr-x 1 1000 1000   4096 Dec  4 06:08 .
drwxrwxr-x 1 1000 1000   4096 Dec  4 11:46 ..
-rw-rw-r-- 1 1000 1000     16 Oct  8 00:09 .dockerignore
-rwxrwxr-x 1 1000 1000    464 Oct  8 00:09 .drone.yml
drwxrwxr-x 1 1000 1000   4096 Dec  4 06:11 .git
-rw-rw-r-- 1 1000 1000    461 Dec  4 06:08 .gitignore
....
```

---

So that's it for this topic, in this blog, we explored the use of Linux capabilities for managing runtime privileges in Docker containers. We discussed some common capabilities and how we can grant these privileged capabilities to containers, as well as the potential security risks of granting too many privileges.\
Managing runtime privileges and Linux capabilities is an essential part of container security in Docker. By default, Docker containers run as unprivileged, which limits their device access and prevents certain processes from running inside them. However, by understanding Linux capabilities and how to use them, it is possible to grant additional privileges to containers and enhance their functionality.\
Remember, managing runtime privileges and Linux capabilities is just one aspect of container security. It's important to take a comprehensive approach to container security, including using *<u>secure images</u>*, implementing *<u>network security</u>*, and following other best practices.

---