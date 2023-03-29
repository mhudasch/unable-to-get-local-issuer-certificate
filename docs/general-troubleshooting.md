---
css: toc
layout: page
title: General Troubleshooting
permalink: /common/
nav_order: 2
---

# {{ page.title }}
{: .no_toc }

Depending on the used tool or operating system the fix might differ but here are some general causes for the issues an their fixes.
{: .fs-4 .fw-100 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Missing or outdated root CA certificates

A program or application may not have the root CA certificate needed to verify a server's SSL certificate. This can happen if the root CA certificate is missing or outdated, or if the program is looking for the certificate in the wrong location.

The root CA certificates of the biggest root CA authorities like Microsoft, Google, DigiCert, GlobalSign, VeriSign etc. are typically updated automatically by operating system updates. However in case you need add the certificate manually - maybe it is the one missing by your own company - follow these steps:

### Windows

To update root CA certificates using an UI, follow these steps:

- Open the Microsoft Management Console (MMC) by pressing Windows key + R, typing "mmc" and hitting Enter.
- From the File menu, select Add/Remove Snap-in, and then select Certificates from the Available snap-ins list and click Add.
- Choose the "Computer Account" option and click Next.
- Select "Local Computer" and click Finish.
- Expand the "Trusted Root Certification Authorities" folder and right-click on "Certificates" to select "All Tasks" and then "Import".
- Follow the prompts to import the new root CA certificate file.

To update root CA certificates using the command-line, follow these steps:

- Open the Command Prompt (cmd) or PowerShell (powershell/pwsh) as an administrator.
- Run the following command to import the new root CA certificate file:

  ```shell
  certutil -addstore -f "Root" <path to certificate file>
  ```

  Replace `<path to certificate file>` with the path to the new root CA certificate file.

To update intermediate certificates using an UI, follow these steps:

- Open the MMC and select Certificates from the Available snap-ins list.
- Choose the "Computer Account" option and select "Local Computer".
- Expand the "Intermediate Certification Authorities" folder and right-click on "Certificates" to select "All Tasks" and - then "Import".
- Follow the prompts to import the new intermediate certificate file.

To update intermediate certificates using the command-line, follow these steps:

- Open the Command Prompt (cmd) or PowerShell (powershell/pwsh) as an administrator.
- Run the following command to import the new intermediate certificate file:

  ```shell
  certutil -addstore -f "CA" <path to certificate file>
  ```

  Replace `<path to certificate file>` with the path to the new intermediate certificate file.

### macOS

To update root CA certificates in macOS using an UI, follow these steps:

- Open the "Keychain Access" app from the Applications > Utilities folder.
- Select "System" from the Keychains list.
- Select "Certificates" from the Category list.
- Drag and drop the new root CA certificate file into the Keychain Access window.
- Double-click on the imported certificate, expand the "Trust" section, and ensure that the "Always Trust" option is - selected.

To update root CA certificates in macOS using the command-line, follow these steps:

- Open the Terminal app.
- Run the following command to import the new root CA certificate file:

  ```shell
  sudo security add-trusted-cert -d -r trustRoot -k "/Library/Keychains/System.keychain" <path to certificate file>
  ```

  Replace `<path to certificate file>` with the path to the new root CA certificate file.

The `sudo` command is used to run the security command with administrator privileges.
{: .note}

To update intermediate certificates, follow these steps:

- Open the Keychain Access app and select "System" from the Keychains list.
- Select "Certificates" from the Category list.
- Locate the expired or missing intermediate certificate and delete it.
- Drag and drop the new intermediate certificate file into the Keychain Access window.
- Double-click on the imported certificate, expand the "Trust" section, and ensure that the "Always Trust" option is - selected.

### Linux

Depending on your Linux distribution, the root CA certificates can be located in different directories. Some might store the certificate files each as separate [pem - Base64 format](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail) files in a directory. Others have one so-called bundle file which contains all root CA certificates. Files containing one or more certificates as *x509 PEM - Base64 strings* have typically the file extension `.pem` or `.crt`.

#### Debian / Ubuntu / Gentoo etc

To update root CA certificates, follow these steps:

- Install the ca-certificates package by running the following command in the terminal:

  ```bash
  sudo apt-get install ca-certificates
  ```

- Copy the new root CA certificate file to the `/usr/local/share/ca-certificates/` directory.
- Run the following command in the terminal to update the certificate store:
  
  ```bash
  sudo update-ca-certificates
  ```

To update intermediate certificates, follow these steps:

- Copy the new intermediate certificate file to the `/usr/local/share/ca-certificates/` directory.
- Run the `sudo update-ca-certificates` command to update the certificate store.

The `sudo` command is used to run the security command with administrator privileges.
{: .note}

#### Fedora / Red Head Enterprise Linux 6

To update root CA certificates, copy the text content of your certificate at the end of the file located at `/etc/pki/tls/certs/ca-bundle.crt`.

#### OpenSUSE

To update root CA certificates, copy the text content of your certificate at the end of the file located at `/etc/ssl/ca-bundle.pem`.

#### OpenELEC

To update root CA certificates, copy the text content of your certificate at the end of the file located at `/etc/pki/tls/cacert.pem`.

#### Amazon Linux / CentOS / Red Head Enterprise Linux 7

To update root CA certificates, follow these steps:

- Copy your certificate file into `/etc/pki/ca-trust/source/anchors/` with:

  ```bash
  sudo cp <path to pem file> /etc/pki/ca-trust/source/anchors/
  ```

- Run the following command in the terminal to update the certificate store:

  ```bash
  sudo update-ca-trust extract
  ```

The `sudo` command is used to run the security command with administrator privileges.
{: .note}

#### Alpine Linux

To update root CA certificates, copy the text content of your certificate at the end of the file located at `/etc/ssl/cert.pem`.

---

Thanks to the article [here](https://serverfault.com/questions/62496/ssl-certificate-location-on-unix-linux/722646), I found some of the certificate file locations for various Linux distributions. I want to very much thank/credit all the contributers in the posts over there. Contributors are still adding more and more locations, so if you could not find the location or guide for your distribution here you could check there as well.
{: .note}

## Certificate revocation

The SSL certificate presented by the server may have been revoked by the issuing CA, or may have expired, leading to trust issues.

First check the SSL certificate trust chain like described [here](#detecting-ssl-certificate-chain-issues). When the certificate is expired, you might have to update the certificate store of the operating system (like shown [here](#missing-or-outdated-root-ca-certificates)) or the certificate store of your program like shown [here]({{ site.baseurl }}/#want-a-solution-for-the-error).

An actual SSL certificate revocation might occur for several reasons like:

1. Compromised private key: If the private key used to generate the SSL certificate is compromised or stolen, the certificate must be revoked to prevent unauthorized access to sensitive data.

2. Misissued certificate: If the SSL certificate was issued incorrectly or contains incorrect information, it may need to be revoked to prevent further security issues.

3. Certificate expiration: SSL certificates have an expiration date, and if the certificate is not renewed or replaced before the expiration date, it must be revoked.

4. Change in domain ownership: If the ownership of the domain associated with the SSL certificate changes, the certificate may need to be revoked to ensure the security of the new domain owner.

5. Breach of CA policies: If a certificate authority (CA) violates industry standards or security policies, its certificates may be revoked to protect users' security and trust.

SSL certificate revocation is an important mechanism for maintaining the integrity and security of online communication, and is used to prevent unauthorized access to sensitive data and protect users from security threats.

## Self-Signed certificates

To resolve the issue when using a self-signed SSL certificate, the certificate needs to be manually added to the trusted certificates list on the client-side. This will enable the program or application to establish a secure and encrypted connection with the server using the self-signed SSL certificate.

However, it's important to note that relying on self-signed certificates for secure communication is generally discouraged, as it can leave users vulnerable to man-in-the-middle attacks and other security risks. In production environments, it's recommended to use SSL certificates issued by trusted third-party CAs to ensure the highest level of security and trust.

## Detecting SSL certificate chain issues

A server's SSL certificate may be missing one or more intermediate certificates in its chain, which are needed to establish trust between the root CA and the server's certificate.

### Investigate with the browser

If you know the url which your framework or app requested while the error occurred, you can simply put in into your browser and see what happens. Here you might see immediately that the server certificate is not trustworthy because the address bar of the browser highlights this for you. To investigate further you might be able to click on the the padlock (or crossed-through 'https://' text) to open a small dialog showing some details. In this dialog you can always find a button or link that shows details of the server certificate. For example:

- Microsoft Edge

  ![Microsoft Edge]({{ '/assets/img/edge-cert.png' | relative_url }})

- Google Chrome

  ![Google Chrome]({{ '/assets/img/chrome-cert.png' | relative_url }})

- Mozilla Firefox

  ![Mozilla Firefox]({{ '/assets/img/firefox-cert.png' | relative_url }})

The highlighted buttons open a dialog containing details of the server's SSL certificate. The detail dialogs also contain a tree-like structure that can be opened and closed. This structure represents the so-called certificate chain. It starts with the root CA certificate, sometimes some intermediate certificates and finally the server's SSL certificate on the last level (leaf). You can open each node in the tree and see the details for the certificate on that level of the tree. Here you can investigate if and why the certificate is not trustworthy. You can for example check if it is expired or if it has been revoked. If you encounter the `unable to get local issuer certificate` issue either the whole tree or parts of the certificate tree is missing.

### Investigate with curl

When you have curl installed on your machine you can investigate certificate issues using the command-line. This is especially handy when the framework or app that thew the issue uses libcurl under the hood to perform its network traffic.

You can use the following command to show SSL/TLS connection details using curl:

```bash
curl -iLv https://github.com
```

{: .note}
`-i` to include the HTTP response headers in the output.<br>
`-L` to include HTTP location headers in the output.<br>
`-v` Outputs the previous infos in a verbose manner.<br>

An output can look like this:

```diff
> * Uses proxy env variable https_proxy == 'http://localhost:9000'
> *   Trying 127.0.0.1:9000...
> * Connected to localhost (127.0.0.1) port 9000 (#0)
 * allocate connect buffer
 * Establish HTTP proxy tunnel to github.com:443
 > CONNECT github.com:443 HTTP/1.1
 > Host: github.com:443
 > User-Agent: curl/8.0.1
 > Proxy-Connection: Keep-Alive
 >
 < HTTP/1.1 200 Connection Established
 HTTP/1.1 200 Connection Established
 < Proxy-Agent: ...
 Proxy-Agent: ...
 <

 * CONNECT phase completed
 * CONNECT tunnel established, response 200
 * ALPN: offers h2,http/1.1
 * TLSv1.3 (OUT), TLS handshake, Client hello (1):
> *  CAfile: ...
> *  CApath: ...
 * TLSv1.3 (IN), TLS handshake, Server hello (2):
 * TLSv1.3 (IN), TLS handshake, Encrypted Extensions (8):
 * TLSv1.3 (IN), TLS handshake, Certificate (11):
> * TLSv1.3 (IN), TLS handshake, CERT verify (15):
 * TLSv1.3 (IN), TLS handshake, Finished (20):
 * TLSv1.3 (OUT), TLS change cipher, Change cipher spec (1):
 * TLSv1.3 (OUT), TLS handshake, Finished (20):
 * SSL connection using TLSv1.3 / TLS_AES_128_GCM_SHA256
 * ALPN: server accepted h2
 * Server certificate:
 *  subject: C=US; ST=California; L=San Francisco; O=GitHub, Inc.; CN=github.com
 *  start date: ...
 *  expire date: ...
 *  subjectAltName: host "github.com" matched cert's "github.com"
 *  issuer: C=US; O=DigiCert Inc; CN=DigiCert TLS Hybrid ECC SHA384 2020 CA1
> *  SSL certificate verify ok.
 * using HTTP/2
 ...
 <HTTP HEADERS / HTML content>
```

I've highlighted the most important lines here. The first lines show us that the communication goes through a proxy (for test purposes here). This fact often gets overlooked when investigating certificate issues. The certificate issues might already occur in the communication with your proxy and not event the target website.

The next lines show the SSL/TLS handshake with the target website (here: github.com). The first important lines here are the used `CAfile/CApath` configurations. This is the source for trusted certificates. When we have issues as explained in the other paragraphs, then it is very likely that it has to do with the state of this configuration. After that we focus on the part that says `CERT verify`. When there are issues with the root CA certificate or the certificate chain, errors will pop up here.

An error output would look like this:

```diff
...

> *  CAfile: ...
> *  CApath: ...
 * TLSv1.3 (IN), TLS handshake, Server hello (2):
 * TLSv1.3 (OUT), TLS change cipher, Change cipher spec (1):
 * TLSv1.3 (OUT), TLS handshake, Client hello (1):
 * TLSv1.3 (IN), TLS handshake, Server hello (2):
 * TLSv1.3 (IN), TLS handshake, Encrypted Extensions (8):
< * TLSv1.3 (IN), TLS handshake, Certificate (11):
< * TLSv1.3 (OUT), TLS alert, unknown CA (560):
< * SSL certificate problem: unable to get local issuer certificate
 * Closing connection 0
 curl: (60) SSL certificate problem: unable to get local issuer certificate

...
```

And here is the error this page is dedicated to 😄 (highlighted in red).<br>
Since the error/alert says `unknown CA`, we can conclude that the provided `CAfile/CApath` configuration does not contain information about **C**ertificate **A**uthority of the given server certificate. Usually this means the root CA certificate is not part of our certificate store.
To investigate further we might use openSSL.

### Investigate with openSSL

If you have [openSSL](https://www.openssl.org/) installed, you can investigate problems of server certificates using the following commands:

```bash
openssl s_client -connect < url without https:// >:443 -showcerts
```

When you are behind a proxy you have to add it to the commnd:

```bash
openssl s_client -connect < url without https:// >:443 -showcerts -proxy < host:port of proxy without http(s):// >
```

An output for `https://github.com` would look like this:

```diff
openssl s_client -connect github.com:443 -showcerts

 CONNECTED(000001B8)
> depth=1 C = US, O = DigiCert Inc, CN = DigiCert TLS Hybrid ECC SHA384 2020 CA1
< verify error:num=20:unable to get local issuer certificate
 verify return:1
> depth=0 C = US, ST = California, L = San Francisco, O = "GitHub, Inc.", CN = github.com
 verify return:1
 ---
> Certificate chain
>  0 s:C = US, ST = California, L = San Francisco, O = "GitHub, Inc.", CN = github.com
>    i:C = US, O = DigiCert Inc, CN = DigiCert TLS Hybrid ECC SHA384 2020 CA1
 -----BEGIN CERTIFICATE-----
 ...<x509 base64 certificate>...
 -----END CERTIFICATE-----
>  1 s:C = US, O = DigiCert Inc, CN = DigiCert TLS Hybrid ECC SHA384 2020 CA1
>    i:C = US, O = DigiCert Inc, OU = www.digicert.com, CN = DigiCert Global Root CA
 -----BEGIN CERTIFICATE-----
 ...<x509 base64 certificate>...
 -----END CERTIFICATE-----
 ---
 Server certificate
 subject=C = US, ST = California, L = San Francisco, O = "GitHub, Inc.", CN = github.com
 
 issuer=C = US, O = DigiCert Inc, CN = DigiCert TLS Hybrid ECC SHA384 2020 CA1
 
 ---
 No client certificate CA names sent
 Peer signing digest: SHA256
 Peer signature type: ECDSA
 Server Temp Key: X25519, 253 bits
 ---
 SSL handshake has read 2870 bytes and written 411 bytes
< Verification error: unable to get local issuer certificate
 ---
 New, TLSv1.3, Cipher is TLS_AES_128_GCM_SHA256
 Server public key is 256 bit
 Secure Renegotiation IS NOT supported
 Compression: NONE
 Expansion: NONE
 No ALPN negotiated
 Early data was not sent
< Verify return code: 20 (unable to get local issuer certificate)
 ---

...<rest>...
```

As you can see from the output, the certificate chain is as follows:

- Level 0 (leaf) - The website server certificate for github.com
- Level 1 (intermediate) - The certificate (given by DigiCert) that issued the certificate for github.com
- Level 2 (root) - The root CA certificate (given by DigiCert) that issued the intermediate certificate

You can see that the root CA does not get its own index because the verification of the root CA failed with error highlighted in red (num:20). As a consequence the whole certificate chain can not be trusted (the further down errors in red).
In the sample the certificate store that openSSL used didn't contain information about the `DigiCert Global Root CA` certificate and the familiar `unable to get local issuer certificate` error shows up.

When we include a certificate store (here a single crt file containing all root CA certificates) the error disappears:

```diff
openssl s_client -connect github.com:443 -showcerts -CAfile ~/ssl/ca-trusted.crt

 CONNECTED(000001B4)
> depth=2 C = US, O = DigiCert Inc, OU = www.digicert.com, CN = DigiCert Global Root CA
> verify return:1
 depth=1 C = US, O = DigiCert Inc, CN = DigiCert TLS Hybrid ECC SHA384 2020 CA1
 verify return:1
 depth=0 C = US, ST = California, L = San Francisco, O = "GitHub, Inc.", CN = github.com
 verify return:1
 ---
 Certificate chain
  0 s:C = US, ST = California, L = San Francisco, O = "GitHub, Inc.", CN = github.com
    i:C = US, O = DigiCert Inc, CN = DigiCert TLS Hybrid ECC SHA384 2020 CA1
 -----BEGIN CERTIFICATE-----
 ...<x509 base64 certificate>...
 -----END CERTIFICATE-----
  1 s:C = US, O = DigiCert Inc, CN = DigiCert TLS Hybrid ECC SHA384 2020 CA1
    i:C = US, O = DigiCert Inc, OU = www.digicert.com, CN = DigiCert Global Root CA
 -----BEGIN CERTIFICATE-----
 ...<x509 base64 certificate>...
 -----END CERTIFICATE-----
 ---
 Server certificate
 subject=C = US, ST = California, L = San Francisco, O = "GitHub, Inc.", CN = github.com
 
 issuer=C = US, O = DigiCert Inc, CN = DigiCert TLS Hybrid ECC SHA384 2020 CA1
 
 ---
 No client certificate CA names sent
 Peer signing digest: SHA256
 Peer signature type: ECDSA
 Server Temp Key: X25519, 253 bits
 ---
 SSL handshake has read 2869 bytes and written 411 bytes
> Verification: OK
 ---
 New, TLSv1.3, Cipher is TLS_AES_128_GCM_SHA256
 Server public key is 256 bit
 Secure Renegotiation IS NOT supported
 Compression: NONE
 Expansion: NONE
 No ALPN negotiated
 Early data was not sent
> Verify return code: 0 (ok)
 ---

...<rest>...
```

Now we can see the root CA certificate at the level 2 as is has been verified successfully (return:1). Also the whole chain has now been verified as well (OK).

As the used certificate store could verify all certificates in the chain, this is a good certificate store file to use with other tools as described in the other guides [here]({{ '/tools' | relative_url }}).
