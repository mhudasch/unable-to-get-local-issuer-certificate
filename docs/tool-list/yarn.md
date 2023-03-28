---
layout: tool
name: Yarn
title: Yarn
permalink: /tools/yarn
parent: Troubleshooting by Tool

tool_description: >
  Yarn is an alternative to npm as package manager for the Node.js runtime.
tool_docs: https://yarnpkg.com/getting-started

date: 2023-03-28
published: true
---

{% capture sample %}
Yarn 1.x

```text
...
Trace: 
  Error: unable to get local issuer certificate
    at TLSSocket.onConnectSecure
  .. <stack-trace> ..
...
```

Yarn 2.x

```text
...
YN0001: │ GotError: unable to get local issuer certificate
...
```

{% endcapture %}

{% include tool_head.md issue_sample=sample -%}

## All Platforms

### Yarn 1.x

Yarn 1.x takes many configurations directly from Npm by reading the `.npmrc` and supports the Node.js environment variables. For that reason you can follow the guid for npm [here]({{ '/tools/npm' | relative_url }}).

### Yarn 2.x

Yarn 2.x doesn't use the configuration from your `.npmrc` files anymore; it instead reads all of the configuration from the `.yarnrc.yml` files whose available settings can be found [here](https://yarnpkg.com/configuration/yarnrc). The `.yarnrc.yml` file must be placed inside your Node.js project and not inside your user directory in order to work correctly.

This means that configurations for certificates must be done inside `.yarnrc.yml`. You can configure Yarn to use a central trusted CA certificate bundle file in [pem - Base64 format](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail) using the following entry in the config file:

```yaml
# .yarnrc.yml

caFilePath: "<path to pem file>"
```

The path to the pem file can be absolute or relative.
{: .note }

Another configuration would be `httpsCertFilePath` which works like `caFilePath` but for the whole SSL certificate chain and not only the trusted root CA certificate. So if you have problems with intermediate certificates, this would be the place to check.

**Do not** use settings like `yarn config set enableStrictSsl false` or `npm config set strict-ssl false` because they disable the security feature completely even if you are using a self-signed certificate. It is always better to secure the SSL connection by making a certificate known to npm/Node.js.
{: .warning}
