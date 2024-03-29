---
layout: home
title: Welcome to a page dedicated to one error
nav_exclude: true
---

# {{ page.title }}
{: .no_toc }

---

## Want to know what the error means?

When developing software or just when using some software you might come across a cryptic error message that contains the error string this web page is dedicated to:

```text
... unable to get local issuer certificate ...
```

Most times this error occurs when a software tries to establish a SSL (HTTPS) connection to a server and the server certificate that secured the connection can not be trusted because the issuer of said certificate cannot be validated. But how can that be?

If you want to know more about the issue, look [here]({{ 'further-issue-details' | relative_url }}).

## Want a solution for the error

You can get general troubleshooting help [here]({{ 'common' | relative_url }}).

Alternatively you can get tool specific help [here]({{ 'tool-list' | relative_url }}).

## Did it help?

If this site was any help, please consider to

{% include donation.html %}
