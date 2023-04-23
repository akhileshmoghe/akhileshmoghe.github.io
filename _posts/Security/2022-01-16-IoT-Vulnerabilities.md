---
title:  "Vulnerabilities & Attack Surfaces in IoT Ecosystem"
date:   2022-01-16 12:33:22 +0530
permalink: /_posts/iot/vulnerabilities
categories:
  - IoT
  - Cloud Computing
  - IoT Security
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - IoT
  - Cloud Computing
  - IoT Security
author: Akhilesh Moghe
show_author_profile: true
sidebar:
 nav: IoT
---

This article provides reasons for the top IoT Vulnerabilities, the attack surface of these vulnerabilities.

# IoT Vulnerabilities:
## 1.  Username Enumeration
- It's the Ability to collect a set of valid usernames by interacting with the authentication mechanism
- Attach Surface:
  - Administrative Interface
  - Device & Web Interface
  - Cloud Interface
  - Mobile Application
  
## 2.  Weak Passwords
- Ability to set account passwords to '1234' or '123456' for example.
- Usage of pre-programmed default passwords
- Attack Surface:
  - Administrative Interface
  - Device & Web Interface
  - Cloud Interface
  - Mobile Application
- Example: [The Mirai Botnet (aka Dyn Attack)](https://www.theguardian.com/technology/2016/oct/26/ddos-attack-dyn-mirai-botnet)

## 3.  Account Lockout
- Ability to continue sending authentication attempts after 3 - 5 failed login attempts
- Attack Surface:
  - Administrative Interface
  - Device & Web Interface
  - Cloud Interface
  - Mobile Application

## 4.  Unencrypted Services
- Network services are not properly encrypted to prevent eavesdropping or tampering by attackers
- Attack Surface:
  - Device Network Services

## 5.  Two-factor Authentication
- Lack of two-factor authentication mechanisms such as a security token or fingerprint scanner
- Attack Surface:
  - Administrative Interface
  - Cloud & Web Interface
  - Mobile Application

## 6.  Poorly Implemented Encryption
- Encryption is implemented however it is improperly configured or is not being properly updated, e.g. using SSL v2
- Attack Surface:
  - Device Network Services

## 7.  Update Sent Without Encryption
- Updates are transmitted over the network without using TLS or encrypting the update file itself
- Attck Surface:
  - Update Mechanism

## 8.  Update Location Writable
- Storage location a.k.a repository/artifactory for update files is writable by anyone, potentially allowing firmware to be modified and distributed to all users
- Attck Surface:
  - Update Mechanism

## 9.  Denial of Service
- Service can be attacked in a way that denies service to that service or the entire device
- Attck Surface:
  - Device Network Services

## 10. Removal of Storage Media
- Ability to physically remove the storage media from the device
- Attck Surface:
  - Device Physical Interfaces

## 11. No Manual Update Mechanism	
- No ability to manually force an update check for the device
- Attack Surface:
  - Update Mechanism

## 12. Missing Update Mechanism
- No ability to update device
- Attck Surface:
  - Update Mechanism

## 13. Firmware Version Display and/or Last Update Date	
- Current firmware version is not displayed and/or the last update date is not displayed
- Attack Surface:
  - Device Firmware

## 14. Firmware and storage extraction
- Firmware contains a lot of useful information, like source code and binaries of running services, pre-set passwords, ssh keys etc.
- Attack Surface:
  - JTAG / SWD interface
  - In-Situ dumping i.e. replacement of the original firmware by fradulant one at the same location.
  - Intercepting a OTA update
  - Downloading from the manufacturers web page
  - eMMC tapping
  - Unsoldering the SPI Flash / eMMC chip and reading it in a adapter
- Example:
  - [Shining a Light on SolarCity: Practical Exploitation of the X2e IoT Device](https://www.mandiant.com/resources/solarcity-exploitation-of-x2e-iot-device-part-one)

## 15. Manipulating the code execution flow of the device
- With the help of a JTAG adapter and gdb we can modify the execution of firmware in the device and bypass almost all software based security controls.
- Side channel attacks can also modify the execution flow or can be used to leak interesting information from the device
- Attack Surface:
  - JTAG / SWD interface
  - Side channel attacks like glitching

## 16. Obtaining console access
- By connecting to a serial interface, we will obtain full console access to a device
- Usually security measures include custom bootloaders that prevent the attacker from entering single user mode, but that can also be bypassed.
- Attack Surface:
  - Serial interfaces (SPI / UART)

## 17. Insecure 3rd party components
- Out of date versions of busybox, openssl, ssh, web servers, etc.
- Attack Surface:
  - Software


# IoT Attack Surface Areas
  
  | Attack Surfaces    | Vulnerabilities |
  |--------------------|:----------------|
  | __*IoT Ecosystem*__ | :small_orange_diamond: Interoperability standards <br/> :small_orange_diamond: Data governance <br/> :small_orange_diamond: System wide failure <br/> :small_orange_diamond: Individual stakeholder risks <br/> :small_orange_diamond: Implicit trust between components <br/> :small_orange_diamond: Enrollment security <br/> :small_orange_diamond: Decommissioning system <br/> :small_orange_diamond: Lost access procedures |
  | __*Device Memory*__ | :small_orange_diamond: Sensitive data <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_orange_diamond: Cleartext usernames <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_orange_diamond: Cleartext passwords <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_orange_diamond: Third-party credentials <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_orange_diamond: Encryption keys |
  | __*Device Physical Interfaces*__ | :small_orange_diamond: Firmware extraction <br/> :small_orange_diamond: User CLI <br/> :small_orange_diamond: Admin CLI <br/> :small_orange_diamond: Privilege escalation <br/> :small_orange_diamond: Reset to insecure state <br/> :small_orange_diamond: Removal of storage media <br/> :small_orange_diamond: Tamper resistance <br/> :small_orange_diamond: Debug port <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_orange_diamond: UART (Serial) <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_orange_diamond: JTAG / SWD <br/> :small_orange_diamond: Device ID/Serial number exposure |
  | __*Device Web Interface*__ | :small_orange_diamond: Standard set of web application vulnerabilities, see: <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_orange_diamond: [OWASP Web Top 10](https://wiki.owasp.org/index.php/Category:OWASP_Top_Ten_Project) <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_orange_diamond: [OWASP ASVS](https://wiki.owasp.org/index.php/Category:OWASP_Application_Security_Verification_Standard_Project) <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_orange_diamond: [OWASP Testing guide](https://wiki.owasp.org/index.php/Category:OWASP_Testing_Project) <br/> :small_orange_diamond: Credential management vulnerabilities: <br/> :small_orange_diamond: Username enumeration <br/> :small_orange_diamond: Weak passwords <br/> :small_orange_diamond: Account lockout <br/> :small_orange_diamond: Known default credentials <br/> :small_orange_diamond: Insecure password recovery mechanism |
  | __*Device Firmware*__ | :small_orange_diamond: Sensitive data exposure (See [OWASP Top 10 - A6 Sensitive data exposure](https://wiki.owasp.org/index.php/Top_10_2013-A6-Sensitive_Data_Exposure)): <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_orange_diamond: Backdoor accounts <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_orange_diamond: Hardcoded credentials <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_orange_diamond: Encryption keys <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_orange_diamond: Encryption (Symmetric, Asymmetric) <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_orange_diamond: Sensitive information <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_orange_diamond: Sensitive URL disclosure <br/> :small_orange_diamond: Firmware version display and/or last update date <br/> :small_orange_diamond: Vulnerable services (web, ssh, tftp, etc.) <br/> &nbsp;&nbsp;&nbsp;&nbsp; :small_orange_diamond: Verify for old sw versions and possible attacks (Heartbleed, Shellshock, old PHP versions etc) <br/> :small_orange_diamond: Security related function API exposure <br/> :small_orange_diamond: Firmware downgrade possibility |
  | 


###### References:
- [IoT Vulnerabilities Project](https://wiki.owasp.org/index.php/OWASP_Internet_of_Things_Project#tab=IoT_Vulnerabilities)
- [IoT Attack Surface Areas Project](https://wiki.owasp.org/index.php/OWASP_Internet_of_Things_Project#tab=IoT_Attack_Surface_Areas)


