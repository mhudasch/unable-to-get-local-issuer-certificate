---
layout: tool
name: Git
title: Git
permalink: /tools/git
parent: Troubleshooting by Tool

tool_description: >
  Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.
tool_docs: https://git-scm.com/docs

date: 2023-03-10
published: true
---

{% capture sample %}

```text
...
SSL Certificate problem: unable to get local issuer certificate.
...
```

{% endcapture %}

{% include tool_head.md issue_sample=sample -%}

## Windows

The Git for Windows installation brings its own certificate store with it. Trusted certificates are located in the directory `C:\Program Files\Git\mingw64\ssl\certs` and are saved in file called `ca-bundle.crt` or `ca-bundle.trust.crt`. The files contain certificates in the [pem - Base64 format](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail).

### Solution 1

You can insert your missing certificate at the end of those files.
The drawback of that solution is that the file(s) get updated every time Git for Windows gets updated. You have to automate the solution above to update the certificate files on every update.

### Solution 2

Another solution is to place a certificate store (crt) file somewhere in your user directory and insert your missing certificate in that file.
Now you can use the following command to let Git use that file instead of its own:

```shell
git config --global http.sslCAInfo "%USERPROFILE%\<filepath to ca-trusted.crt>"
```

This can also be achieved by using the `GIT_SSL_CAINFO` environment variable to a filepath of a crt file.

This solution has a similar drawback. When certificates in the store  file, which are not your own expire you have to update them in [pem - Base64 format](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail). But since certificate expiration is more rare than a Git update, this solution is little bit better.

### Solution 3

All Git network traffic is performed by cURL. The cURL executable used by Git for Windows is can be found at the location `C:\Program Files\Git\mingw64\bin\curl.exe`. Git uses this cURL executable unless the `PATH` environment variable contains another path to a cURL executable with higher precedence. Since cURL version `7.60.0`, the Windows native Secure Channel (schannel) is supported which can utilize the certificates located in the Windows Certificate Store. To check if your Git for Windows installation uses a compatible version run the following command:

```shell
"C:\Program Files\Git\mingw64\bin\curl.exe" --version
```

When the version is higher than `7.60.0` you can configure Git to use the *schannel* as the SSL-Backend by executing the following command:

```shell
git config --global http.sslBackend schannel
```

This solution has the benefit that no certificate files have to be managed which leaves the Windows Certificate Store as the single point of truth when it comes to certificate trust.

<h3 id="windows-links">Links</h3>

[Documentation of `git config`](https://www.git-scm.com/docs/git-config#Documentation/git-config.txt-httpsslCAInfo)<br>
[Documentation of Native Windows Certificate Verification (Schannel)](https://learn.microsoft.com/en-us/windows/win32/secauthn/secure-channel)

<!-- TODO: Write article about that -->
<!-- [Convert Windows Certificate Store certificate into pem format](#)-->

## Linux

Depending on your Linux distribution, the trusted certificates can be located in different directories. So convert your certificate into [pem - Base64 format](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail) and save it somewhere in your user directory. After that follow the guide [here]({{ '/common/#linux' | relative_url }}) to put the certificate in right location for your Linux distribution.

If your Linux distribution bundles the trusted certificates in one file you can use the following git command to share this info with git:

```bash
git config --global http.sslCAInfo <path to pem/crt file>
```

Or if your Linux distribution stores the trusted certificates in separate file in a directory you can use the following git command:

```bash
git config --global http.sslCAPath <path to cert directory>
```

Alternatively you can set the `GIT_SSL_CAINFO` or `GIT_SSL_CAPATH` environment variable respectively.
The documentation for the git configuration regarding certificate locations can be found [here](https://www.git-scm.com/docs/git-config#Documentation/git-config.txt-httpsslCAInfo).

## Fix it in macOS

Coming Soon...

<!--

Install Git with cURL using Homebrew with the following command:

```sh
brew install curl
brew install --with-curl git
```

If you have already installed Git without the cURL support use the following command to replace that installation:

```sh
brew reinstall --with-curl git
```

Now that you have the Homebrew version of cURL installed you 

-->