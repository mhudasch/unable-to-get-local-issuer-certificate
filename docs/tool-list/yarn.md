---
layout: tool
name: yarn
title: yarn
permalink: /tools/yarn
parent: Troubleshooting by Tool

tool_description: >
  yarn is an alternative package manager for the Node.js runtime.
tool_docs: https://docs.npmjs.com/

date: 2023-03-27
published: false
---

{% capture sample %}
The error might look like this when using {{ page.title }}:

```text
...
Trace: 
  Error: unable to get local issuer certificate
    at TLSSocket.onConnectSecure
  .. <stack-trace> ..
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

**Do not** use settings like `yarn config set enableStrictSsl false` or `yarn config set "strict-ssl" false` because they disable the security feature completely even if you are using a self-signed certificate. It is always better to secure the SSL connection by making a certificate known to npm/Node.js.
{: .warning}
