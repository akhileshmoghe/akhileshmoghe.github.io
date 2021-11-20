---
title:  "SSL TLS Explained"
date:   2020-03-26 12:33:22 +0530
categories:
  - Security
  - SSL
  - TLS
  - HTTPS
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Security
  - SSL
  - TLS
  - HTTPS
author: Akhilesh Moghe
show_author_profile: true
---

## History:
* First SSL prototypes came from Netscape when they were developing the first versions of their flagship browser, Netscape Navigator.
* __*<u>SSL version 3</u>*__ (SSLv3) was an enhanced protocol which <u>still works today</u> and is widely supported. The protocol has been standardized, with a new name in order to avoid legal issues; the new name is __*<u>TLS</u>*__.
* Three versions of TLS have been produced so far, each with its dedicated RFC: TLS 1.0, TLS 1.1 and TLS 1.2.
  * SSLv3 is then 3.0
  * while the TLS versions are, respectively, 3.1, 3.2 and 3.3.
  * Thus, TLS 1.0 is sometimes called SSL 3.1. (SSL 3.0 and TLS 1.0 differ by only some minute details.)
  * TLS 1.1 and 1.2 are not yet widely supported
  * SSLv3 and TLS 1.0 are supported "everywhere" (even IE 6.0 knows them).
  
## Context 
* SSL aims at providing a secure bidirectional tunnel for arbitrary data.
### TCP in brief:
* TCP is the well-known protocol for sending data over the Internet.
* TCP works over the IP "packets" and provides a bidirectional tunnel for bytes; it works for every byte values and sends them into two streams which can operate simultaneously.
* TCP handles the hard work of splitting the data into packets, acknowledging them, reassembling them back into their right order, while removing duplicates and reemitting lost packets.
* From the point of view of the application which uses TCP, there are just two streams, and the packets are invisible; in particular, the streams are not split into "messages" (it is up to the application to take its own encoding rules if it wishes to have messages, and that's precisely what HTTP does).

### Why SSL/TLS:
* TCP is reliable in the presence of "accidents", i.e. transmission errors due to flaky hardware, network congestion, people with smartphones who walk out the range of a given base station, and other non-malicious events.
* However, an ill-intentioned individual (the "attacker") with some access to the transport medium could read all the transmitted data and/or alter it intentionally, and TCP does not protect against that.
* Hence SSL.
* SSL assumes that it works over a TCP-like protocol, which provides a reliable stream;
* SSL does not implement reemission of lost packets and things like that. The attacker is supposed to be in power to disrupt communication completely in an unavoidable way (for instance, he can cut the cables)
* so SSL's job is to:
  * detect alterations (the attacker must not be able to alter the data silently);
  * ensure data confidentiality (the attacker must not gain knowledge of the exchanged data).
  * SSL fulfills these goals to a large (but not absolute) extent.

## Record Protocol:
* SSL is layered and the <u>bottom layer</u> is the __*record protocol*__.
* Whatever data is sent in an SSL tunnel is split into records.
* Over the wire (the underlying TCP socket or TCP-like medium), a record looks like this:\
  ``` HH V1:V2 L1:L2 data ```
  * *<u>HH</u>* is a *single byte* which indicates <u>the type of data</u> in the record.
    * Four types are defined:
      * __*change_cipher_spec*__ (20)
      * __*alert*__ (21)
      * __*handshake*__ (22)
      * __*application_data*__ (23)
  * *<u>V1: V2</u>* is the __*protocol version*__, over *two bytes*.
    For all versions currently defined,
    * <u>V1 has value 0x03</u>, while <u>V2 has value 0x00</u> for __*SSLv3*__,
    * 0x01 for TLS 1.0
    * 0x02 for TLS 1.1
    * 0x03 for TLS 1.2
  * *<u>L1: L2</u>* is the __*length of data*__, in *bytes* (big-endian convention is used: the length is 256*L1+L2).
    * The <u>total length of data cannot exceed 18432 bytes</u>, but in practice, it cannot even reach that value.
* So a record has a *<u>five-byte header</u>*, followed by at most *<u>18 kB of data</u>*.
* The data is where *<u>symmetric encryption</u>* and *<u>integrity checks</u>* are applied.
* When a record is emitted, both sender and receiver are supposed to agree on which cryptographic algorithms are currently applied, and with which keys; this agreement is obtained through the *handshake protocol*. *<u>Compression</u>*, if any, is also applied at that point.

### Record protocol works like this:
  * Initially, there are some bytes to transfer; these are application data or some other kind of bytes. This *<u>payload consists of at most 16384 bytes</u>*, but possibly less (a payload of length 0 is legal, but it turns out that Internet Explorer 6.0 does not like that at all).
  * The *<u>payload is then compressed</u>* with whatever compression algorithm is currently agreed upon.
    * <u>Compression is *stateful* and thus may depend upon the contents of previous records.</u>
    * In practice, compression is either "null" (no compression at all) or "Deflate" (RFC 3749),
      * the latter being currently courteously but firmly shown the exit door in the Web context, due to the recent CRIME attack.
    * Compression aims at shortening data, but it must necessarily expand it slightly in some unfavorable situations (due to the pigeonhole principle).
    * SSL allows for an expansion of at most 1024 bytes. Of course, null compression never expands (but never shortens either); Deflate will expand by at most 10 bytes if the implementation is any good. 

  * The compressed payload is then protected against alterations and *<u>encrypted</u>*.
    * If the current encryption-and-integrity algorithms are "null", then this step is a no-operation.
    * Otherwise, a MAC is appended, then some padding (depending on the encryption algorithm), and the result is encrypted.
    * These steps again induce some expansion, which the SSL standard limits to 1024 extra bytes (combined with the maximum expansion from the compression step, this brings us to the 18432 bytes, to which we must add the 5-byte header). 

    * The MAC is, usually, __*<u>HMAC</u>*__ with one of the usual hash functions (mostly *<u>MD5, SHA-1 or SHA-256</u>*)(with SSLv3, this is not the "true" HMAC but something very similar and, to the best of our knowledge, as secure as HMAC).
    * __*Encryption*__ will use either a *<u>block cipher in CBC mode</u>*, or the *<u>RC4 stream cipher</u>*.
    * Crucially, the MAC is first computed and appended to the data, and the result is encrypted.
      * This is MAC-then-encrypt and it is actually not a very good idea. The MAC is computed over the concatenation of the (compressed) payload and a sequence number, so that an industrious attacker may not swap records.

## Handshake:
* The handshake is a protocol which is played within the record protocol.
* Its goal is to *<u>establish the algorithms and keys which are to be used for the records</u>*.
* It consists of messages. Each handshake message begins with a *<u>four-byte header</u>*,
  - *<u>one byte</u>* which describes the __*message type*__,
  - then *<u>three bytes</u>* for the __*message length*__ (big-endian convention).
* The successive handshake messages are then sent with records tagged with the "handshake" type (the first byte of the header of each record has value 22). 

* Initially, client and server "agree upon" *<u>null encryption with no MAC</u>* and *<u>null compression</u>*. This means that the record they will first send will be sent as *<u>cleartext and unprotected</u>*.

### Full Handshake:
* The full handshake looks like this: 
  ![TLS-Handshake](/assets/images/TLS-Handshake.png)
  
* __*<u>ClientHello</u>*__:
  * The first message of a handshake is a __*<u>ClientHello</u>*__.
    * It is the message by which the client states its intention to do some SSL.
      (Note that "client" is a symbolic role; it means "the party which speaks first".)
    * The ClientHello message contains: 
    * the __*maximum protocol version*__ that the client wishes to support; 
    * the __*client random*__ (32 bytes, out of which 28 are supposed to be generated with a *<u>cryptographically strong number generator</u>*);
    * the __*session ID*__ (in case the client wants to resume a session in an abbreviated handshake, see below);
    * the list of __*cipher suites*__ that the client knows of, *<u>ordered by client preference</u>*;
    * the list of __*compression algorithms*__ that the client knows of, *<u>ordered by client preference</u>*; 
    * some optional extensions.

* __*<u>ServerHello</u>*__:
  * The server responds to the *<u>ClientHello</u>* with a __*<u>ServerHello</u>*__ which contains:
    * the __*protocol version*__ that the client and server will use;
    * the __*server random*__ (32 bytes, with 28 random bytes);
    * the __*session ID*__ for this connection;
    * the __*cipher suite*__ that will be used;
    * the __*compression algorithm*__ that will be used;
    * optionally, some extensions.

* Besides *<u>ClientHello</u>* and *<u>ServerHello</u>*, the server sends a few other messages, which depend on the cipher suite and some other parameters:
  * __*Certificate*__: the server's certificate, which contains its *<u>public key</u>*. This message is almost always sent, except if the cipher suite mandates a handshake without a certificate. 
  * __*ServerKeyExchange*__: some extra values for the key exchange, if what is in the certificate is not sufficient. In particular, the __*<u>DHE cipher suites</u>*__ use an *<u>ephemeral Diffie-Hellman key exchange</u>*, which requires that message. 
  * __*CertificateRequest*__: a message to request the client to also identify itself with a certificate of its own. This message contains the list of names of trust anchors (aka "root certificates") that the server will use to validate the client certificate.
  * __*ServerHelloDone*__: a marker message (of length zero) which says that the server is finished, and the client should now talk.
* The *<u>client</u>* must then respond with:
  * __*Certificate*__: the client certificate, if the server requested one. There are subtle variations between versions (with SSLv3, the client must omit this message if it does not have a certificate; with TLS 1.0+, in the same situation, it must send a Certificate message with an empty list of certificates).
  * __*ClientKeyExchange*__: the client part of the actual key exchange (e.g. *<u>some random value encrypted with the server RSA key</u>*).
  * __*CertificateVerify*__: a *<u>digital signature computed by the client over all previous handshake messages</u>*. This message is sent when the server requested a client certificate, and the client complied. *<u>This is how the client proves to the server that it really "owns" the public key</u>* which is encoded in the certificate it sent.
* Initial Handshake is over by now.
* With following messages Client and Server will switch over to encrypted communication.
  * __*ChangeCipherSpec*__: Then the client sends a ChangeCipherSpec message, which is not a handshake message: it has its own record type, so it will be sent in a record of its own. <u>Its contents are purely symbolic</u> (a single byte of value 1). This message marks the *<u>point at which the client switches to the newly negotiated cipher suite and keys</u>*. <u>The subsequent records from the client will then be</u> __*<u>encrypted</u>*__.
  * __*Finished*__: The Finished message is a __*<u>cryptographic checksum</u>*__ <u>computed over all previous handshake messages</u> (from both the client and server). Since it is emitted after the ChangeCipherSpec, it is also *<u>covered by the</u>* __*<u>integrity check</u>*__ and the __*<u>encryption</u>*__.
    * When the server receives __Finished__ message and verifies its contents, it obtains a proof that it has indeed talked to the same client all along. This message protects the handshake from alterations (the attacker cannot modify the handshake messages and still get the __Finished__ message right).
* The server finally responds with its own __*ChangeCipherSpec*__ then __*Finished*__.
* At that point, the handshake is finished, and the client and server may exchange application data (in encrypted records tagged as such).

### Cipher Suite:
* A cipher suite is a *<u>16-bit symbolic identifier</u>* for a set of cryptographic algorithms.
* For instance, the __*TLS_RSA_WITH_AES_128_CBC_SHA*__ cipher suite has value 0x002F,
  * and means records use *<u>HMAC/SHA-1</u>* and *<u>AES encryption with a 128-bit key</u>*,
  * and the key exchange is done by *<u>encrypting a random key</u>* with the server's *<u>RSA public key</u>*".
* the client suggests but the server chooses.
* The cipher suite is in the hands of the server. Courteous servers are supposed to follow the preferences of the client (if possible), but they can do otherwise and some actually do (e.g. as part of protection against BEAST).

### Abbreviated Handshake:
* In the full handshake, the server sends a "session ID" (i.e. a bunch of up to 32 bytes) to the client.
* Later on, the *<u>client</u>* can come back and send the __*<u>same session ID</u>*__ as part of his ClientHello. This means that the *<u>client still remembers the cipher suite and keys from the previous handshake</u>* and would like to reuse these parameters.
* If the *<u>server</u>* also remembers the cipher suite and keys, then it copies that __*<u>specific session ID</u>*__ in its ServerHello, and then follows the abbreviated handshake:
  ![Abbreviated Handshake](/assets/images/TLS-abbreviated-handshake.png)
* The abbreviated handshake is shorter: fewer messages, __*<u>no asymmetric cryptography business</u>*__, and, most importantly, __reduced latency__.
* A typical Web browser will open an SSL connection with a full handshake, then do abbreviated handshakes for all other connections to the same server: the other connections it opens in parallel, and also the subsequent connections to the same server.
* Indeed, typical *<u>Web servers will close connections after 15 seconds of inactivity</u>*, but they will *<u>remember sessions (the cipher suite and keys) for a lot longer</u>* (possibly for hours or even days).

### Handshake Re-initiation:
* At any time, the client or the server can initiate a new handshake (the server can send a __*<u>HelloRequest</u>*__ message to trigger it; the client just sends a __*<u>ClientHello</u>*__).
* A typical situation is the following:
  * An HTTPS server is configured to listen to SSL requests. 
  * A client connects and a handshake is performed. 
  * Once the handshake is done, the client sends its "applicative data", which consists of an *<u>HTTP request</u>*. __At that point__ (and at that point only), the server learns the target path. Up to that point, the URL which the client wishes to reach was unknown to the server (the server might have been made aware of the target server name through a Server Name Indication SSL extension, but this does not include the path).
  * Upon seeing the path, the server may learn that *<u>this is for a part of its data which is supposed to be accessed only by clients authenticated with certificates</u>*. But the server did not ask for a client certificate in the handshake (in particular because not-so-old Web browsers displayed freakish popups when asked for a certificate, in particular, if they did not have one, so a server would refrain from asking a certificate if it did not have good reason to believe that the client has one and knows how to use it).
  * Therefore, the server triggers a new handshake, this time requesting a certificate.


