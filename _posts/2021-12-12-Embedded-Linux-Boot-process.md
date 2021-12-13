---
title:  "Typical Embedded Linux Boot Process"
permalink: /_post/embedded/linux/boot_process
date:   2021-12-12 1:33:22 +0530
categories:
  - Linux
  - Embedded Linux
  - Bootloader
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Linux
  - Embedded Linux
  - Bootloader
author: Akhilesh Moghe
show_author_profile: true
---

Usually, there are 6 distinct stages of boot process, each explained in brief in [Linux Boot Process](/_post/linux/boot_process) article.
This article will focus on different stages of boot process Specifically in Embedded Linux.
Though the content and screenshots in this article are for BeagleBone board, the boot process is almost similar in general.

## Embedded Linux Booting Process (Multi-Stage Bootloaders, Kernel, Filesystem)
  <iframe width="665" height="374" src="https://www.youtube.com/embed/DV5S_ZSdK0s" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

  - In this video, we will look at how the BeagleBone Black boots into an embedded Linux system. We will understand how the __*ROM Bootloader*__ works for the AM335x SoC series, the first and second stage bootloaders (__*SPL/MLO*__ and __*U-Boot*__) and how to load the kernel, device tree, filesystem and user space processes.


## Embedded Linux Boot Process:

![Embedded Linux Boot Process](/assets/images/embedded/boot/embedded_linux_boot_process.png){:.shadow}

  ---

## Embedded Linux Boot Sequence:

![Embedded Linux Boot Sequence](/assets/images/embedded/boot/embedded_linux_boot_sequence.png)
  
  - The variation in the boot process is typically only in the first couple of Steps, i.e. how does a __*ROM Bootloader*__ loads the __*u-boot*__ in RAM and passes control to it. These steps will be something board specific.
  - Once the __*u-boot*__ is in memory, rest of the steps are standard.

### Why the 1st stage bootloader (MLO) is required at all?
  - ROM Bootloader has only access to internal RAM by default. As the SoC vendors knows very little about how & which external peripherals the SoC is going to interact with. DRAM/RAM controllers are again a separate vendors.
  - So, ROM Bootloader code cannot assume anything, and can only work on the things it knows, i.e. internal RAM.
  - Internal RAM is usually very limited in size in the range of few Kilo-Bytes, for BeagleBone it's 128KB.
  - RAM internal to a SoC is very expensive. This is a key reason why multiple stage bootloaders are really required and are common in embedded devices.
  - On the other hand, U-Boot is fairly large program, given the amountof functionality it supports. It cannot be accomodated in internal RAM of the SoC.
  - That's why MLO or multiple stage bootloaders are required, the MLO understands the interaction with external RAM and can load the u-boot on it.
  - [NVIDIA® Jetson™ Nano has in all 5 Bootloader stages, including BootROM, which is hard-wired in SoC.](/_post/embedded/linux/boot_process/jetson_nano#bootloader-components)

##### References:
  - [Embedded Linux Booting Process (Multi-Stage Bootloaders, Kernel, Filesystem)](https://www.youtube.com/watch?v=DV5S_ZSdK0s&t=6s&ab_channel=PentesterAcademyTV)
  
  
  
