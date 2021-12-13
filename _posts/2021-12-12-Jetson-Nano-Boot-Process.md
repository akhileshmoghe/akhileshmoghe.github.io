---
title:  "NVIDIA L4T Embedded Linux Boot Process for Jetson Nano"
permalink: /_post/embedded/linux/boot_process/jetson_nano
date:   2021-12-12 1:33:22 +0530
categories:
  - Linux
  - Embedded Linux
  - Bootloader
  - Jetson Nano
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Linux
  - Embedded Linux
  - Bootloader
  - Jetson Nano
author: Akhilesh Moghe
show_author_profile: true
---

## Jetson Nano Bootloader functionalities:
  - The primary function of the NVIDIA® Jetson Nano™ boot software is to initialize the SoC (System on a Chip), including:
  - Initializing MC/EMC/CPU
  - Setting up security parameters
  - Loading different firmware
  - Maintaining Chain of Trust
  - Setting memory carveouts for different firmware
  - Flashing the device
  - Booting to the operating system
  Additionally, the Jetson Nano boot software also performs other operations defined by product requirements, including but not limited to:
  - Initialization of HDMI™/DSI
  - Displaying the boot logo

  ![jetson_nano_boot_sequence](/assets/images/embedded/boot/jetson_nano_boot_sequence.png)

  - __*BPMP???*__
    - BPMP is NVIDIA Tegra *<u>Boot and Power Management Processor</u>*
    - The BPMP is a specific processor in Tegra chip, which is designed for booting process handling and offloading the power management, clock management, and reset control tasks from the CPU.
    - The BPMP firmware driver, which can create the interprocessor communication (IPC) between the CPU and BPMP.

## Bootloader Components
### 0. BootROM
  - Jetson Nano BootROM (BR) is *<u>hard-wired in the SoC</u>*.
  - It initializes the Boot Media and loads bootloaders and firmware from the Boot Media.
  - __*Boot Configuration Table (BCT)*__
    - Multiple copies of the BootROM Boot Configuration Table (BCT) may be stored *<u>at the start of the Boot-Media</u>*.
    - The BCT contains configuration parameters used by the BootROM for hardware initialization.
    - Bootloader info in BCT:
      - Size
      - Entry point
      - Load address
      - Hash

    ![bootrom-boot-flow](/assets/images/embedded/boot/bootrom-boot-flow.jpg)
  
### 1. TegraBoot
  - TegraBoot (NVTBoot) is the *<u>first boot software</u>* component loaded by BootROM in *<u>SysRAM (Internal RAM)</u>*, and runs on BPMP.
  - 2 Types:
    - One used for *<u>cold boot</u>* (~hard boot:restart the board)
    - One for *<u>recovery boot/flashing</u>*
  - Responsibilities:
    - Loading and initializing firmware (FW) components such as __*TOS*__
      - TOS contains the *<u>trusted OS binary</u>*.
    - Creating carveouts
      - carveouts: It’s share memory for the coprocess.
    - Completing CPU initialization
    - Loading the next stage bootloader
    - Supporting flashing
    - Supporting RCM boot
      - Recovery mode: used during flasing the board
    - Reading PMIC reset reason
      - PMIC: Power Management IC
    - Loading the bootloader device tree and passing the device tree load address to CBoot
    - Stops execution when the CCPLEX is booted
      - CCPLEX: main CPU Complex, CCPLEX typically runs the system’s primary software stack.

### 2. TegrBoot CPU
  - Add rollback prevention.
  - Using bootloader DTB(~device tree binary), perform EMC(~Electromagnetic Compatibility) training and update kernel DTB with training results.
  - Pass control to CBoot.

### 3. CBoot
  - *<u>Primary CPU bootloader</u>* used on mobile platforms in the *<u>cold boot</u>* path.
  - features:
    - Supports display and *<u>boot logo/bmp splash</u>*
    - Based on the [__*Little Kernel (LK) open source bootloader*__](https://github.com/littlekernel/lk)
    - Uses the interrupt and scheduling frameworks of LK
    - Uses CDF for frameworks, drivers, and libraries
    
  - BootLoader and kernel use separate device trees stored in separate partitions.

  - Responsibilities:
    - Parsing the CPU-BL parameters and initializing the bootloader device tree
    - Chaining to U-Boot to boot the kernel
    - *<u>Supporting the update mechanism</u>*
    - Unhalts the BPMP so that the BPMP-FW can start running

### 4. U-Boot
  - default OS bootloader for NVIDIA® Jetson™ L4T Driver Package.


## Partitions
  - __*L4T*__ supports formatting mass storage media(~SD cards, USB) into *<u>multiple partitions</u>* for storing data, such as the device *<u>OS image</u>*, *<u>Bootloader image</u>*, *<u>device firmware</u>*, and *<u>Bootloader splash screens</u>*.

### Partition Configuration file
  - Located at `<top>/Linux_for_Tegra/bootloader/t210ref/cfg/` for Jetson Nano devices
  - NVIDIA Jetson Nano (SKU 0000): flash_l4t_t210_max-spi_sd_p3448.xml
  - NVIDIA Jetson Nano (SKU 0002): flash_l4t_t210_emmc_p3448.xml
  - During the flashing procedure, `flash.sh` reads in the partition configuration file, translates keywords into values specified in `<device>.conf` or in option parameters and saves the data in `bootloader/flash.xml`.
  - Then `__*bootloader/tegraflash.py*__` reads in `bootloader/flash.xml` and *<u>performs actual flashing</u>* as specified by `bootloader/flash.xml`.

### Partition Table Overview
  - Describe partition use for the *<u>boot device</u>* (~MicroSD Card, USB) on each supported platform.
  - Not all Partiotions are really required.
  - [Jetson Nano Development Module (P3448-0000) Flashed to On-Board Memory](https://docs.nvidia.com/jetson/l4t/Tegra%20Linux%20Driver%20Package%20Development%20Guide/part_config.html#wwp115285)
  - [Jetson Nano Development Module (P3448-0000) Flashed to Micro SD Card](https://docs.nvidia.com/jetson/l4t/Tegra%20Linux%20Driver%20Package%20Development%20Guide/part_config.html#wwp116068)
  - [Jetson Nano Production Module (P3448-0002)](https://docs.nvidia.com/jetson/l4t/Tegra%20Linux%20Driver%20Package%20Development%20Guide/part_config.html#wwp117730)


## Bootloader Update
  - NVIDIA® Jetson™ Nano platform use the __*Debian package*__ facility to update Bootloader.
  - __*Dual Boot Strategy*__
    - *<u>Ensures that a usable Bootloader partition exists at all times during an update</u>*.

### Bootloader Components Validation & Partition Layout
  - __Boot Configuration Table__ (BCT) is stored in a partition named BCT.
  - The NVIDIA flashing utility `tegraflash.py` writes up to *<u>64 instances of the BCT</u>*.
#### *<u>Bootloader Validation</u>*:
  - __BootROM__ validates the BCT through an *<u>integrated checksum</u>* or *<u>RSA signature</u>*.
  - If the calculated checksum or signature does not match the value in the BCT located at the beginning of the partition, the BootROM attempts to validate the next instances of the BCT.
  - When __BootROM__ finds a valid set of checksums or signatures, it transfers control to the specified instance of TegraBoot.
  - __TegraBoot__ computes and *<u>validates the checksums</u>* for the first __BFS__ and the first __KFS__ and the signatures the individual files. When the checksums and signatures have been validated, TegraBoot loads the appropriate boot files,
    - __BFS__: a set of all of the files in a group of partitions that are concerned with booting.
      - The TegraBoot CPU binary
      - DTB files used by Bootloader
    - __KFS__: a set of all of the files in a group of partitions that are concerned with loading the kernel.
      - DTB files used by the kernel
      - Warmboot binary
      - Trusted OS image
  - After validation, TegraBoot transfers control to the boot loader, e.g. __CBoot__.
  - The CBoot loader validates and loads the next-level software component, such as the __Linux kernel__ or __U‑Boot__.
  - If TegraBoot *<u>fails to validate</u>* the first BFS andKFS, it *<u>overwrites itself</u>* and *<u>resets the board</u>* so that the BootROM can validate and load the next set of TegraBoot, BFS, and KFS.

#### *<u>Partition Layout</u>*:
__1.__ __*<u>Boot Partition</u>*__:
  - __BCT__, which contains redundant instances of the *<u>Boot Configuration Table</u>*. This must be the first partition on the boot device.
  - __NVC__ contains *<u>TegraBoot</u>*. This must be the second boot partition.
  - The following boot partitions, PT through BPF, are part of the BFS.
  - __PT__ contains layout information for each BFS, and indicates the beginning of each one. It is the first partition in the BFS.
  - __TBC__ contains the *<u>TegraBoot CPU-side binary</u>*.
  - __RP1__ contains *<u>TegraBoot DTBs</u>*.
  - __EBT__ contains *<u>CBoot</u>*.
  - __WB0__ contains the *<u>warm boot binary</u>*.
  - __BPF__ contains [*<u>BPMP microcode</u>*](/_post/embedded/linux/boot_process/jetson_nano#jetson-nano-bootloader-functionalities)
  - __NVC‑1__ contains a copy of NVC.
  - __PT‑1__ through BPF‑1 are copy partitions for the primaries NVC through BPF, making up a copy of the BFS, denoted BFS‑1.
  - __PAD__ is an *<u>empty partition</u>* which ensures the VER and VER_b are at the very end of the boot partition.
  - __VER_b__ contains additional *<u>version information for redundancy and version checking</u>*.
  - __VER__ contains version information.
  
__2.__ __*<u>GP1</u>*__ contains the sdmmc_user device’s *<u>primary GPT</u>*. All partitions defined after this one are configured in the Linux kernel, and are accessible by standard partition tools such as gdisk and parted.

__3.__ __*<u>User Partition</u>*__:
  - The following partitions constitute the kernel-file-set (KFS), and have redundant copy partitions:
  - __DTB__ contains *<u>kernel DTBs</u>*.
  - __TOS__ contains the *<u>trusted OS binary</u>*.
  - __EKS__ is optional, and is reserved for future use.
  - __LNX__ contains either the *<u>Linux kernel</u>* or *<u>U-Boot</u>*, depending on your choice of DFLT_KERNEL_IMAGE in the configuration file.
  - __DTB‑1__ through EKS‑1 constitute a copy of the primary KFS, denoted KFS‑1.
  

### Bootloader Update Payload (BUP)
  - __multi-spec BUP__:
    - A multi-spec BUP includes multiple firmware binaries and BCT images of each type.
    - When you run the updater, it uses the appropriate binary of each type for the device you are updating.
    
  - __single-spec BUP__:
    - A single-spec BUP contains just one binary of each type, and can be used to update just one Jetson device configuration.

  Detailed Steps for:
  - [Generating the Bootloader Update Payload (BUP)](https://docs.nvidia.com/jetson/l4t/Tegra%20Linux%20Driver%20Package%20Development%20Guide/bootloader_update_nano_tx1.html#wwpID0E0IG0HA)
  - [Bootloader Update](https://docs.nvidia.com/jetson/l4t/Tegra%20Linux%20Driver%20Package%20Development%20Guide/bootloader_update_nano_tx1.html#wwpID0E03E0HA)
  - [Bootloader Update Flow](https://docs.nvidia.com/jetson/l4t/Tegra%20Linux%20Driver%20Package%20Development%20Guide/bootloader_update_nano_tx1.html#wwpID0E0QE0HA)
  

  
##### References:
  - [Embedded Linux Booting Process (Multi-Stage Bootloaders, Kernel, Filesystem)](https://www.youtube.com/watch?v=DV5S_ZSdK0s&t=6s&ab_channel=PentesterAcademyTV)
  
  
  
