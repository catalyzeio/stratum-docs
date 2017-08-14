---
title: The Platform Supported Databases
category: database
summary: Learn about the supported databases you can use on The Platform.
---

# The Platform Supported Databases
All databases supported by The Platform are available either in single node or highly available (HA) configurations.

Databases supported in the single node deployment are:
- Postgres 9.6
- MySQL (Percona) 5.7
- mongoDB 3.2

The currently supported databases are also available in HA mode which is described more below.

## Postgres HA
Postgres HA is configured as follows:
- A pair of Postgres containers deployed as master and slave respectively
- Streaming replication is enabled between the master and slave
- Promotion of slave to master manually triggered by Datica engineer

## MySQL (Percona) HA
Percona MySQL HA is configured as follows:
- A pair of Percona MySQL containers deployed as master and slave respectively
- Promotion of slave to master manually triggered by Datica engineer

## MongoDB HA
mongoDB HA is configured as follows:
- A pair of mongoDB containers are deployed as master and slave
- An arbiter node manages them and promotes the slave to a read-only master if a master failure occurs
