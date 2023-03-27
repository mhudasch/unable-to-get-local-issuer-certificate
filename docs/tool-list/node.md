---
layout: tool
name: Node
title: Node.js
permalink: /tools/node
parent: Troubleshooting by Tool

tool_description: >
  Node.js is an open-source, cross-platform JavaScript runtime environment.
tool_docs: https://nodejs.org/en/docs

date: 2023-03-22
published: true
---

{% capture sample %}
The error might look like this when using {{ page.title }}:

```text
...
fatal: unable to access <URL>: SSL certificate problem: unable to get local issuer certificate
...
```

{% endcapture %}

{% include tool_head.md issue_sample=sample -%}

## All platforms

Node.js comes with its own store of trustworthy CA certificates, that are updated with each update of Node.js. This certificates are not stored in a certificate file but rather is hard-coded into the Node.js source code as you can see [here](https://github.com/nodejs/node/blob/main/src/node_root_certs.h).

The certificate store that comes with the Node.js installation can only be extended by creating a central file that contains *additional* certificates in [pem - Base64 format](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail). Node.js supports an environment variable `NODE_EXTRA_CA_CERTS` that points to the aforementioned central file.

You can use the environment variable using the following command(s):

### Windows

For the current shell session:

```shell
set NODE_EXTRA_CA_CERTS=<path\\to\\pem\\file>
```

Or for all following shell sessions:

```shell
setx NODE_EXTRA_CA_CERTS <path\\to\\pem\\file>
```

Or in PowerShell:

```powershell
$env:NODE_EXTRA_CA_CERTS="</path/to/pem/file>"
```

Use (`\\`) as path separator when using Windows cmd/DOS and (`` `\ `` or `/`) as path separator in PowerShell.
{: .fs-3 .fw-300}

### Linux / macOS

```bash
export NODE_EXTRA_CA_CERTS=</path/to/pem/file>
```

{: .note }
The path to the pem file can be absolute or relative.

The documentation for the environment variable can be found [here](https://nodejs.org/api/cli.html#node_extra_ca_certsfile).

## Using openSSL certificates

When you have openSSL installed, you can start every Node.js process with the command-line argument `--use-openssl-ca` to prevent double configuration. Sometimes you can not define the command-line arguments for the process. In those cases you can add the argument to an environment variable with the name `NODE_OPTIONS`.

The documentation for the argument can be found [here](https://nodejs.org/api/cli.html#ssl_cert_dirdir).

When the command-line argument is used, Node.js respects the openSSL environment variables `SSL_CERT_DIR` or `SSL_CERT_FILE`. This prevents parallel configuration.
