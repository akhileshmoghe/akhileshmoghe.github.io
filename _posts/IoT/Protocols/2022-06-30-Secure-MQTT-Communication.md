---
title:  "Securing MQTT Communication with Username-Password & TLS/SSL Certificates using Mosquitto MQTT Broker"
date:   2022-06-30 12:33:22 +0530
permalink: /_posts/iot/protocols/secure-mqtt
categories:
  - IoT
  - IoT Security
  - MQTT
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - IoT
  - IoT Security
  - MQTT
author: Akhilesh Moghe
#show_author_profile: true
---


In this article, we will configure the `mosquitto` MQTT broker to use Username-Password and TLS/SSL security mechanisms. We will be using `openssl` to create our own Self-Signed Certificate authority (CA), Server keys and certificates. You can refer [SSL TLS Explained](/_posts/security/tls) article to learn more about SSL, Handshake mechanism, Various certificates, etc.


## *<u>Unsecured-Unauthenticated MQTT Communication</u>*
- __*<u>Mosquitto Broker Configurations</u>*__:
    - Settings:
        - `listener 8883`
        - `listener 1883`
        - `allow_anonymous true`
    - __*<u>listener</u>*__:
        - Mosquitto Broker listens on a port/ip address combination with above keyword.
        - By using this variable multiple times, mosquitto can listen on more than one port.
        - The __*<u>port</u>*__*<u> number to listen on must be given</u>*. Optionally, an __*ip address*__ or __*host*__ name may be supplied as a second, optional argument.
            - Port number is mandatory, but not IP.
        - `listener` by default listen on all network interfaces, but can also have IP address, or Hostname, or Unix Socket path, which can be used to restrict the broker access to specific interface and network.
        - Only one listener should be sufficient.
        - But, I faced issues to operate `mosquitto_sub` client on port 8883, it resulted in `error: Client <unknown> disconnected due to malformed packet`, but it worked fine on port 1883, for some unknown reason, so added both listeners.
    - __*<u>allow_anonymous</u>*__:
        - Boolean value that determines whether clients that connect without providing a username are allowed to connect.
        - Defaults to false.
        - Setting this parameter to `true` enables the remote MQTT clients, running on other machines like CloudRail.Box to connect to Mosquitto Broker running on laptop.
        - Without this option enabled, Mosquitto Broker can only connect to local MQTT clients, i.e. clients running on same laptop.
- __Run Mosquitto MQTT Broker__:
```
$ mosquitto -c mosquitto.conf -v
```
- __Run Mosquitto Clients Receiver/Publisher__:
```
$ mosquitto_sub -t # -v
$ mosquitto_pub -m <message> -t /<topic>
```

    ---

## *<u>Username and Password Authentication for Mosquitto MQTT</u>*
- Mosquitto MQTT broker can be configured to require client authentication using a valid username and password for successful connection.
- The username and password combination is transmitted from the mosquitto clients to the broker in clear text format, and it is not secure in any form of encryption.(~SSL)
- __*<u>Password File</u>*__:
    - Create Username and Password file:
    ```
$ mosquitto_passwd -c <passwordfilename> <username>
    ```
    - This command will ask you to set the password.
    - It will not echo the characters, so be careful and remember the password.
    - This creates a `passwordfile` which has username and encrypted password in `<username>:<encrypted-password>`.
    - This file needs to be provided to Mosquitto Broker in its config file with parameter `password_file`.
- __*<u>Mosquitto Broker Configurations</u>*__:
    - Parameter Settings:
        - `allow_anonymous false`
            - Set this to false, as we are implementing the basic security, access control mechanism.
        - `password_file <password_file_path>`
            - Provide the `passwordfile` created with `mosquitto_passwd` utility with this parameter, along with its absolute/relative path.
- __Run Mosquitto MQTT Broker__:
```
$ mosquitto -c mosquitto.conf -v
```
- __Run Mosquitto Clients Receiver/Publisher__:
```
$ mosquitto_sub -t # -u <username> -P <password>
$ mosquitto_pub -m <message> -t /<topic>
```

    ---

## *<u>Secure MQTT Communication with TLS/SSL Certificates</u>*
- Authentication with TLS/SSL certificates is another way of authenticating & securely connecting the clients to the broker.
- A client certificate identifies the client just like the server certificate identifies the server.
- Normally certificates are created and distributed to each client that connects to the server/broker that requires them.
- We can use certificates in combination with username and password authentication.
- __*<u>Mosquitto Broker Configurations</u>*__:
    - Parameter Settings:
        - `listener 8883`
            - Recommended port for MQTT over TLS is 8883, but this must be set manually.
        - `allow_anonymous false`
        - `cafile <ca-certificate-file-path>`
            - Certificate Authority certificates that will be considered trusted when checking incoming client certificates.
        - `certfile <broker-certificate-file-path>`
            - Path to the PEM encoded server/broker certificate.
        - `keyfile <broker-private-key-file-path>`
            - Path to the PEM encoded keyfile.
        - `require_certificate true`
        - `use_identity_as_username true`\
&nbsp;
- __*<u>OpenSSL Configuration</u>*__
  - Required to create for __*<u>SAN</u>*__ Certificates required to use __*<u>Private/Public IP Addresses</u>*__ instead of a Domain name/URLs.
  - Backup `openssl` configs:
```
$ cp /etc/openssl<version>/openssl.cnf /tmp/openssl.cnf
```
  - Add Private/Public IP Addresses entries to SAN extensions:
```
$ SAN='\n[SAN]\nsubjectAltName=IP:<IP-Address>,IP:<IP-Address>'
$ 
$ echo -e "$SAN" >> /tmp/openssl.cnf
```

### *<u>Self-Signed CA Certificates</u>*:
- Create __*<u>Certificate Authority</u>*__:
  ```
$ openssl req -new -x509 -days 365 -extensions v3_ca -keyout ca.key -out ca.crt
  ```

  - <u>Sample output</u> of above command, Fill all details:
    ```
---
Enter PEM pass phrase: <password-for-ca-key>
Verifying - Enter PEM pass phrase: <password-for-ca-key>
---
Country Name (2 letter code) [AU]:IN
State or Province Name (full name) [Some-State]:<State>
Locality Name (eg, city) []:<City>
Organization Name (eg, company) [Internet Widgits Pty Ltd]:<Company-Name>
Organizational Unit Name (eg, section) []:<Org-Unit>
Common Name (e.g. server FQDN or YOUR name) []:<Website-URL>
Email Address []:<support-email-address>
---
    ```
  - :red_circle: __Important Note__: *The* __*Common Name*__ *for the CA cannot be the same as the Common Name for the client and server certificates*.
    - *Probable Error*:
      `OpenSSL Error[0]: error:14094418:SSL routines:ssl3_read_bytes:tlsv1 alert unknown ca`
  - :pencil: Note down the __PEM pass phrase__, you will need this at a next stage while creating the broker & client certificates.

- Check generated CA Certificate, specifically __Subject__ field, which reflect all information provided in above step:
  ```
$ openssl x509 -in ca.crt -text -noout
  ```

### *<u>MQTT Broker/Server Certificate</u>*:
- Create __*Private Key*__ for Broker:
  ```
$ openssl genrsa -out broker.key 2048
  ```

- Create a __*<u>Certificate Signing Request(CSR)</u>*__ file from Broker key:
  ```
$ openssl req -out broker.csr -key broker.key -new -config /tmp/openssl.cnf
  ```

- <u>Sample output</u> of above command, Fill all details:

```
---
You are about to be asked to enter information that will be incorporated into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
---
Country Name (2 letter code) [AU]:IN
State or Province Name (full name) [Some-State]:<State>
Locality Name (eg, city) []:<City>
Organization Name (eg, company) [Internet Widgits Pty Ltd]:<Company-Name>
Organizational Unit Name (eg, section) []:<Org-Unit>
Common Name (e.g. server FQDN or YOUR name) []:<IP Address of the MQTT Broker> or localhost
Email Address []:<support-email-address>

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:              
An optional company name []:
---
```
  - :red_circle: __Important Note__: *The* __*Common Name*__ *for the CA cannot be the same as the Common Name for the client and server certificates*.
  - :o: Optional fields: A challenge password []: & An optional company name []:\
&nbsp;

- Check generated MQTT Broker Certificate Signing Request:
  ```
$ openssl req -in broker.csr -text -noout
  ```

- Create a MQTT Broker __*<u>Certificate(.crt)</u>*__ from above CSR Request:
  ```
$ openssl x509 -req -in broker.csr -CA ../ca/ca.crt -CAkey ../ca/ca.key -CAcreateserial -out broker.crt -days 365 -extensions SAN -extfile ../openssl.cnf
  ```
    - :red_circle: __Important Note__: You need to provide __*PEM pass phrase*__ used in CA certificate creation hereby.\
&nbsp;

- <u>Sample output</u>:
```
Certificate request self-signature ok
subject=C = IN, ST = Maharashtra, L = Pune, O = CloudRail, OU = IoT, CN = 10.149.0.30, emailAddress = contact@cloudrail.com
Enter pass phrase for ../ca/ca.key:
```

- Check generated MQTT Broker Certificate, specifically __Subject__ & __SAN__ fields, which reflect all information provided in above step:
```
$ openssl x509 -in broker.crt -text -noout
```

### *<u>MQTT Client Certificate</u>*:
- Create __*Private Key*__ for Client:
  ```
$ openssl genrsa -out client.key 2048
  ```

- Create a __*<u>Certificate Signing Request(CSR)</u>*__ file from Client key:
  ```
$ openssl req -out client.csr -key client.key -new -config /tmp/openssl.cnf
  ```

- <u>Sample output</u> of above command, Fill all details:

```
---
You are about to be asked to enter information that will be incorporated into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
---
Country Name (2 letter code) [AU]:IN
State or Province Name (full name) [Some-State]:<State>
Locality Name (eg, city) []:<City>
Organization Name (eg, company) [Internet Widgits Pty Ltd]:<Company-Name>
Organizational Unit Name (eg, section) []:<Org-Unit>
Common Name (e.g. server FQDN or YOUR name) []:<IP Address of the MQTT Broker> or localhost
Email Address []:<support-email-address>

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:              
An optional company name []:
---
```
  - :red_circle: __Important Note__: *The* __*Common Name*__ *for the CA cannot be the same as the Common Name for the client and server certificates*.
  - :o: Optional fields: A challenge password []: & An optional company name []:\
&nbsp;

- Check generated Client Certificate Signing Request:
  ```
$ openssl req -in client.csr -text -noout
  ```

- Create a Client __*<u>Certificate(.crt)</u>*__ from above CSR Request:
  ```
$ openssl x509 -req -in broker.csr -CA ../ca/ca.crt -CAkey ../ca/ca.key -CAcreateserial -out client.crt -days 365 -extensions SAN -extfile ../openssl.cnf
  ```
    - :red_circle: __Important Note__: You need to provide __*PEM pass phrase*__ used in CA certificate creation hereby.\
&nbsp;

- <u>Sample output</u>:
```
Certificate request self-signature ok
subject=C = IN, ST = Maharashtra, L = Pune, O = CloudRail, OU = IoT, CN = 10.149.0.30, emailAddress = contact@cloudrail.com
Enter pass phrase for ../ca/ca.key:
```

- Check generated Client Certificate, specifically __Subject__ & __SAN__ fields, which reflect all information provided in above step:
```
$ openssl x509 -in client.crt -text -noout
```

### *<u>MQTTS Communication Testing</u>*:
- __Run Mosquitto MQTT Broker__:
```
$ mosquitto -c mosquitto.conf -v
```

- __Run Mosquitto Client Receiver__:
```
$ mosquitto_sub -p <1883/8883> --cafile /<path-to-ca-certificate>/ca.crt --cert /<path-to-client-certificate>/client.crt --key /<path-to-client-key>/client.key -h <IPAddress-or-localhost-or-BrokerDomainURL> -t <topic>
```

- __Run Mosquitto Client Publisher__:
```
$ mosquitto_pub -p <1883/8883> --cafile /<path-to-ca-certificate>/ca.crt --cert /<path-to-client-certificate>/client.crt --key /<path-to-client-key>/client.key -h <IPAddress-or-localhost-or-BrokerDomainURL> -m <message> -t <topic>
```

  ---

## *<u>What is SAN Certificate?</u>*
- A __*Subject Alternate Name*__ (or SAN) certificate is a digital security certificate which allows multiple hostnames to be protected by a single certificate.
- A SAN certificate may also be called a __*<u>Unified Communication Certificate</u>*__ (or UCC), a __*<u>multi-domain certificate</u>*__, or an __*<u>Exchange certificate</u>*__.
- SAN certificates can be used on unlimited multiple servers concurrently and can be used on unlimited IP addresses with multiple, concurrent private keys (which is great for hosting and virtual hosts).
- The entries in any SSL.com SAN certificate: can be a __*<u>Fully Qualified Domain Name (FQDN)</u>*__ and can be a __*<u>wildcard domain name</u>*__ (i.e. *.domain.com or *.store.domain.com) but NOT a multiple-level wildcard (like *.*.domain.com).
- :x: *<u>CA Certificate can not be a “Subject Alternative Name (SAN)” certificate</u>*, it results in an Error:
  - `OpenSSL Error[0]: error:14094418:SSL routines:ssl3_read_bytes:tlsv1 alert unknown ca`.\
&nbsp;

- `Error: {"message":"Device muruMsT0IhwpFZeNNPlaYxFjx11Uxikv failed to connect with err Error: Hostname/IP doesn't match certificate's altnames: \"IP: 10.149.0.30 is not in the cert's list: \"\n","timestamp":"2022-06-29T09:45:18.080Z","type":"out","process_id":9,"app_name":"main"}`
  - <u>Reason</u>:
    - This Error appeared for me while using Self-Signed CA, Server, Client certificates with NodeJS Application, but Mosquitto MQTT clients/broker had no issues using the same self-signed certificates. Also these certificates were created for a Private IP address, and not for any domain URL, as is the usual case. I was testing the local MQTTS communication over local LAN so private IP addresses makes sense.
    - The problem is as indicated that, a Node ‘mqtt’ module seems to need a SAN certificate for Private IP Addresses.
        - According to the CA Browser forum, there may be compatibility issues with certificates for IP addresses unless the IP address is in both the commonName and subjectAltName fields. This is due to legacy SSL implementations which are not aligned with RFC 5280, notably, Windows OS prior to Windows 10.

  ---

## *Encountered Errors & Reasons for them*:
- `Error: mosquitto mqtt tls certificate error Client auto-5F29B532-D3F5-7C93-EF6A-0289F5C3DAEA disconnected, not authorised.`
  - <u>Reason</u>: Client certificates were not created and broker certificates were tried to use, so this error.\
&nbsp;

- `Error: OpenSSL Error: error:14094438:SSL routines:ssl3_read_bytes:tlsv1 alert internal error`
  - <u>Reason</u>:
    - TLS expects the connection to be directed to the same CN (Common Name) as the one used in the generation step of the client.csr request file.
    - The CN (Common Name) should be the same as the server/broker hostname if no SAN extension is used (See What is SAN Certificate).
  - References:
    - [Client using libmosquitto gets "tlsv1 alert internal error" using TLS, works fine without TLS](https://stackoverflow.com/a/65957636/2003556)
    - [MQTTS : How to use MQTT with TLS?](https://openest.io/en/services/mqtts-how-to-use-mqtt-with-tls/)\
&nbsp;

- `Error: {“message":"Device XdtNrVk5zzZ5Fvmf8adL9R0dJeG6inkG failed to connect with err Error: write EPROTO 1996375408:error:1409441B:SSL routines:ssl3_read_bytes:tlsv1 alert decrypt error:../deps/openssl/openssl/ssl/s3_pkt.c:1498:SSL alert number 51\n1996375408:error:1409E0E5:SSL routines:ssl3_write_bytes:ssl handshake failure:../deps/openssl/openssl/ssl/s3_pkt.c:659:\n\n","timestamp":"2022-06-29T09:34:12.858Z","type":"out","process_id":9,"app_name":"main"}`
  - <u>Reason</u>: Certificate mismatch, older certificate used.\
&nbsp;

- `New connection from 10.149.0.1:59884 on port 8883.`\
	`OpenSSL Error[0]: error:0407008A:rsa routines:RSA_padding_check_PKCS1_type_1:invalid padding`\
	`OpenSSL Error[1]: error:04067072:rsa routines:rsa_ossl_public_decrypt:padding check failed`\
	`OpenSSL Error[2]: error:0D0C5006:asn1 encoding routines:ASN1_item_verify:EVP lib`\
	`OpenSSL Error[3]: error:1417C086:SSL routines:tls_process_client_certificate:certificate verify failed Client <unknown> disconnected: Protocol error.`
  - <u>Reason</u>: Certificate mismatch, older certificate used.

  ---

###### References:
- <u>SAN References</u>:
  - [Is it possible to have SSL certificate for IP address, not domain name? [closed]](https://stackoverflow.com/a/64186819/2003556)
  - [Is it possible to have SSL certificate for IP address, not domain name? [closed]](https://stackoverflow.com/a/37063612/2003556)
  - [Self-Signed Certificate for Public and Private IP (Tomcat 7)](https://stackoverflow.com/questions/33494750/self-signed-certificate-for-public-and-private-ip-tomcat-7)
  - [Invalid self signed SSL cert - "Subject Alternative Name Missing"](https://stackoverflow.com/a/58210221/2003556)
  - [Unable to connet a mosquitto using a private IP address and WSS #663](https://github.com/mqttjs/MQTT.js/issues/663)
- [Creating and Using Client Certificates with MQTT and Mosquitto](http://www.steves-internet-guide.com/creating-and-using-client-certificates-with-mqtt-and-mosquitto/)
- [Mosquitto SSL Configuration -MQTT TLS Security](http://www.steves-internet-guide.com/mosquitto-tls/)
- [Quick Guide to The Mosquitto.conf File With Examples](http://www.steves-internet-guide.com/mossquitto-conf-file/)
- [Using The Mosquitto_pub and Mosquitto_sub MQTT Client Tools- Examples](http://www.steves-internet-guide.com/mosquitto_pub-sub-clients/)
- [MQTTS : How to use MQTT with TLS?](https://openest.io/en/services/mqtts-how-to-use-mqtt-with-tls/)

- <u>Mosquitto Username and Password Authentication References</u>:
  - [Mosquitto Username and Password Authentication -Configuration and Testing](http://www.steves-internet-guide.com/mqtt-username-password-example/)
  - [mosquitto_passwd man page](https://mosquitto.org/man/mosquitto_passwd-1.html)
  - [Mosquitto MQTT Authentication methods](https://mosquitto.org/documentation/authentication-methods/)

