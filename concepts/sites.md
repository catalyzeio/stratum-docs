---
title: Sites
category: concepts
summary: Mapping hostnames to certificates and services on The Platform.
---

[The Platform](https://datica.com/platform) concept of a **Site** represents the mapping of a hostname to a [code service](/compliant-cloud/articles/concepts/services#code-services). An [environment](/compliant-cloud/articles/concepts/environments) can have more than one code service, so its [service proxy](/compliant-cloud/articles/concepts/service-proxy) needs to know how to route requests.

## Creating a New Site

> ***Note:*** An [SSL Certificate](/compliant-cloud/articles/ssl-certs) needs to be uploaded before a site can be created.

To create a new site entry, three pieces of information are needed.

1. Hostname (this **cannot** be a wildcard)
2. Certificate name
3. Code service label

The hostname is up to you, and the certificate name is also decided by you when uploaded. The code service label is the name of the code service that you intend traffic for this hostname to go to. If you don't know what your code service's name is, you can find it in [The Platform dashboard](https://product.datica.com/environments).

The [CLI](/compliant-cloud/articles/cli-platform) [sites create](/compliant-cloud/cli-reference#sites-create) command is used to create the site, taking the form `datica -E "<your_env_name>" sites create <hostname> <code service label> <certificate name>`. For example, to set up a site mapping `example.com` to a code service named `code-1` using an uploaded cert named `example`:

```
datica -E "<your_env_name>" sites create .example.com code-1 example
```

Note the leading `.` - this is required for apex domains.

If you've uploaded a wildcard cert and intend to use it with multiple subdomains, that cert can be used as many times as needed:

```
datica -E "<your_env_name>" sites create api1.example.com code-1 wildcard-example
datica -E "<your_env_name>" sites create api2.example.com code-2 wildcard-example
datica -E "<your_env_name>" sites create api3.example.com code-3 wildcard-example
```

In order for your service proxy to pick up the new site, it needs to be redeployed:

```
datica -E "<your_env_name>" redeploy service_proxy
```

## Listing Sites

To list what sites your environment has configured, use the [sites list](/compliant-cloud/cli-reference#sites-list) command. For details about a specific site, use the [sites show](/compliant-cloud/cli-reference#sites-show) command.

If a site needs to be removed, use the [sites rm](/compliant-cloud/cli-reference#sites-rm) command.

### See also

* [Initial Setup](/compliant-cloud/articles/initial-setup)
* [SSL Certificates](/compliant-cloud/articles/ssl-certs)
* [Service Proxy](/compliant-cloud/articles/concepts/service-proxy)
