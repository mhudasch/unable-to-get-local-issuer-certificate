---
layout: page
name: git
title: Git
category: tool_article
date: 2023-03-10
published: true
---

- Do not remove this line (it will not be displayed)
{:toc}

## What is it?

Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.

[Official Documentation](https://git-scm.com/docs)

You can always check the [general troubleshooting guide]({{ site.baseurl }}/common/) before continuing.

## Issue sample

The error might look like this when using Git:

```text
...
SSL Certificate problem: unable to get local issuer certificate.
...
```

## Fix it in Windows

The Git for Windows installation brings its own certificate store with it. Trusted certificates are located in the directory `C:\Program Files\Git\mingw64\ssl\certs` and are saved in file called `ca-bundle.crt` or `ca-bundle.trust.crt`. The files contain certificates in the  [pem - Base64 format](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail).

### Solution 1

You can insert your missing certificate at the end of those files.
The drawback of that solution is that the file(s) get updated every time Git for Windows gets updated. You have to automate the solution above to update the certificate files on every update.

### Solution 2

Another solution is to place a certificate store (crt) file somewhere in your user directory and insert your missing certificate in that file.
Now you can use the following command to let Git use that file instead of its own:

```cmd
git config --global http.sslCAInfo "%USERPROFILE%\<filepath to ca-trusted.crt>"
```

This can also be achieved by using the `GIT_SSL_CAINFO` environment variable to a filepath of a crt file.

This solution has a similar drawback. When certificates in the store  file, which are not your own expire you have to update them in [pem - Base64 format](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail). But since certificate expiration is more rare than a Git update, this solution is little bit better.

### Solution 3

All Git network traffic is performed by cURL. The cURL executable used by Git for Windows is can be found at the location `C:\Program Files\Git\mingw64\bin\curl.exe`. Git uses this cURL executable unless the `PATH` environment variable contains another path to a cURL executable with higher precedence. Since cURL version `7.60.0`, the Windows native Secure Channel (schannel) is supported which can utilize the certificates located in the Windows Certificate Store. To check if your Git for Windows installation uses a compatible version run the following command:

```cmd
"C:\Program Files\Git\mingw64\bin\curl.exe" --version
```

When the version is higher than `7.60.0` you can configure Git to use the *schannel* as the SSL-Backend by executing the following command:

```cmd
git config --global http.sslBackend schannel
```

This solution has the benefit that no certificate files have to be managed and which leaves the Windows Certificate Store as the single point of truth when it comes to certificate trust.

Links:

[git config](https://www.git-scm.com/docs/git-config#Documentation/git-config.txt-httpsslCAInfo)

[schannel](https://learn.microsoft.com/en-us/windows/win32/secauthn/secure-channel)

<!-- TODO: Write article about that -->
<!-- [Convert Windows Certificate Store certificate into pem format](#)-->

## Fix it in Linux

Depending on your Linux distribution, the trusted certificates can be located in different directories. So convert your certificate into [pem - Base64 format](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail) and save it somewhere in your user directory. After that you can use a solution below that fits your distribution:

### Debian / Ubuntu / Gentoo etc

Copy your pem file into `/usr/local/share/ca-certificates/` with:

```bash
sudo cp <path to pem file> /usr/local/share/ca-certificates/
```

Update the certificate store with:

```bash
sudo update-ca-certificates
```

Configure Git to use the same directory to verify certificates using:

```bash
git config --global http.sslCAPath /usr/local/share/ca-certificates/
```

Alternatively you can set the `GIT_SSL_CAPATH` environment variable.

### Fedora / Red Head Enterprise Linux 6

Copy the text content of your pem file at the end of the file located at `/etc/pki/tls/certs/ca-bundle.crt`.

Configure Git to use the same file to verify certificates using:

```bash
git config --global http.sslCAInfo /etc/pki/tls/certs/ca-bundle.crt
```

Alternatively you can set the `GIT_SSL_CAINFO` environment variable.

### OpenSUSE

Copy the text content of your pem file at the end of the file located at `/etc/ssl/ca-bundle.pem`.

Configure Git to use the same file to verify certificates using:

```bash
git config --global http.sslCAInfo /etc/ssl/ca-bundle.pem
```

Alternatively you can set the `GIT_SSL_CAINFO` environment variable.

### OpenELEC

Copy the text content of your pem file at the end of the file located at `/etc/pki/tls/cacert.pem`.

Configure Git to use the same file to verify certificates using:

```bash
git config --global http.sslCAInfo /etc/pki/tls/cacert.pem
```

Alternatively you can set the `GIT_SSL_CAINFO` environment variable.

### Amazon Linux / CentOS / Red Head Enterprise Linux 7

Copy your pem file into `/etc/pki/ca-trust/source/anchors/` with:

```bash
sudo cp <path to pem file> /etc/pki/ca-trust/source/anchors/
```

Update the certificate store with:

```bash
sudo update-ca-trust extract
```

Configure Git to use the same directory to verify certificates using:

```bash
git config --global http.sslCAPath /etc/pki/ca-trust/source/anchors/
```

Alternatively you can set the `GIT_SSL_CAPATH` environment variable.

### Alpine Linux

Copy the text content of your pem file at the end of the file located at `/etc/ssl/cert.pem`.

Configure Git to use the same file to verify certificates using:

```bash
git config --global http.sslCAInfo /etc/ssl/cert.pem
```

Alternatively you can set the `GIT_SSL_CAINFO` environment variable.

Links:

[git config](https://www.git-scm.com/docs/git-config#Documentation/git-config.txt-httpsslCAInfo)

[Linux certificate file/folder locations](https://serverfault.com/questions/62496/ssl-certificate-location-on-unix-linux/722646)

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