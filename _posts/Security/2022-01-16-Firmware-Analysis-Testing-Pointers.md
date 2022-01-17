---
title:  "Pointers for Secure IoT Firmware Analysis & Testing"
date:   2022-01-16 12:33:22 +0530
permalink: /_posts/iot/firmware_analysis
categories:
  - IoT
  - Security
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - IoT
  - Security
author: Akhilesh Moghe
show_author_profile: true
---

This article is from Firmware Analysis Project that provides:
  - Security testing guidance for vulnerabilities in the "Device Firmware" attack surface.
  - Steps for extracting file systems from various firmware files.
  - Guidance on searching a file systems for sensitive of interesting data.
  - Information on static analysis of firmware contents.
  - Information on dynamic analysis of emulated services. (e.g. web admin interface)
  - Testing tool links
  - A site for pulling together existing information on firmware analysis
  
## Device Firmware Vulnerabilities
Usual suspect points:
  - Out-of-date core components
  - Unsupported core components
  - Expired and/or self-signed certificates
  - Same certificate used on multiple devices
  - Admin web interface concerns
  - Hardcoded or easy to guess credentials
  - Sensitive information disclosure
  - Sensitive URL disclosure
  - Encryption key exposure
  - Backdoor accounts
  - Vulnerable services (web, ssh, tftp, etc.)

## Recommendations
  - Ensure that supported and up-to-date software is used by developers.
  - Ensure that robust update mechanisms are in place for devices.
  - Ensure that certificates are not duplicated across devices and product lines.
  - Ensure supported and up-to-date software is used by developers.
  - Develop a mechanism to ensure a new certificate is installed when old ones expire.
  - Disable deprecated SSL versions.
  - Ensure developers do not code in easy to guess or common admin passwords.
  - Ensure services such as SSH have a secure password created.
  - Develop a mechanism that requires the user to create a secure admin password during initial device setup.
  - Ensure developers do not hard code passwords or hashes.
  - Have source code reviewed by a third party before releasing device to production.
  - Ensure industry standard encryption or strong hashing is used.


## Device Firmware Analysis Guidance:
- Following kind of analysis practices should be established to avoid dand minimize the risk of evice firmware vulnerabilities:
  - Firmware file analysis
  - Firmware extraction
  - Dynamic binary analysis
  - Static binary analysis
  - Static code analysis
  - Firmware emulation
  - File system analysis

## Device Firmware Tools
  - [Firmwalker](https://github.com/craigz28/firmwalker)
    - A simple bash script for searching the extracted or mounted firmware file system.
  - [Firmware Modification Kit](https://code.google.com/archive/p/firmware-mod-kit/)
    - This kit is a collection of scripts and utilities to extract and rebuild linux based firmware images.
  - [Angr binary analysis framework](https://github.com/angr/angr)
    - angr is a platform-agnostic binary analysis framework.
    - a suite of Python 3 libraries that let you load a binary and do a lot of cool things to it:
      - Disassembly and intermediate-representation lifting
      - Program instrumentation
      - Symbolic execution
      - Control-flow analysis
      - Data-dependency analysis
      - Value-set analysis (VSA)
      - Decompilation
  - [Binwalk firmware analysis tool](https://github.com/ReFirmLabs/binwalk)
    - Binwalk is a fast, easy to use tool for analyzing, reverse engineering, and extracting firmware images.
  - [Binary Analysis Tool](http://www.binaryanalysis.org/old/content/show/tool)
    - The Binary Analysis Tool (BAT) makes it easier and cheaper to look inside binary code, find compliance issues, and reduce uncertainty when deploying Free and Open Source Software.
    - [binaryanalysis-ng(BANG)](https://github.com/armijnhemel/binaryanalysis-ng)
      - Binary Analysis Next Generation (BANG)
      - BANG is a framework for processing binary files (like firmware). It consists of an unpacker that recursively unpacks and classifies/labels files and separate analysis programs that work on the results of the unpacker.
  - [Firmadyne](https://github.com/firmadyne/firmadyne)
    - FIRMADYNE is an automated and scalable system for performing emulation and dynamic analysis of Linux-based embedded firmware.
  - Firmware Analysis Comparison Toolkit
  - [ByteSweep](https://gitlab.com/bytesweep/bytesweep)
    - A Free Software IoT Firmware Security Analysis Tool

## Vulnerable Firmware
- (for testing, analysis & study purpose)
  - [Damn Vulnerable Router Firmware](https://github.com/praetorian-inc/DVRF)
  - [OWASP IoTGoat](https://github.com/scriptingxss/IoTGoat)
    - The IoTGoat Project is a deliberately insecure firmware based on OpenWrt and maintained by OWASP to educate users how to test for the most common vulnerabilities found in IoT devices.


###### References:
- [Firmware Analysis Project](https://wiki.owasp.org/index.php/OWASP_Internet_of_Things_Project#tab=Firmware_Analysis)

