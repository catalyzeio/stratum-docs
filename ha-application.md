---
title: Highly-Available Services
category: application
summary: How can The Platform Ensure High Application Ability?
---

**Highly-Available** (HA) [services](/compliant-cloud/articles/concepts/services) are services deployed in configurations that allow them to better deal with unexpected stability issues. A service can go down for a number of reasons - application fault, memory issues, deadlocks, or host failures (The Platform does [pretty well](http://status.datica.com/), but no hardware is immune to failure).

At a high level, making a service HA means to scale it up - that is, to set the number of [deploy jobs](/compliant-cloud/articles/concepts/jobs#deploy-jobs). For specific service types, it gets more complex. As rule of thumb, Datica highly recommends all services be set to HA for provided stability.

## HA Code Services
An HA [code service](/compliant-cloud/articles/concepts/services#code-services) simply is configured to have two or more deploy jobs running simultaneously. During the provisioning of an HA code service, Datica will also increase the scale of the [service proxy](/compliant-cloud/articles/concepts/service-proxy). Traffic will be routed via round-robin from the environment's load balancer between service proxies, which will in turn only route to running deploy jobs containers for that service.

## HA Database Services
HA [database services](/compliant-cloud/articles/concepts/services#database-services) vary in setup depending on the type of database.

* [HA PostgreSQL](/compliant-cloud/articles/ha-postgres)
* [HA MySQL](/compliant-cloud/articles/ha-mysql)
* [HA MongoDB](/compliant-cloud/articles/ha-mongo)

For details on how a database type not listed above could be made HA, [contact us](/compliant-cloud/articles/contact).

## HA Cache Services
HA [cache services](/compliant-cloud/articles/concepts/services#cache-services) vary in setup depending on the type of database.

* [HA Redis](/compliant-cloud/articles/ha-redis)

For details on how a cache type not listed above could be made HA, [contact us](/compliant-cloud/articles/contact).