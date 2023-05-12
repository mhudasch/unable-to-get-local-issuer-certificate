---
layout: tool
name: curl
title: curl (libcurl)
permalink: /tools/curl
parent: Troubleshooting by Tool

tool_description: >
  curl is used in command lines or scripts to transfer data.
tool_docs: https://curl.se/docs/manpage.html

date: 2023-03-28
published: true
---

{% capture sample %}
When used as command-line tool:

```text
...
curl: (60) SSL certificate problem: unable to get local issuer certificate
...
```

Depending on the use of *libcurl* in other frameworks and apps the error message might be formatted differently.

{% endcapture %}

{% include tool_head.md issue_sample=sample -%}

## All Platforms

Curl and especially libcurl are the basis for network traffic in many other frameworks and applications like php, python, git, and many many more.
This is the reason why many guides on this site point to this place to fix the certificate issue.

The curl command-line can use the argument `--cacert` to point to a trusted CA certificate file for a single call.
Additionally curl and libcurl support the environment variables `CURL_CA_BUNDLE` which can be pointed to a certificate bundle file containing trusted CA certificates in [pem - Base64 format](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail).

Since libcurl is the base for other frameworks and applications, you can use the curl command-line to investigate certificate issues on your machine as shown [here]({{ '/common#detecting-ssl-certificate-chain-issues' }}).

## Certificate Verification in Windows/macOS

> If libcurl was built with *Schannel* (Microsoft's native TLS engine) or *Secure Transport* (Apple's native TLS engine) support, then libcurl will still perform peer certificate verification, but instead of using a CA cert bundle, it will use the certificates that are built into the OS. These are the same certificates that appear in the Internet Options control panel (under Windows) or Keychain Access application (under OS X). Any custom security rules for certificates will be honored.

{: .fs-1}
Source: [curl documentation](https://curl.se/docs/sslcerts.html)

An example for libcurl with built-in Schannel support is the *Git for Windows Installer*. Here you can use a configuration like the following to enable the use use of the Native Windows Certificate Store instead of a certificate bundle file:

```shell
git config --global http.sslBackend schannel
```

More information can be found [here](https://curl.se/docs/sslcerts.html).

## Certificate Verification in Linux

> If libcurl was built with *NSS* support, then depending on the OS distribution, it is probably required to take some additional steps to use the system-wide CA cert db. Red Hat ships with an additional module, libnsspem.so, which enables NSS to read the OpenSSL PEM CA bundle. On openSUSE you can install p11-kit-nss-trust which makes NSS use the system wide CA certificate store.

{: .fs-1}
Source: [curl documentation](https://curl.se/docs/sslcerts.html)
