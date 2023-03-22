---
layout: post
name: Node
title: Node (node.js)
category: tool_article
date: 2023-03-22
published: false
---

- Do not remove this line (it will not be displayed)
{:toc}

## What is it?

Node.js® is an open-source, cross-platform JavaScript runtime environment.

[Official Documentation](https://nodejs.org/en/docs)

You can always check the [general troubleshooting guide]({{ site.baseurl }}/common/) before continuing.

## Issue sample

The error might look like this when using Git:

```text
...
fatal: unable to access <URL>: SSL certificate problem: unable to get local issuer certificate
...
```

## All platforms

When you have openSSL installed, you can start every Node.js process with the command-line argument `--use-openssl-ca` to prevent double configuration. Sometimes you can not define the command-line arguments for the process. In those cases you can add the argument to an environment variable with the name `NODE_OPTIONS`.

The documentation for the argument can be found [here](https://nodejs.org/api/cli.html#ssl_cert_dirdir).

When the command-line argument is used, Node.js respects the openSSL environment variables `SSL_CERT_DIR` or `SSL_CERT_FILE`. This prevents parallel configuration.

## Fix it in Windows

Node.js comes with its own store of trustworthy certificates, that are updated with each update of Node.js. The store can only be extended by creating a central file that contains additional certificates in [pem - Base64 format](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail). Node.js supports an environment variable `NODE_EXTRA_CA_CERTS` that points to the aforementioned central file.

When you the certificates in your company are rolled out with group policies or similar processes it is recommended to run a script regularly that exports the trusted CA certificates and trusted intermediate certificates from the Windows Certificate Store into a central file in [pem - Base64 format](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail).

This can be done for the current shell session by executing the following script:

```cmd
set NODE_EXTRA_CA_CERTS=<path to pem file>
```

Or for all following shell sessions by executing the following script:

```cmd
setx NODE_EXTRA_CA_CERTS <path to pem file>
```

Note: The path to the pem file can be absolute or relative.

The documentation for the environment variable can be found [here](https://nodejs.org/api/cli.html#node_extra_ca_certsfile).

## Fix it in Linux / macOS

Node.js comes with its own store of trustworthy certificates, that are updated with each update of Node.js. The store can only be extended by creating a central file that contains additional certificates in [pem - Base64 format](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail). Node.js supports an environment variable `NODE_EXTRA_CA_CERTS` that points to the aforementioned central file.

Depending on your Linux distribution, the trusted certificates are already one file in [pem - Base64 format](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail) (crt file) or can easily copied into one file. Finally you have the point the environment variable `NODE_EXTRA_CA_CERTS` to that one file.

This can be done by executing the following script:

```bash
export NODE_EXTRA_CA_CERTS=<path to pem file>
```
