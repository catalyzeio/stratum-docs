---
title: Highly-Available Services
category: application
summary: How can Stratum Ensure High Application Ability?
---

**Highly-Available** (HA) [services](/stratum/articles/concepts/services) are services deployed in configurations that allow them to better deal with unexpected stability issues. A service can go down for a number of reasons - application fault, memory issues, deadlocks, or host failures (Stratum does [pretty well](http://status.catalyze.io/), but no hardware is immune to failure).

At a high level, making a service HA means to scale it up - that is, to set the number of [deploy jobs](/stratum/articles/concepts/jobs#deploy-jobs). For specific service types, it gets more complex.

## HA Code Services

An HA [code service](/stratum/articles/concepts/services#code-services) simply is configured to have two or more deploy jobs running simultaneously. During the provisioning of an HA code service, Catalyze will also increase the scale of the [service proxy](/stratum/articles/concepts/service-proxy). Traffic will be routed via round-robin from the environment's load balancer between service proxies, which will in turn only route to running deploy jobs containers for that service.

## HA Database Services

HA [database services](/stratum/articles/concepts/services#database-services) vary in setup depending on the type of database.

* [HA PostgreSQL](/stratum/articles/ha-postgres)
* [HA MySQL](/stratum/articles/ha-mysql)
* [HA MongoDB](/stratum/articles/ha-mongo)

For details on how a database type not listed above could be made HA, [contact us](/stratum/articles/contact).

## HA Cache Services

HA [cache services](/stratum/articles/concepts/services#cache-services) vary in setup depending on the type of database.

* [HA Redis](/stratum/articles/ha-redis)

For details on how a cache type not listed above could be made HA, [contact us](/stratum/articles/contact).
