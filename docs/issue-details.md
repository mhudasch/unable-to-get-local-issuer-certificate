---
layout: default
title: Further issue details
permalink: /further-issue-details/
nav_order: 1
---

# {{ page.title }}
{: .no_toc }

Do you want to know more about what caused the issue?
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Issue Description

When developing software or just when using some software you might come across a cryptic error message that contains the error string this web page is dedicated to:

```text
... unable to get local issuer certificate ...
```

The error message typically occurs when a program or application tries to establish a SSL (HTTPS) connection to a server and is unable to verify the authenticity of an SSL certificate presented by the server, because it cannot find the root CA certificate that issued the server's certificate.

Some possible reasons for this error message include:

1. Missing or outdated root CA certificate: The program or application may not have the root CA certificate needed to verify the server's SSL certificate. This can happen if the root CA certificate is missing or outdated, or if the program is looking for the certificate in the wrong location.

2. SSL certificate chain issues: The server's SSL certificate may be missing one or more intermediate certificates in its chain, which are needed to establish trust between the root CA and the server's certificate.

3. Certificate revocation: The SSL certificate presented by the server may have been revoked by the issuing CA, or may have expired, leading to trust issues.

4. Self-Signed certificate: When a self-signed SSL certificate gets used, the program or application will not be able to verify the authenticity of the server's SSL certificate because there is no trusted root CA involved in the certificate chain.

5. Firewall or network issues: The error message may be caused by a firewall or network issue that is preventing the program or application from accessing the root CA certificate or the server's SSL certificate.

Many answers on pages like [stackoverflow](https://stackoverflow.com) or other user forums suggest to disable the security option in an application by setting some option like `strict-ssl=false` or `validate-server-certificate=false` etc. or to use an `http` url instead in order to *fix* the issue and go on quickly.

Please **DO NOT** do this because you are disabling security entirely and not fixing the root cause of the issue. Please **DO** follow the steps provided here to actually fix the issue for your particular case and tool.

## Going a bit deeper

### The SSL trust hierarchy

SSL certificates create a hierarchical trust model, where trust is established by a chain of certificates, starting with a root certificate authority (CA) that is trusted by web browsers and operating systems.

Intermediate CAs are then used to issue SSL certificates for individual websites, and these certificates are trusted because they are signed by a CA higher up in the chain, ultimately leading back to the trusted root CA.

This hierarchical trust model ensures that the identity of the website owner and the integrity of the website's communications are verified by a trusted third party, providing users with a secure and reliable online experience.

When a certificate on each level of the hierarchy are missing in the certificate store or directory of a the browser or operating system the error above will occur.

- See [Chain of trust](https://en.wikipedia.org/wiki/Chain_of_trust)

### Why even have SSL (TLS) certificates

SSL certificate trust is crucial for secure web traffic because it establishes a secure and encrypted connection between a user's web browser (or other apps) and a server, protecting sensitive information such as passwords, credit card numbers, api-keys, and other personal data from interception by attackers.

Trust in SSL certificates is established through the use of digital signatures and encryption algorithms that verify the authenticity of the server's identity and the integrity of the data being exchanged. Without this trust, users would be at risk of having their information stolen or compromised, leading to financial loss and other security risks.

When you need a better understanding using a visual presentation you can go [here](https://howhttps.works/).

- See [HTTPS](https://en.wikipedia.org/wiki/HTTPS)
- See [SSL Certificates](https://en.wikipedia.org/wiki/Public_key_certificate)

### SSL interception

SSL interception, also known as SSL inspection, is a technique used by web proxies to intercept and inspect encrypted SSL traffic between a user's app and a server.

To perform SSL interception, the web proxy generates its own SSL certificate that mimics the target sever's SSL certificate. When a user attempts to connect to the server, their app is redirected to the proxy's SSL certificate, which is then used to establish an encrypted connection with the server.

The web proxy can then intercept and inspect the traffic flowing between the user's app and the server, allowing it to filter or modify the content as needed. Once the inspection is complete, the proxy forwards the traffic back to the user's app, which decrypts it using the proxy's SSL certificate.

### But it is only an 'internal' tool

Even when developing software that is only used internally a so-called **zero-trust mindset** should be preferred because it recognizes that threats can come from both outside and inside an organization, and that all users, devices, and applications should be treated as potentially untrusted.

Developing software with a zero-trust mindset involves implementing security measures such as identity verification, access control, and encryption at every stage of the development process, to prevent unauthorized access and data breaches.

By adopting a zero-trust approach, organizations can ensure that their internal software is protected against both external and internal threats, reducing the risk of data loss and maintaining the confidentiality, integrity, and availability of their information assets.

- See [Zero-Trust](https://en.wikipedia.org/wiki/Zero_trust_security_model)
