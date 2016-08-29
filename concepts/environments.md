---
title: Environments
category: concepts
summary: Learn what Environments are on Stratum.
alias: "stratum/articles/environment-defining/"
---

**Environments** are the highest-level object that a user can interact with on [Stratum](https://catalyze.io/stratum). An Environment represents your entire hosted application or product - runtimes, databases, caches, queues, tools, and all.

An Environment consists primarily of a set of [Services](/stratum/articles/concepts/services). At a minimum, an Environment has:

* 1 [Code Service](/stratum/articles/concepts/services#code-services)
* 1 [Database Service](/stratum/articles/concepts/services#database-services)
* 1 [Service Proxy](/stratum/articles/concepts/service-proxy)
* 1 [Logging Service](/stratum/articles/logging-access)
* 1 [Monitoring Service](/stratum/articles/monitoring)

Each environment has its own isolated network, allowing those containers to find each other - and no one else. That is to say, with the exception of [Service Proxies](/stratum/articles/concepts/service-proxy) (which have ports 80 and 443 exposed to the outside world), your containers are not accessible except to each other.

An Environment can expand to have any number of services - as many as you need. An Environment does not have any [containers](/stratum/articles/concepts/containers) of its own - only those of its services.

### See also

* [The Stratum CLI](/stratum/articles/cli-stratum)
