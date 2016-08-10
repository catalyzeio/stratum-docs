---
title: Environments
category: concepts
summary: Learn what Environments are on Stratum.
---

**Environments** on [Stratum](https://catalyze.io/stratum) are the highest-level object that a user can interact with on Stratum. An Environment represents your entire hosted application or product - runtimes, databases, caches, queues, tools, and all. If you have both production and staging hosted through

An Environment consists primarily of a set of [Services](/stratum/articles/concepts/services). At a minimum, an Environment has:

* 1 [Code Service](/stratum/articles/concepts/services#code-services)
* 1 [Database Service](/stratum/articles/concepts/services#database-services)
* 1 [Service Proxy](/stratum/articles/concepts/service-proxy)
* 1 [Logging Service](/stratum/articles/logging-access)
* 1 [Monitoring Service](/stratum/articles/monitoring)

An Environment can expand to have any number of services - as many as you need. An Environment does not have any [containers](/stratum/articles/concepts/containers) of its own - only those of its services.

### See also

* [The Stratum CLI](/stratum/articles/cli-stratum)
