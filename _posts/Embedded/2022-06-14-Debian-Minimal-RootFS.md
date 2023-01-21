---
title:  "Complete tutorial to create a Debian Minimal RootFS"
permalink: /_post/linux/debian_minimal_rootfs
date:   2022-06-14 1:33:22 +0530
categories:
  - Linux
  - Embedded Linux
  - Systems Engineering
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Linux
  - Embedded Linux
  - Systems Engineering
author: Akhilesh Moghe
show_author_profile: true
---

This article is about creation of minimal __*<u>rootfs</u>*__ which will be the base file system to be mounted on `/` rootfs partition any Embedded board like Raspberry Pi, NVIDIA Jetson Nano/TX2 once the kernel is loaded in memory by the boot loader. We will also install few linux packages like `ssh`, `nodeJS`, which forms part of pre-installed packages on our customize `rootfs`.\
All steps below should work on any Debian/Ubuntu host and I have verified them with Ubuntu 18.04 LTS.

## Install support packages on host:
- Get apt sources updated:
```
$ sudo apt update
```

  ### *<u>qemu-user-static</u>*
  - QEMU user mode emulation binaries (static version)
  - QEMU is a fast processor emulator: By using dynamic translation it achieves reasonable speed while being easy to port on new host CPUs.
  - currently the package supports `ARM`, `CRIS`, `i386`, `M68k` (ColdFire), `MicroBlaze`, `MIPS`, `PowerPC`, `SH4`, `SPARC` and `x86-64` emulation.
  - This `qemu-user-static` package provides the *<u>user mode emulation binaries</u>*, built statically.
  - In this mode QEMU can *<u>launch Linux processes compiled for one CPU on another CPU</u>*.
  - `qemu-user-static` package will *<u>register binary formats</u>* which the provided emulators can handle, so that it will be possible to *<u>run foreign binaries directly</u>*.
  - *Other few QEMU packages (mentioned here just for info, not required for the task at hand)*:
    - __*<u>qemu</u>*__: Once upon a time there was only one package named `qemu`, with all functionality included. These days, qemu become large and has been split into numerous packages. Different packages provides entirely different services, and it is very unlikely one will need all of them together. So current `qemu` package makes no sense anymore, and is becoming a dummy package.
    - __*<u>qemu-user</u>*__: If you want user-mode emulation, install `qemu-user` or `qemu-user-static` package.
    - __*<u>qemu-utils</u>*__: If you need utilities, use `qemu-utils` package.
  ```
  $ sudo apt install qemu-user-static
  ```

  ### binfmt-support
  - __*<u>Support for extra binary formats</u>*__
  - The `binfmt_misc` kernel module allows system administrators to *<u>register interpreters for various binary formats</u>* based on a *magic number* or their *file extension*, and cause the *<u>appropriate interpreter to be invoked whenever a matching file is executed</u>*.
    - Think of it as a more flexible version of the `#!` (shebang line) executable interpreter mechanism.
  - This `binfmt-support` package provides an `update-binfmts` script with which *<u>package maintainers can register interpreters to be used with this module</u>* without having to worry about writing their own `init.d` scripts, and which sysadmins can use for a slightly higher-level interface to this module.
  ```
  $ sudo apt install binfmt-support
  ```

  ### debootstrap
  - `debootstrap` is a tool which will *<u>install a Debian base system into a subdirectory of another, already installed system</u>*.
  - It doesn't require an installation CD, just access to a Debian repository (~internet).
  - It can also be installed and run from another operating system, so, for instance, you can use `debootstrap` to *<u>install Debian onto an unused partition from a running</u>* [Gentoo Linux](https://www.gentoo.org/).
  - It can also be used to __*<u>create a rootfs</u>*__*<u> for a machine of a different architecture</u>*, which is known as `cross-debootstrapping`.
  - :x: `Debootstrap` can __*only use one repository for its packages*__. If you need to merge packages from different repositories (the way apt does) to make a rootfs, or you need to automatically customise the rootfs, then use [Multistrap](https://wiki.debian.org/Multistrap).
  ```
  $ sudo apt install debootstrap
  ```

    ---

## Build 1st stage of Debian RootFS
- __*ARM Architecture*__:
  - I am using Raspberry Pi 4 Model B board while drafting this article, RPi4 uses __*Broadcom BCM2711*__ chip which has __*ARM Cortex-A72*__ CPU, which implements __*ARMv8-A*__ 64-bit instruction set.
  - I am also going to try this created RootFS on [Revolution Pi, RevPi Connect](https://revolutionpi.com/revpi-connect/) box which uses __*Raspberry Pi Compute Module 3*__ chip which has __*ARM Cortex-A53*__ CPU, which also implements __*ARMv8-A*__ 64-bit instruction set.
  - So, in my opinion, the same RootFS should work on both the boards, though they have different processors, but the same instruction set and the ARM Architecture.
  - ARMv8-A has 64-bit ARM Architecture known as __*AArch64*__ or __*ARM64*__. __*Is there a difference?*__
    - AArch64 is the 64-bit state introduced in the Armv8-A architecture, The 32-bit state which is backwards compatible with Armv7-A and previous 32-bit Arm architectures is referred to as AArch32. Therefore the GNU triplet for the 64-bit ISA is __*aarch64*__.
    - The Linux kernel community chose to call their port of the kernel to this architecture __*arm64*__ rather than aarch64, so that's where some of the arm64 usage comes from.
    - *<u>So, AArch64 is the ARM64, no difference</u>*.
  - References:
    - [Wikipedia: AArch46](https://en.wikipedia.org/wiki/AArch64)
    - [Stackoverflow: Differences between arm64 and aarch64](https://stackoverflow.com/q/31851611/2003556)
    - [Wikipedia: Raspberry_Pi Specifications](https://en.wikipedia.org/wiki/Raspberry_Pi#Specifications)
    - [Wikipedia: ARM Cortex-A53](https://en.wikipedia.org/wiki/ARM_Cortex-A53)
    - [Wikipedia: ARM Cortex-A72](https://en.wikipedia.org/wiki/ARM_Cortex-A72)
    - [Wikipedia: Comparison of ARMv8-A processors](https://en.wikipedia.org/wiki/Comparison_of_ARMv8-A_processors)
    - [Wikipedia ARM 64/32-bit Architecture](https://en.wikipedia.org/wiki/ARM_architecture_family#64/32-bit_architecture)
    - [Revolution Pi, RevPi Connect](https://revolutionpi.com/revpi-connect/)
    - [Raspberry Pi OS (64-bit): compatible with CM3 & Pi4](https://www.raspberrypi.com/software/operating-systems/)
    - [Raspberry Pi 4 Model B: BCM2711](https://www.raspberrypi.com/documentation/computers/processors.html#bcm2711)
    - [Raspberry Pi Compute Module 3: BCM2837](https://www.raspberrypi.com/documentation/computers/processors.html#bcm2837)
      - The underlying architecture of the BCM2837 is identical to the BCM2836.
      - The only significant difference is the replacement of the ARMv7 quad core cluster with a quad-core ARM Cortex A53 (ARMv8) cluster.
    - [Raspberry Pi Compute Module 3+: BCM2837B0](https://www.raspberrypi.com/documentation/computers/processors.html#bcm2837b0)
      - The underlying architecture of the BCM2837B0 is identical to the BCM2837 chip used in other versions of the Raspberry Pi.
      - The ARM core hardware is the same, only the frequency is rated higher.
- __*buster*__: One of th distributions of debian.
- __*debianMinimalRootFS*__: Target directory for the new RootFS.
- __*--foreign option*__: `debootstrap` is also capable of installing a system for any supported architecture, not only the host system's architecture; with its `--foreign` option. If necessary it can use Qemu to emulate the target architecture.
- :heavy_check_mark: The output will be a basic rootfs file structure in target directory. Above command installs many debian packages in this directory, have a look at the logs and the created folder structures in target directory.
```
$ mkdir debianMinimalRootFS
```
- For __64-Bit__ OS:
```
$ sudo debootstrap --arch=arm64 --foreign buster debianMinimalRootFS
```
- For __32-Bit__ OS:
```
$ sudo debootstrap --arch=armhf --foreign buster debianMinimalRootFS
```

    ---

## Copy `qemu-aarch64-static` to newly created rootfs 
- Copy `qemu-aarch64-static` binary into `/$target_dir/usr/bin/` for binfmt packages to find it.
- For __64-Bit__ OS:
```
$ sudo cp /usr/bin/qemu-aarch64-static /debianMinimalRootFS/usr/bin/
```
- For __32-Bit__ OS:
```
$ sudo cp /usr/bin/qemu-arm-static /debianMinimalRootFS/usr/bin/
```

    ---

## Copy `resolv.conf` to newly created rootfs for internet access
- The `/etc/resolv.conf` file defines how the system uses __*<u>DNS</u>*__ *<u> to resolve host names and IP addresses</u>*.
- This file usually contains *<u>a line specifying the </u>*__*<u>search domains</u>*__ and up to *<u>three lines that specify the </u>*__*<u>IP addresses of DNS server</u>*__.
```
$ sudo cp /etc/resolv.conf /debianMinimalRootFS/etc/resolv.conf
```

    ---

## Setup newly created RootFS
- __*<u>chroot, chroot jail</u>*__:
  - A `chroot` on Unix operating systems is *<u>an operation that changes the apparent root directory for the current running process and its children</u>*.
  - *<u>A program that is run in such a modified environment cannot name (and therefore normally cannot access) files outside the designated directory tree</u>*.
  - A chroot environment can be used to create and host a separate virtualized copy of the software system.
- In our case, we will `chroot` to further setup the RootFS.
```
$ sudo chroot /debianMinimalRootFS
```
- Setup environment:
```
# export LANG=C
```

    ---

## 2nd stage of `debootstrap`
- We need to run the 2nd stage of `debootstrap` to install the packages downloaded earlier, as:
```
# /debootstrap/debootstrap --second-stage
```
- Let the installation complete, it may take few minutes. After completion, you should get following message on terminal:
- :heavy_check_mark: `I: Base system installed successfully.`

    ---

## Add debian repository and apt configuration

```
# cat <<EOT > /etc/apt/sources.list
> deb http://deb.debian.org/debian buster main contrib non-free
> deb-src http://deb.debian.org/debian buster main contrib non-free
> deb http://security.debian.org/ buster/updates main contrib non-free
> deb-src http://security.debian.org/ buster/updates main contrib non-free
> deb http://deb.debian.org/debian buster-updates main contrib non-free
> deb-src http://deb.debian.org/debian buster-updates main contrib non-free
< EOT
```

- Check the apt sources list:
```
# cat /etc/apt/sources.list
```
- Update debian database:
```
# apt update
```

    ---

## Install and configure few packages
- Install `locales` and `dialog` packages, otherwise, set up locales dpkg scripts tend to complain.
```
# apt install locales dialog
```

- Configure `locales`:
```
# dpkg-reconfigure locales
```
  - Select `en_US.UTF-8 UTF-8` only, click `ok` on the dialog box.
  - On next dialog box, again select `en_US.UTF-8`, click `ok`.

    ---

## Install some useful packages inside the chroot
```
# apt install vim openssh-server ntpdate sudo ifupdown net-tools udev iputils-ping wget dosfstools unzip binutils libatomic1
```

  - `ntpdate` - sets the local date and time by polling the Network Time Protocol (NTP) server(s) given as the server arguments to determine the correct time.
  - `ifupdown` - configure (or, respectively, deconfigure) network interfaces based on interface definitions in the file /etc/network/interfaces.
  - `net-tools` - This package includes arp(8), hostname(1), ifconfig(8), ipmaddr, iptunnel, mii-tool(8), nameif(8), netstat(8), plipconfig(8), rarp(8), route(8) and slattach(8).
  - `udev` - (userspace /dev) - primarily manages device nodes in the `/dev` directory. At the same time, udev also handles all user space events raised when hardware devices are added into the system or removed from it, including firmware loading as required by certain devices.
  - `iputils-ping` - `ping` command that sends ICMP ECHO_REQUEST packets to a host in order to test if the host is reachable via the network. This package includes a `ping6` utility which supports `IPv6` network connections.
  - `wget` - downloads files served with HTTP, HTTPS, or FTP over a network.
  - `dosfstools` - The dosfstools package contains various utilities for use with the `FAT` family of file systems like `mkfs.fat` and `fsck.fat` utilities, which respectively make and check MS-DOS FAT filesystems.
  - `unzip` - Command used to unzip the zipped files.
  - `binutils` - It's very basic GNU package and it's utilities listed below are required by many other applications to work properly, like AWS Greengrass.
    - GNU Binutils are a collection of binary tools:
    - `ld` - the GNU linker.
    - `as` - the GNU assembler.
    - `addr2line` - Converts addresses into filenames and line numbers.
    - `ar` - A utility for creating, modifying and extracting from archives.
    - `c++filt` - Filter to demangle encoded C++ symbols.
    - `dlltool` - Creates files for building and using DLLs.
    - `gold` - A new, faster, ELF only linker, still in beta test.
    - `gprof` - Displays profiling information.
    - `nlmconv` - Converts object code into an NLM.
    - `nm` - Lists symbols from object files.
    - `objcopy` - Copies and translates object files.
    - `objdump` - Displays information from object files.
    - `ranlib` - Generates an index to the contents of an archive.
    - `readelf` - Displays information from any ELF format object file.
    - `size` - Lists the section sizes of an object or archive file.
    - `strings` - Lists printable strings from files.
    - `strip` - Discards symbols.
    - `windmc` - A Windows compatible message compiler.
    - `windres` - A compiler for Windows resource files.
  - `libatomic1` - library providing __atomic built-in functions. When an atomic call cannot be turned into lock-free instructions, GCC will make calls into this library.
                 - Required by node 12.x installation.

    ---

## Install NodeJS on custom debian rootfs
- We are creating the `rootfs` for the `arm` architecture, so we will not take the usual `nvm` installation path, rather we will download the Node Archive for particular ARM Architecture for required Node version from the `https://nodejs.org/dist/` website, which hosts all Node releases.
- We will be installing __*Node12*__ this time.
- Download node installable:
```
# cd /tmp
# wget https://nodejs.org/dist/v12.0.0/node-v12.0.0-linux-armv7l.tar.xz
```
- Extract:
```
# xz -d node-v12.0.0-linux-armv7l.tar.xz
# tar -xvf node-v12.0.0-linux-armv7l.tar
```
- Install:
```
# cd /tmp/node-v12.0.0-linux-armv7l.tar.xz/
# cp -R * /usr/local/
```
- Verify Installation:
```
# node -v
# npm -v
```

  ---

## Set root password for login
- `passwd` command is used to update the password of the Linux users.
- If no user is specified, default `root` user is considered for password update.
```
# passwd
```

  ---

## Create user and set password
- `useradd` command creates a new user entry in `/etc/passwd`, `/etc/shadow`, `/etc/group`, `/etc/gshadow` files.
- `-m` option with `useradd` creates Home Directory for the new user as: `/home/<user-name>`
```
# useradd -m <user-name>
# passwd <user-name>
```
- __*<u>Change the default `shell` to `/bin/bash`</u>*__ for the users, why because, Bash provides a lot of flexibility and syntax that looks a lot like those of modern programming languages. Few key features of Bash are:
  - __Command-line completion__ can be used to quickly complete a command using the <TAB> key
  - __Command history__ through which we can quickly search for commands previously executed by using the <Up> arrow key or <CTRL-R>
  - __Arithmetic evaluation__ without having to use external programs
  - __Associative arrays__, which enables us to create an array with string indices
  - Keyboard shortcuts for command-line editing
  - Customizability enables us to modify the default presentation offered by Bash
    - like Colors, Current Folder or Complete Path to current folder settings can be modified with variables like: `$PS1`.
```
# usermod --shell /bin/bash <user-name>
```

  ---

## Network settings in RootFS
- Following settings will enable a Static IP address: 192.168.0.254 to the board on `eth0` network interface. We should be able to SSH to the board.
- `allow-hotplug <interface>` - Start the <interface> when a "hotplug" event is detected, i.e. when the ethernet cable is inserted.
- For __*Static IP*__:
```
# cat >> EOT > etc/network/interfaces
allow-hotplug eth0
iface eth0 inet static
	address 192.168.1.254
	netmask 255.255.255.248
	gateway 192.168.1.1	
EOT
```
- For __*Dynamic IP*__:
```
# cat >> EOT > etc/network/interfaces
> auto eth0
> iface eth0 inet dhcp
> EOT
```
- __*<u>Ping Command Alias</u>*__:
  - Ping command supports both ipv4 & ipv6, but for some reason unknown to me, `ping` command is acting as `ping -6` i.e. for ipv6 protocol by default.
  - On the RootFS created, by default ipv6 is disabled, confirmed with below command, which returns ipv6 address, if it's enabled on the OS or on RootFS:
    ```
$ ip -6 addr
$ ip -4 addr
$ ip a
    ```
  - So `ping` command results in error:
    - Error: `ping: socket: Address family not supported by protocol`
  - Solution for this error is to create an alias of the `ping -4` command, so that we need not to remember everytime about using this flag for ipv4.
  ```
$ sudo vim ~/.bashrc
>
> # Custom aliases
> alias ping='ping -4'
>
  ```
  - Reference:
    - [archlinux: ping error](https://bbs.archlinux.org/viewtopic.php?id=202614)
    - [Check if ipv6 enabled or disabled](https://www.golinuxcloud.com/linux-check-ipv6-enabled/)

  ---

## Set `hostname` for the board
```
# echo <any-host-name> > etc/hostname
```
- Edit `/etc/hosts` to __*add an entry*__ for hostname in it.
```
# vim /etc/hosts
> 127.0.1.1 <Tab> <any-host-name>
```

  ---

## Add `fstab` entries for partitions
- Note: these are the tabs, between the columns in `fstab` file.

```
# vim etc/fstab
>>>
proc  /proc proc  defaults  0 0
PARTUUID=<PARTUUID of `ROOTFS Partition of SD card or loop mounted Image`>  /   ext4    defaults,noatime    0   1
PARTUUID=<PARTUUID of `BOOT Partition of SD card or loop mounted Image`>  /boot  vfat    defaults    0   2
PARTUUID=<PARTUUID of `DATA Partition of SD card or loop mounted Image`>  /data ext4  defaults  0 0
>>>
```

  ---

## Configure `sudo` utility
- Few Errors that we may face, if `sudo` utility is not configured properly as:
  - `Error: sudo: /usr/bin/sudo must be owned by uid 0 and have the setuid bit set`
    - Solution: [Reference](https://askubuntu.com/a/471503/896173)
      ```
      # chown root:root /usr/bin/sudo
      # chmod 4755 /usr/bin/sudo
      ```
  - `Error: <username> is not in the sudoers file`
    - Solution: [Reference](https://www.tecmint.com/fix-user-is-not-in-the-sudoers-file-the-incident-will-be-reported-ubuntu/)
      ```
      # adduser <username> sudo
      ```
  - `Error: sudo unable to resolve host`
    - Solution: [Reference](https://askubuntu.com/a/59517/896173)
      - Add `hostname` entry to `/etc/hosts` file as per section: [Set `hostname` for the board](/_post/linux/debian_minimal_rootfs#set-hostname-for-the-board)

  ---

## Exit chroot shell
- Your Custom Debian Minimal RootFS is ready in debianMinimalRootFS` folder.
- We can now exit the from the `chroot`.
```
# exit
```

  ---

## Remove Support files from RootFS
```
$ sudo rm /debianMinimalRootFS/usr/bin/qemu-aarch64-static
or
$ sudo rm debianMinimalRootFS/usr/bin/qemu-arm-static
```

  ---

## Copy Custom RootFS to SD card Partition
- You can dirctly copy whole contents of the debianMinimalRootFS` folder on to a ROOTFS partition of the bootable/disk image.
- But if we directly copy the contents of the `debianMinimalRootFS` i.e. Custom RootFS folder, we had just exited from to any partition of the SD card using `cp` command, there are definite chances of intereferences with the file/folder properties like, the folder/file permissions getting changed to the `root` user, which might probably be used to copy the contents and many more as such.
- This may result in folders/files only being accessible to `root` user even after successful boot.
- To avoid any such issues, it's always better to bundle the custom RootFS using `tar` command, which stores information about ownership and permissions and is recursive by default.
- Also, while extracting the contents from the bundle, `tar` command provides the flags like `--same-owner` and `--same-permissions`, which preserves the ownership & permissions of the files while being extracted.
- Create a bundle/archive of the custom rootfs:
```
$ cd `debianMinimalRootFS`
$ sudo tar cvf debianMinimalRootFS.tar *
```
- Check the bundle created:
```
$ ls -ltha
```
- Extract the custom rootfs bundle to SD card partition mounted e.g. at `/media/SD-Card`
```
$ sudo tar --same-owner --same-permissions -xvf debianMinimalRootFS.tar -C /media/SD-Card/ROOTFS/
```
- Check the Extracted custom rootfs from SD-Card partition:
```
$ ls -ltha /media/SD-Card/ROOTFS/
```

---

- With this, your SD-Card is ready to use with board.
- You may be required to modify the boot command like in `cmdline.txt` in `/boot` partiton for RaspberryPi as per the boot settings.
- Also, there might be changes required in `/etc/fstab` file to mount the various partitions at the boot time.
- Hopefully, your board should boot without any errors... fingers crossed...
- Note:
  - This is a very minimal rootfs, many packages would be required to be installed in order to carryout the work properly.
  - But this image is configured to connect to the ethernet port and internet should be accessible for you to install packages, so No Worries...

  ---

###### References:
- [qemu-user-static](https://packages.debian.org/sid/qemu-user-static)
- [binfmt-support](https://packages.debian.org/sid/binfmt-support)
- [Debootstrap](https://wiki.debian.org/Debootstrap)
- [Wikipedia: chroot](https://en.wikipedia.org/wiki/Chroot)
- [useradd](https://linuxize.com/post/how-to-create-users-in-linux-using-the-useradd-command/)
- [Details of /etc/network/interfaces](https://unix.stackexchange.com/q/128439/98539)
- [How to preserve file structure/ownership/permission/... when downloading a folder from remote server?](https://superuser.com/a/1461631/568169)
- 
