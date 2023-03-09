---
layout: page
title: Further issue details
permalink: /further-issue-details/
order: 2
---

When developing software or just when using some software you might come across a cryptic error message that contains the error string this web page is dedicated to:

```shell
... unable to get local issuer certificate ...
```

Most times this error occurs when a software tries to establish a SSL (HTTPS) connection to a server and the server certificate that secured the connection can not be trusted because the issuer of said certificate cannot be validated. But how can that be?

Again most times this is no problem because the software that you use has access to a *store* or directory that contains a collection of certificate files that can validate the issuers of server certificates. But in some edge cases (frequent for workers in software security or software development) the certificate to validate certificate issuers are not present in those *stores* or directories. The cases when this happens is most times one of the following:

1. You use so-called **Self-Signed-Certificate** to secure a HTTP connection
2. Your company uses secure Web Proxy that performs **SSL interception** for certain HTTP connections
3. Your certificate store is not up-to-date or the software you are using comes with its own store for trusted issuer certificates that is not up-to-date
4. A combination of the ones above

Many answers on pages like [stackoverflow](https://stackoverflow.com) or other user forums suggest just disabling the security option in the software you use by setting some option like `strict-ssl=false` or `validate-server-certificate=false` etc. or just use a `http` url instead to *fix* the issue an go on quickly. Please **DO NOT** do this because you are disabling security not fixing the issue. Please **DO** follow the steps provided below to actually fix the issue for your particular case and tool.
