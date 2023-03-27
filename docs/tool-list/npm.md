---
layout: tool
name: npm
title: npm
permalink: /tools/npm
parent: Troubleshooting by Tool

tool_description: >
  npm is the world's largest software registry of packages for the Node.js runtime.
tool_docs: https://docs.npmjs.com/

date: 2023-03-27
published: true
---

{% capture sample %}
The error might look like this when using {{ page.title }}:

```text
...
npm ERR! code UNABLE_TO_GET_LOCAL_ISSUER_CERT_LOCALLY

npm ERR! unable to get local issuer certificate
...
```

{% endcapture %}

{% include tool_head.md issue_sample=sample -%}

## All Platforms

Npm has multiple configurations to enable additional trusted certificates.

Npm supports the `NODE_EXTRA_CA_CERTS` environment variable as described in the guide for [Node.js]({{ '/docs/tool-list/node' | relative_url }}).

In isolation, Npm can be configured to use a central file for trusted certificates (bundle file) in [pem - Base64 format](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail). This configuration can be done by using the following command (make entries into `.npmrc` file):

```shell
npm config set cafile "<path to pem file>"
```

[Documentation for configuration](https://docs.npmjs.com/cli/using-npm/config)

An additional way to add trusted certificates to Npm is to add the certificates manually to the `.npmrc` file in [pem - Base64 format](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail). For example:

```toml
# .npmrc file
# one trusted ca certificate string
# line-breaks replaced by \n
ca = "-----BEGIN CERTIFICATE-----\n<base64 cert (.pem/.CER)>\n-----END CERTIFICATE-----"
```

```toml
# .npmrc file
# multiple trusted ca certificate strings
ca[] = "-----BEGIN CERTIFICATE-----\n<base64 cert (.pem/.CER)>\n-----END CERTIFICATE-----"
ca[] = "-----BEGIN CERTIFICATE-----\n<base64 cert (.pem/.CER)>\n-----END CERTIFICATE-----"
```

The solutions above, when already configured, might also be the source of other certificate problems, so always check these places for a correct configuration:

- `.npmrc` file (ca setting)
- `.npmrc` file (cafile setting)
- `NODE_EXTRA_CA_CERTS` environment variable

**Do not** use settings like `npm set strict-ssl=false` or `NODE_TLS_REJECT_UNAUTHORIZED=0` because they disable the security feature completely even if you are using a self-signed certificate. It is always better to secure the SSL connection by making a certificate known to npm/Node.js.
{: .warning}
