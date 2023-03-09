---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
title: Welcome to a page only dedicated to one error
---

## Want to know what the error means?

When developing software or just when using some software you might come across a cryptic error message that contains the error string this web page is dedicated to:

```shell
... unable to get local issuer certificate ...
```

Most times this error occurs when a software tries to establish a SSL (HTTPS) connection to a server and the server certificate that secured the connection can not be trusted because the issuer of said certificate cannot be validated. But how can that be?

If you want to know more about the issue, look [here]({{ site.baseurl }}further-issue-details).

## Want a solution for the error

Checkout the help articles I wrote up for the following tools:

{% for tool in site.tool_articles %}

- [{{ tool.name }}]({{ site.baseurl }}{{ tool.url }})

{% endfor %}
