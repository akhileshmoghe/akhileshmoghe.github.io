---
title:  "Container Security"
date:   2021-10-07 12:33:22 +0530
permalink: /_posts/containers/container_security
categories:
  - Containers
  - Docker
  - Kubernetes
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Containers
  - Docker
  - Kubernetes
author: Akhilesh Moghe
show_author_profile: true
---


## Introduction

## Threats and Risks Areas:

### Image Development:
* Careless approach: 
  * Installing components, applications without security safeguards or with default configs. 
* Sensitive Data in DockerFiles: 
  * Password hardcoding 
  * Default passwords 
  * SSH encryption keys (specifically private part of keys) 
* Unreliable third-party source of Base Images: 
  * Embedded Malwares: 
    * Owner added Malware in image with malicious intents 
* Non-updated Images: 
  * Images not being maintained with Vulnerability patches 
* Unreliable source of package installations: 
  * ppa repositories 
  * Cascaded dependencies 

### Code Repository:
* Open-source unmaintained repositories/projects 
* Cascaded dependencies project repositories 

### Image Registries Hub:
* Unsecured Public Registry 
* Local unsecured registry setup 
* Misconfigured security controls on registries 

### Kubernetes/Docker APIs Abuse:
* Vulnerabilities in Kubernetes or Docker APIs itself. 
  * CVE-2017-1002101 allows containers that use subPath volume mounts to access files or directories outside of the volume, including the host’s file system.
  * CVE-2017-1002102 allows containers using secret, configMap, or projected or downwardAPI volume to trigger deletion of arbitrary files and directories on the nodes where they are running. 
  * CVE-2018-1002105, proxy request handling in kube-apiserver, can leave vulnerable TCP connections open to abuse. 
* Unrestricted Accesses: 
  * Admin accesses missuses. 
* Unauthorized Accesses 

### Applications and Ports:
* Misconfigured Applications and Ports 
  * May give-out sensitive information 
* Application Exploitation with: 
  * SQL injection 
  * cross-site scripting 
  * remote file inclusion 
  * brute-force attacks. 
* API Abuse with misconfigured/Exposed ports 


## Best Practises:
### Use of User Namespaces in Linux:
* when enabled, allows for <u>container isolation by limiting container access to system resources</u>.

### Run Applications as regular users in containers also:
* Setting applications to run as regular users can stop privilege escalation attacks from accessing the critical parts of the container. 

### SELinux/AppArmor:
* Mandatory access control (MAC) tools such as SELinux (Security-Enhanced Linux) and AppArmor can help prevent attacks that compromise application and system services by <u>limiting access to files and network resources</u>.

### Minimize use of third-party, unmaintained, unverifiable installs:

### Mount Root File System as Read-Only on Host:
* will restrict write access for applications, limiting the chances of an attacker being able to introduce malicious elements to the container. 

### Scan Repository Images for new Vulnerabilities being reported:
* When building an image in your CI pipeline, image scanning must be a requirement for a passing build run. 
* Scanners: 
  * Many scanners check only installed operating system packages. Others may also scan installed runtime libraries for some programming languages. Some may also provide additional binary fingerprinting or other testing of file contents. 
  * Make sure your scanner offers a compatible API or tool that you can plug into your CI pipeline and provides the data you need to evaluate your criteria for failing a build.
* determine acceptable levels of risk for allowing a build to pass such as any vulnerability below a certain severity - or alternatively, failing builds with fixable vulnerabilities above a certain severity. 

### Runtime Image Scanning:
* Runtime scanning is more important, both for any third-party image you may use and for your own images, which may contain __newly discovered security vulnerabilities__.
* You can use custom or third-party [<u>admission controllers</u>](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/) in Kubernetes clusters <u>to prevent the scheduling of insecure container images</u>. 
* While some scanners support storing __scan output in a database or cache__, users will have to weigh their tolerance for outdated information __against the latency introduced by performing a real-time scan for each image pull__. 

### Deploy Firewall: 
* Using the integrated firewall for the Docker virtual network, especially for TCP API control, can filter external attacks. 
* Use of Application firewalls. 
* Allow only required network ingress. 

### Use of static analysis tools for containers: 
* [<u>Clair</u>](https://quay.github.io/clair/)
  * Prevents vulnerability with Static analysis on containers 

### Disable Sockets: 
* UNIX socket is a two-way communication mechanism that allows the host to communicate with the containers. 
* Disabling this socket can thwart attacks that exploit it — for example, an attacker abusing the API from inside a container. 

### Use of Different/Distributed Databases for applications: 
* Reduces the attack vector 

### Regular updates to Host OS and Kernel: 

### Limit Administrative accesses to Build/CI infrastructure: 

### Remove Package Installers such as APT, YUM, even shells if possible: 
* This might reduce attack vector, as in any compromised container, attacker will not be able to install malicious software, packages. 
* [<u>Google Distroless</u>](https://github.com/GoogleContainerTools/distroless)
  * "Distroless" images contain only your application and its runtime dependencies. They do not contain package managers, shells or any other programs you would expect to find in a standard Linux distribution. 
  * Restricting what's in your runtime container to precisely what's necessary for your app is a best practice. 
  * Distroless images are very small. 
* Keeping the image as minimal as possible has the added bonus of reducing the probability of encountering zero-day vulnerabilities – which will need patching – and keeping the image smaller, which makes storing and pulling it faster. 

### Use of Signed and Verifiable Container Images: 
* With image signing, the <u>registry generates a __checksum__</u> of a tagged image’s contents and then uses a __private cryptographic key__ to create an encrypted signature with the image metadata. 
* Clients could still pull and run the image without verifying the signature, but runtimes in secure environments should support image verification requirements. 
* <u>Image verification uses the __public key counterpart__ of the signing key to decrypt</u> the contents of the image signature, which can then be compared to the pulled image to ensure the image’s contents have not been modified. 
* [<u>cosign</u>](https://github.com/sigstore/cosign)
  * Container Signing, Verification and Storage in an OCI registry. 
  * Cosign aims to make signatures invisible infrastructure. 
  * Cosign supports: 
    * Hardware and KMS signing 
    * Bring-your-own PKI 
  * [<u>Detailed Usage of cosign</u>](https://github.com/sigstore/cosign/blob/main/USAGE.md)

### Avoid installing: 
* Restricting the image to required binaries, libraries, and configuration files provides the best protection. 
* <u>Package managers</u>:
  * apt, yum, apk 
* <u>Network tools and clients</u>:
  * wget, curl, netcat, ssh. 
  * If you normally use curl to download, say, a configuration file, make the contents into a <u>__Kubernetes ConfigMap__</u> instead.
* <u>Unix shells</u>:
  * sh, bash. 
  * Note that removing shells also prevents the use of shell scripts at runtime. Instead, __use a compiled language when possible__.
* <u>Compilers and debuggers</u>: 
  * These should be used only in build and development containers, but never in production containers. 
  * Kubernetes also currently (as of version 1.16) has alpha support for [<u>ephemeral containers</u>](https://kubernetes.io/docs/concepts/workloads/pods/ephemeral-containers/), which can be placed in an existing pod to facilitate debugging. 
    * If you install these tools in your production images to perform application debugging. 

### Build vs Runtime Containers should be different: 
* build tools you use to generate and compile your applications can be exploited when run on production systems. 
* Containers should be treated as temporary, ephemeral entities. 
* Never plan on “patching” or altering a running container. 
* Build a new image and replace the outdated container deployments. 
* Use multi-stage Dockerfiles to keep software compilation out of runtime images. 

### Never bake any secrets into your images, even if the images are for internal use: 
* TLS certificate keys 
* cloud provider credentials 
* SSH private keys 
* database/user passwords 
* Supplying sensitive data only at runtime also enables you to use the same image in different runtime environments, which should use different credentials. 
* It also simplifies updating expired or revoked secrets without rebuilding the image. 
* As an alternative to baked-in secrets, <u>supply secrets to Kubernetes pods as</u> __*Kubernetes secrets*__, or use another secret management system. 

### Image Registry and Controls: 
* Registries which support using __*immutable tags on images*__, <u>preventing the same tag from being reused on multiple versions</u> of a repository’s image, enforce __*deterministic image runtimes*__.
* Many audited certifications and site reliability teams require the ability to know exactly which version of a given image, and therefore of an application, is deployed at a given time, a situation which is impossible when every image pull uses the latest tag. 
* Kubernetes does not offer native support for using __*secure image pull*__ options. You will need to deploy a [<u>Kubernetes admission controller</u>](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/) that can <u>verify that pods use trusted registries</u>. 
* For signed image support, the controller would need to be able to verify the image’s signature. 

### Maintainability: Vulnerability Management: 
* Generate your own policies and procedures for handling image security and vulnerability management. Start by defining your criteria for what constitutes an unsafe image, using metrics such as: 
  * vulnerability __*severity*__
  * __*number*__ of vulnerabilities 
  * whether those vulnerabilities have __*patches or fixes available*__
  * whether the vulnerabilities impact __*misconfigured deployments
* Set a __*deadline*__ for building replacement images and deploying those to production. 
  * Should the deadlines vary by vulnerability severity? 
* Decide if you want to block the scheduling of containers from existing images when a new vulnerability is discovered? You will also need to define procedures to handle containers with these vulnerable images that are already running in production. 

## References:
[Container Security: Examining Potential Threats to the Container Environment](https://www.trendmicro.com/vinfo/us/security/news/security-technology/container-security-examining-potential-threats-to-the-container-environment)\
[Container Image Security: Beyond Vulnerability Scanning](https://cloud.redhat.com/blog/container-image-security-beyond-vulnerability-scanning)\
[StackRox: Container and Kubernetes Security Eval Guide.pdf](/assets/docs/container-and-kubernetes-security-eval-guide-v3.pdf)


