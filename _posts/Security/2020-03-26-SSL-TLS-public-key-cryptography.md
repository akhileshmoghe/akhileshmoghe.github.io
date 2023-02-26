---
title:  "SSL TLS: Public Key Cryptography"
date:   2020-03-26 12:33:22 +0530
permalink: /_posts/security/tls_pkc
categories:
  - HTTPS
  - Secure Communication
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - HTTPS
  - Secure Communication
author: Akhilesh Moghe
show_author_profile: true
---

# Overview
  In this blog, we will explore how a Public key is created with RSA Asymmetric Key Encryption in an oversimplified version. We will know, how a shared secrete is shared between server and client, which is then used for further encrypted commnication after initial handshake.

## Magic of Public Key Cryptography:
Here's an oversimplified version of RSA Asymmetric Key Encryption:

* Let __*<u>n</u>*__ be a big integer (say 300 digits);
* __*<u>n</u>*__ is chosen such that it is a *<u>product of two prime numbers of similar sizes</u>* (let's call them __*<u>p</u>*__ and __*<u>q</u>*__).
* We will then compute things __*<u>modulo n</u>*__: this means that whenever we *<u>add or multiply together two integers</u>*, we *<u>divide the result by n</u>* and we *<u>keep the remainder</u>* (which is between 0 and n-1, necessarily).
  * Given *x*, computing __*<u>x<sup>3</sup> modulo n</u>*__ is easy:
    * you multiply x with x and then again with x, and then you divide by n and keep the remainder. Everybody can do that.
  * On the other hand, given __*<u>x<sup>3</sup> modulo n</u>*__, <u>recovering x seems overly difficult</u> (the best known methods being far too expensive for existing technology)
  * -- unless you know __*<u>p</u>*__ and __*<u>q</u>*__, in which case it becomes easy again.
  * But computing __*<u>p</u>*__ and __*<u>q</u>*__ from __*<u>n</u>*__ seems hard, too (it is the problem known as [integer factorization](http://en.wikipedia.org/wiki/Integer_factorization)).\
&nbsp;
  
* So here is what the server and client do:
  * The server has a __*<u>n</u>*__ and knows the corresponding __*<u>p</u>*__ and __*<u>q</u>*__ (it generated them). The server sends __*<u>n</u>*__ to the client.
  * The client chooses a *<u>random x</u>* and computes __*<u>x<sup>3</sup> modulo n</u>*__.
  * The client sends __*<u>x<sup>3</sup> modulo n</u>*__ to the server.
  * The server uses its knowledge of __*<u>p</u>*__ and __*<u>q</u>*__ to recover __*<u>x</u>*__.
  * At that point, both client and server know __*<u>x</u>*__. But an eavesdropper saw only __*<u>n</u>*__ and __*<u>x<sup>3</sup> modulo n</u>*__; he cannot recompute __*<u>p</u>*__, __*<u>q</u>*__ and/or __*<u>x</u>*__ from that information.\
&nbsp;

* So __*<u>x</u>*__ is a __*<u>shared secret</u>*__ between the client and the server.
* __*<u>n</u>*__ is the __*<u>RSA Public Key</u>*__ sent by the server.
* After that this is pretty straightforward *<u>symmetric encryption</u>*, using __*<u>x</u>*__ a __*<u>shared secret</u>*__ as key.\
&nbsp;

### Further Reading
If interested in knowing actual RSA operations like Key-generation, Key-distribution, Encryption, Decryption; see [Wikipedia RSA link](https://en.wikipedia.org/wiki/RSA_(cryptosystem)#Operation).

## A Cretificate
* The certificate is a vessel for the server public key (__*<u>n</u>*__).
* It is used to thwart active attackers who would want to impersonate the server: such an attacker intercepts the communication and sends its value __*<u>n</u>*__ instead of the server's __*<u>n</u>*__.
* The certificate is signed by a __*<u>certification authority</u>*__, so that the client may know that a given __*<u>n</u>*__ is really the genuine __*<u>n</u>*__ from the server he wants to talk with.
* Digital signatures also use *<u>asymmetric cryptography</u>*, although in a distinct way (for instance, there is also a variant of __RSA__ for digital signatures).

\
&nbsp;
## References
  - [How is it possible that people observing an HTTPS connection being established wouldn't know how to decrypt it?](https://security.stackexchange.com/a/6296/214329)
  
