---
title: Sites
category: concepts
summary: Mapping hostnames to certificates and services on Stratum.
---

The [Stratum](https://catalyze.io/stratum) concept of a **Site** represents the mapping of a hostname to a [code service](/stratum/articles/concepts/services#code-services). An [environment](/stratum/articles/concepts/environments) can have more than one code service, so its [service proxy](/stratum/articles/concepts/service-proxy) needs to know how to route requests.

## Creating a New Site

> ***Note:*** An [SSL Certificate](/stratum/articles/ssl-certs) needs to be uploaded before a site can be created.

To create a new site entry, three pieces of information are needed.

1. Hostname (this **cannot** be a wildcard)
2. Certificate name
3. Code service label

The hostname is up to you, and the certificate name is also decided by you when uploaded. The code service label is the name of the code service that you intend traffic for this hostname to go to. If you don't know what your code service's name is, you can find it in the [Stratum dashboard](https://product.catalyze.io/stratum).

The [CLI](/stratum/articles/cli-stratum) [sites create](/paas/paas-cli-reference#sites-create) command is used to create the site, taking the form `catalyze sites create <hostname> <code service label> <certificate name>`. For example, to set up a site mapping `example.com` to a code service named `code-1` using an uploaded cert named `example`:

```
catalyze sites create .example.com code-1 example
```

Note the leading `.` - this is required for apex domains.

If you've uploaded a wildcard cert and intend to use it with multiple subdomains, that cert can be used as many times as needed:

```
catalyze sites create api1.example.com code-1 wildcard-example
catalyze sites create api2.example.com code-2 wildcard-example
catalyze sites create api3.example.com code-3 wildcard-example
```

In order for your service proxy to pick up the new site, it needs to be redeployed:

```
catalyze redeploy service_proxy
```

## Listing Sites

To list what sites your environment has configured, use the [sites list](/paas/paas-cli-reference#sites-list) command. For details about a specific site, use the [sites show](/paas/paas-cli-reference#sites-show) command.

If a site needs to be removed, use the [sites rm](/paas/paas-cli-reference#sites-rm) command.

### See also

* [Initial Setup](/stratum/articles/initial-setup)
* [SSL Certificates](/stratum/articles/ssl-certs)
* [Service Proxy](/stratum/articles/concepts/service-proxy)
