---
title: Environments
category: concepts
summary: Learn what Environments are on Compliant Cloud.
alias: "compliant-cloud/articles/environment-defining/"
---

**Environments** are the highest-level object that a user can interact with on [Compliant Cloud](https://datica.com/compliant-cloud). An Environment represents your entire hosted application or product - runtimes, databases, caches, queues, tools, and all.

An Environment consists primarily of a set of [Services](/compliant-cloud/articles/concepts/services). At a minimum, an Environment has:

* 1 [Code Service](/compliant-cloud/articles/concepts/services#code-services)
* 1 [Database Service](/compliant-cloud/articles/concepts/services#database-services)
* 1 [Service Proxy](/compliant-cloud/articles/concepts/service-proxy)
* 1 [Logging Service](/compliant-cloud/articles/logging-access)
* 1 [Monitoring Service](/compliant-cloud/articles/monitoring)

Each environment has its own isolated network, allowing those containers to find each other - and no one else. That is to say, with the exception of [Service Proxies](/compliant-cloud/articles/concepts/service-proxy) (which have ports 80 and 443 exposed to the outside world), your containers are not accessible except to each other.

An Environment can expand to have any number of services - as many as you need. An Environment does not have any [containers](/compliant-cloud/articles/concepts/containers) of its own - only those of its services.

### See also

* [The Compliant Cloud CLI](/compliant-cloud/articles/cli-stratum)
