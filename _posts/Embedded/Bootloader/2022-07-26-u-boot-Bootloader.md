---
title:  "Brief Introduction to U-Boot - the Universal Bootloader"
permalink: /_post/embedded/linux/bootloader/u-boot
date:   2022-07-26 1:33:22 +0530
categories:
  - Linux
  - Embedded Systems
  - Bootloader
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Linux
  - Embedded Systems
  - Bootloader
author: Akhilesh Moghe
show_author_profile: true
---

In this article, we will explore U-Boot Bootloader, a commonly used bootloader in Embedded systems. Number of different Embedded boards, development kits like Raspberry Pi, NVIDIA Jetson supports & actively build their boot-up, board bring up process using U-Boot.
U-Boot is Open Source under __*GPL-2.0-or-later*__ License.

Usually, there are 6 distinct stages of boot process, each explained in brief in [Linux Boot Process](/_post/linux/boot_process) article.
Embedded Linux boot process similar to standard Linux to much extent, but due to the variations in hardware and the board specifics like multi-stage bootloaders, it becomes somewhat different in bootloader section and pre-dominant use of __*u-Boot*__ bootloader in embedded systems also needs to be addressed. To find out overview of [Embedded Linux Boot Process](/_post/embedded/linux/boot_process) see this article.


## U-Boot Reference Material:
- [U-Boot Repository](https://source.denx.de/u-boot/u-boot)
- [U-Boot Documentation](https://u-boot.readthedocs.io/en/latest/)

  ---

## Building U-Boot for Raspberry Pi:
- U-Boot is not an official or default bootloader for Raspberry Pi. RPi uses multi-stage booting process, we can add another stage of U-Boot to this boot process to customize the boot process as per our requirements.
- I have used U-Boot stage with default RPi standard boot process to support __*OTA (Over-the-Air) Update*__ mechanism on RPi 4 board.
- U-Boot main repository itself supports multiple Raspberry Pi boards, look at the [/u-boot/configs/](https://source.denx.de/u-boot/u-boot/-/tree/master/configs) folder.

### *<u>Checkout U-Boot source code</u>*:
```
$ git clone git://git.denx.de/u-boot.git
```
- You can even download the particular release version from [U-Boot repository Tags](https://source.denx.de/u-boot/u-boot/-/tags).

### *<u>Install the ARM Compiler for Cross-compilation</u>*:
- There are various ARM cross-compiler toolchains like __GCC__, __Linaro__, even Raspberry Pi itself provides a compiler in their repository: [raspberrypi/tools](https://github.com/raspberrypi/tools).
- We will use GCC toolchain available as a standard Ubuntu package as follows:
- for __32-bit__:
```
$ sudo apt update
$ sudo apt install gcc-arm-linux-gnueabi
$ export CROSS_COMPILE=arm-linux-gnueabi-
```
- for __64-bit__:
```
$ sudo apt update
$ sudo apt install gcc-aarch64-linux-gnu
$ export CROSS_COMPILE=aarch64-linux-gnu-
```
- Note: Every time you start a new terminal to compile U-Boot, run the export command first.

### *<u>Compile the U-Boot source</u>*:

- __*<u>Configs and DTBs for particular to boards</u>*__:
- __*<u>Important</u>*__:
  - __*Raspberry Pi Kernel*__ depends on Device Tree Binary(DTB) to understand and work with different peripherals during the boot process.
  - Every Raspberry Pi board and the Compute Modules comes with different DTBs and DTB Overlays.
  - One `config` file from `/u-boot/configs/` folder corresponds to a particular DTB, as mentioned in config parameter: `CONFIG_DEFAULT_DEVICE_TREE`.
  - By default, config files supports for Raspberry Pi development boards DTBs.
  - :warning: While compiling, U-Boot for any Raspberry Pi Compute Modules, like CM3 or CM4, we need to modify the default DTB mentioned in the config file corresponding to particular version of Raspberry Pi to the Compute Module compatible DTB.
- Look for different `config` files for various supported boards under `/u-boot/configs/` directory in checked-out repository.
- There are different `config` files for different Raspberry Pi versions and for either 32/64-bit builds. Select appropriate config file.
- for __*RPi 4, 32-Bit*__ build:
```
$ make rpi_4_32b_defconfig
```
- for __*RPi 3, 64-Bit*__ build:
```
$ make rpi_3_defconfig
```
- for __*RPI CM3, 32-Bit*__ build:

```
$ vim /u-boot/configs/rpi_3_32b_defconfig
--> Update following config parameter:
    CONFIG_DEFAULT_DEVICE_TREE=bcm2837-rpi-cm3-io3.dts

$ make rpi_3_32b_defconfig
```
- This creates a hidden `.config` file in the `/u-boot/` folder. You can view it to observe it's contents and modify, if necessary.
- :warning: But be careful, the modifications in `.config` file, will be overwritten by above commands.
- __*<u>Compile the source code</u>*__:
```
$ cd /u-boot/
$ make
```
  - This will create `u-boot` and `u-boot.bin` binaries in the `/u-boot/` folder.

### *<u>Verify U-Boot Binaries</u>*:
```
$ file u-boot
u-boot: ELF 32-bit LSB shared object, ARM, EABI5 version 1 (SYSV), dynamically linked, with debug_info, not stripped

$ file u-boot.bin 
u-boot.bin: COM executable for DOS

```

## *<u>Testing U-Boot on Raspberry Pi</u>*:
- Copy the compiled `u-boot.bin` binary to the Boot Partition of the Raspberry Pi SD card.
- Modify Raspberry Pi boot flow to add `u-boot` bootloader stage as the last stage of normal RPi Boot process, before starting the kernel.
  - :heavy_check_mark: Add `kernel=u-boot.bin` in the `config.txt` file and save this file.
  - With this, the `start.elf` file loads the `u-boot.bin` instead of the kernel.
  - Now, it's `u-boot` bootloaders responsibility to properly load the Linux Kernel + DTBs in memory and start the kernel.
  - :warning: Back-up the original `config.txt` file, before any edits.
```
$ mount /dev/sdb1 /mnt/tmp
$ cp u-boot.bin /mnt/tmp/
$ echo 'kernel=u-boot.bin' > /mnt/tmp/config.txt
$ umount /mnt/tmp
```

### *<u>U-Boot Command Prompt</u>*:
- After above changes, boot the Raspberry Pi board and press any keyboard keys to halt the `u-boot` Autoboot.
- Before Autobooting the kernel, `u-boot` is by default configured to wait for few seconds for user interruption.
- If user presses any keyboard keys, during this wait period, `u-boot` presents the command prompts for user to enter the u-boot commands.

### *<u>Check U-Boot version</u>*:
- Use `version` command on the `u-boot` command prompt to get the version of the `u-boot` binary.

```
<U-Boot> version

```

## *<u>Booting the Kernel</u>*:
- For kernels that need Device Tree Binaries:

```
<U-Boot> setenv fdtfile bcm2710-rpi-cm3.dtb
<U-Boot> mmc dev 0
<U-Boot> fatload mmc 0:1 ${kernel_addr_r} zImage
<U-Boot> fatload mmc 0:1 ${fdt_addr_r} ${fdtfile}
<U-Boot> setenv bootargs earlyprintk console=tty0 console=ttyAMA0 root=/dev/mmcblk0p2 rootfstype=ext4 rootwait noinitrd
<U-Boot> bootz ${kernel_addr_r} - ${fdt_addr_r}
```
- Here, we are setting the Device Tree Binary to be used as `fdtfile` variable.
- We are using __*FAT*__ boot partition to load the kernel `zImage` and DTB file to memory.
- `kernel_addr_r` & `fdt_addr_r` are the RAM addreses for kernel & DTB file respectively.
- We are then setting the boot arguments with `bootargs` variable, enabling the kernel prints, output consoles, and defining the `root` device partition and it's type.
- Then with `bootz` command we are starting the kernel.
- `bootz` command needs the kernel to be in `zImage` format only.


  ---

###### References:
  - [BIOS](http://www.linfo.org/bios.html)
  - [Runlevels](https://en.wikipedia.org/wiki/Runlevel)
  - [Runlevels](https://www.geeksforgeeks.org/run-levels-linux/)
  - [The Linux Booting Process - 6 Steps Described in Detail](https://www.freecodecamp.org/news/the-linux-booting-process-6-steps-described-in-detail/)
  
