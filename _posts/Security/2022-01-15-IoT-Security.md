---
title:  "IoT Security: Do's & Don'ts"
date:   2022-01-15 12:33:22 +0530
permalink: /_posts/iot/security
categories:
  - IoT
  - Cloud
  - Security
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - IoT
  - Cloud
  - Security
author: Akhilesh Moghe
show_author_profile: true
---

## Challenges or Aspects of IoT Security
### *<u>Device exploitation</u>*
- IoT devices, by nature, are vulnerable.
- IoT fleets consist of devices that have diverse capabilities, are long-lived, and are geographically distributed.
- These characteristics, coupled with the growing number of devices, raise questions about how to address security risks posed by IoT devices.

### *<u>Limited Resources with IoT devices</u>*
- Many devices have a low level of compute, memory, and storage capabilities, which limit options for implementing security on devices.
- Even if you have implemented best practices for security in the cloud application, on the device side you might face limited opportunities to implement best-case security. Also, more you think of security techniques, more you demand resources & compromise on performance. So, you have to trade-off somewhere.
- On the other hand, Security is a constantly evolving area. To detect and mitigate exploits, organizations should consistently audit device settings and health.

### *<u>Security of the IoT devices</u>*
- To protect users, devices, and companies, IoT devices must be secured and protected.
- IoT security foundation begins with the ability to control & management of devices, and setting-up of secure connections between devices & to cloud & edge applications.
- Proper protection helps keep data private, restricts access to devices and cloud resources, offers secure ways to connect to the cloud, and audits device usage.
- An IoT security strategy should reduce vulnerabilities by using policies such as device identity management, encryption, and access control.

### *<u>Secure Communication</u>*
- Though communication creates responsive IoT applications, it can also expose IoT security exploits and open up channels for unauthorized users or accidental data leaks.
- With thousands of devices gathering data and communicating to each other, the security of the communications channel is an another critical part of your overall security plan.
- The ability to access data in transit and manipulate, delete, or acquire data as it traverses the network means that the protocols used to transfer the data must be able to inhibit unauthorized access.
- Using secure protocols enables authentication of the devices and ensures that the sender and receiver are who they say they are. This identification is crucial to maintaining the validity of the data.

### *<u>Software Security</u>*
- When discussing technology and security vulnerabilities, software security is often the first thing that comes to mind.
- The ability of an unauthorized user to access the software through a vulnerability is often a topic of discussion. Whether you have a handful of IoT devices or a fleet of thousands of devices.
- A prime planning discussion is how you are going to monitor, maintain, and update the device's software. Patching and maintenance of devices is critical to their security, and time should be taken to understand your IoT infrastructure, the number and types of devices, and how best to roll out patches and updates.

### *<u>Data Security</u>*
- IoT is all about collecting, analyzing, and acting on gathered data to make informed decisions.
- Along with a secure communication channel, it is important that data being generated is encrypted and secured both while moving between devices (in transit) and while in storage (at rest).
- Encrypting data ensures that any unauthorized user who gains access to the data cannot read or use the data.

### *<u>Skills gap</u>*
- Hardware engineers traditionally lack the skills to implement proper integration between the cloud and the back end application.
- Security engineers do not typically understand hardware development well enough to assist the hardware engineers.

## DON'Ts of IoT Security:
- __*OWASP IoT Top 10*__ report represents the top ten things to avoid when building, deploying, or managing IoT systems.
<object data="/assets/docs/iot/security/OWASP-IoT-Top-10-2018.pdf" type="application/pdf" width="800px" height="1165px">
  <embed src="/assets/docs/iot/security/OWASP-IoT-Top-10-2018.pdf">
      <p>This browser does not support PDFs. Please download the PDF to view it: <a href="/assets/docs/iot/security/OWASP-IoT-Top-10-2018.pdf">Download PDF</a>.</p>
  </embed>
</object>


## DO's of IoT Security:
- Following are the best practices to help you protect the IoT ecosystem, from design and implementation to ongoing operations and management. It's also has a list of high-level recommendations follows each rule:
- Best Practicses at a glance:
  1. Provision devices and systems with unique identities and credentials.
  2. Apply authentication and access control mechanisms.
  3. Use cryptographic network protocols.
  4. Create continuous update and deployment mechanisms.
  5. Deploy security auditing and monitoring mechanisms.
  6. Build continuous health checks for security mechanisms.
  7. Proactively assess the impact of potential security events.
  8. Minimize the attack surface of your IoT ecosystem.
  9. Avoid unnecessary data access, storage, and transmission.
  10. Monitor vulnerability disclosure and threat intelligence sources.

<object data="/assets/docs/iot/security/Ten-security-golden-rules-for-IoT-solutions-AWS-IoT-Official-Blog.pdf" type="application/pdf" width="800px" height="1100px">
  <embed src="/assets/docs/iot/security/Ten-security-golden-rules-for-IoT-solutions-AWS-IoT-Official-Blog.pdf">
      <p>This browser does not support PDFs. Please download the PDF to view it: <a href="/assets/docs/iot/security/Ten-security-golden-rules-for-IoT-solutions-AWS-IoT-Official-Blog.pdf">Download PDF</a>.</p>
  </embed>
</object>

###### References
- [AWS IoT Security Primer - Module-1]()
- [Ten-security-golden-rules-for-IoT-solutions-AWS-IoT-Official-Blog](https://aws.amazon.com/blogs/iot/ten-security-golden-rules-for-iot-solutions/)
- [OWASP-IoT-Top-10-2018](https://wiki.owasp.org/index.php/OWASP_Internet_of_Things_Project#tab=Main)


