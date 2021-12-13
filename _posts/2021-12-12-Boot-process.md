---
title:  "Linux Boot Process"
permalink: /_post/linux/boot_process
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

This article will focus on different stages of boot process in Linux OS.
There are 6 distinct stages of boot process, each explained in brief as below.
There's also a note on *<u>runlevels</u>* which represents the state of the system after boot, which are usually managed by systemd.

Embedded Linux boot process similar to standard Linux to much extent, but due to the variations in hadware and the board specifics like multi-stage bootloaders, it becomes somewhat different in bootloader section and pre-dominant use of __*u-Boot*__ bootloader in embedded systems also needs to be addressed. To find out specifics of [Embedded Linux Boot Process](/_post/embedded/linux/boot_process) see this article.

## 1. BIOS (Basic Input/Output System)
  - BIOS is a program that controls computer's hardware from the time the computer is started until the main operating system takes over.
  - BIOS first performs some *<u>integrity checks</u>*, i.e. *<u>power on self test</u>* (POST) of the __HDD__ or __SSD__.
  - BIOS acts as an intermediary between the CPU and the input and output devices. This eliminates the need for the operating system to be aware of the hardware addresses of the input and output devices.
  - When there's a change in device details, only the BIOS Configuration needs to be updated. (accomplished by pressing a specified key F12 or F2 as soon as the computer begins to start up).
  - BIOS program, written in the *<u>assembly language</u>* of the CPU used, is stored on __*Electrically Erasable Programmable ROM (EEPROM)*__ or __*flash*__ memory.
  - BIOS searches for, loads, and executes the boot loader program.
  - BIOS loads and executes the __*Master Boot Record (MBR)*__ boot loader.
  - BIOSs permit the user to select the order in which the system searches for *<u>bootable media</u>*.
  - MBR is usually on HDD or SSD, but sometimes on a USB stick or CD-ROM such as with a live installation of Linux.
  - Once the boot loader is loaded into memory and the BIOS gives control of the system to it.
  

## 2. Master Boot Record (MBR)
  - MBR is responsible for loading and executing the __*GRUB*__ boot loader.
  - MBR is located in the *<u>1st sector</u>* of the bootable disk, which is typically `/dev/hda`, or `/dev/sda`.
  - MBR is less than *<u>512 bytes</u>* in size. This has three components 1) *<u>primary boot loader info</u>* in 1st 446 bytes 2) *<u>partition table info</u>* in next 64 bytes 3) *<u>mbr validation check</u>* in last 2 bytes.


## 3. GNU GRand Unified Bootloader (GRUB)
  - GRUB is the typical boot loader for most modern Linux systems.
  - GRUB Splash Screen is the first to appear on screen. If you have multiple kernel images installed, you can use your keyboard to select the one you want your system to boot with.
  - GRUB configuration file is usually at `/boot/grub/grub.conf` or `/etc/grub.conf`.


## 4. Kernel
  - The Kernel that was selected by GRUB first mounts the __*root file system*__ that's specified in the `grub.conf` file.
  - Then Kernel executes the `/sbin/init` program, which is *<u>always the first program to be executed</u>. You can confirm this with its *<u>process id (PID)</u>, which should always be __1__.
  - The kernel then establishes *<u>a temporary root file system</u>* using __*Initial RAM Disk (initrd)*__ until the real file system is mounted.


## 5. Init
  - With Init stage, system executes runlevel programs. System looks for an init file `/etc/inittab` to decide the Linux run level.

## 6. Runlevel Programs
  - A runlevel is a mode of operation in the computer operating systems that implements Unix System V-style initialization.
  - Conventionally, *<u>seven runlevels</u>* exist, numbered from zero to six.
  - *<u>Only one runlevel is executed on startup</u>*; run levels are *<u>not executed one after another</u>*.
  - __*A runlevel defines the state of the machine after boot*__.
  - By default most of the LINUX based system boots to runlevel 3 or runlevel 5.
  - Depending on your default init level setting, which are usually managed by __*systemd*__, the system will execute the start scripts for each run level are different performing different tasks. These start scripts corresponding to each run level can be found in special files present under __*rc sub directories*__.
    - At `/etc/rc.d` directory there will be either a set of files named `rc.0`, `rc.1`, `rc.2`, `rc.3`, `rc.4`, `rc.5` and `rc.6`, or a set of directories named `rc0.d`, `rc1.d`, `rc2.d`, `rc3.d`, `rc4.d`, `rc5.d` and `rc6.d`.
    - In these directories, you'll find programs that start with either an "__*S*__" or "__*K*__" for *<u>startup</u>* and *<u>kill</u>*, respectively. Startup programs are executed during *<u>system startup</u>*, and kill programs during *<u>shutdown</u>*.
    - There are numbers right next to S and K in the program names. Those are the *<u>sequence number</u>* in which the programs should be started or killed.
    ```
    root@PTL011669:# cd /etc/rc0.d/
    root@PTL011669:rc0.d# ls
    K01alsa-utils           K01avahi-daemon    K01docker         K01kerneloops     K01mdadm-waitidle  K01rpcbind
    K01spice-vdagent        K01apache2         K01bluetooth      K01etc-setserial  K01lighttpd        K01mosquitto
    K01rsyslog              K01unattended-upgrades               K01apache-htcacheclean               K01cgroupfs-mount
    K01gdm3                 K01lvm2-lvmetad    K01networking     K01saned          K01uuidd           K01atop
    K01chrony               K01hddtemp         K01lvm2-lvmpolld  K01plymouth       K01setserial       K01virtualbox
    K01atopacct             K01cups-browsed    K01irqbalance     K01mdadm          K01postfix         K01speech-dispatcher
    root@PTL011669:rc0.d# 
    ```
  - __*Changing runlevel*__:
    - __*init*__ is the program responsible for altering the run level which can be called using __*telinit*__ command.
    - <u>Need for changing the runlevel</u>:
      - There can be a situation when you may find *<u>trouble in logging</u>* in in case you don’t remember the password or because of the corrupted `/etc/passwd` file, in this case the problem can be solved by booting into a single user mode i.e runlevel 1.
      - You can easily *<u>halt the system</u>* by changing the runlevel to 0 by using telinit 0.
  - __*Standard runlevels*__:

  | ID | Name                            | Description                                                                  |
  |----|---------------------------------|------------------------------------------------------------------------------|
  | 0  | Off                             | Turns off the device.                                                        |
  | 1  | Single User mode                | Mode for administrative tasks.                                               |
  | 2  | Multi-user mode                 | Does not configure network interfaces and does not export networks services. |
  | 3  | Multi-user mode with networking | Starts the system normally.                                                  |
  | 4  | Not used/user-definable         | For special purposes.                                                        |
  | 5  | Full mode                       | Same as runlevel 3 + display manager.                                        |
  | 6  | Reboot                          | Reboots the device.                                                          |


##### References:
  - [BIOS](http://www.linfo.org/bios.html)
  - [Runlevels](https://en.wikipedia.org/wiki/Runlevel)
  - [Runlevels](https://www.geeksforgeeks.org/run-levels-linux/)
  - [The Linux Booting Process - 6 Steps Described in Detail](https://www.freecodecamp.org/news/the-linux-booting-process-6-steps-described-in-detail/)
  
