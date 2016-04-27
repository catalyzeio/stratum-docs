---
title: Stratum Supported Databases
category: database
---

# Stratum Supported Databases

All databases supported by Stratum are available either in single node or highly available (HA) configurations.

Databases supported in the single node deployment are:
- Postgres
- MySQL (Percona)
- mongoDB

Support is planned for the following shortly (again to be prioritized based on customer demand)
- Couchdb - [Request Support](mailto:support@catalyze.io?subject=CouchDB support)
- SQLite - [Request Support](mailto:support@catalyze.io?subject=SQLite support)
- Cassandra - [Request Support](mailto:support@catalyze.io?subject=Cassandra support)
- HBase - [Request Support](mailto:support@catalyze.io?subject=HBase support)
- Other - [Request Support](mailto:support@catalyze.io?subject=Other DB support)

The currently supported databases are also available in HA mode which is described more below.

## Postgres HA
Postgres HA is configured as follows:
- A pair of Postgres containers deployed as master and slave respectively
- Streaming replication is enabled between the master and slave
- Promotion of slave to master manually triggered by Catalyze engineer

## MySQL (Percona) HA
Percona MySQL HA is configured as follows:
- A pair of Percona MySQL containers deployed as master and slave respectively
- Promotion of slave to master manually triggered by Catalyze engineer

## MongoDB HA
mongoDB HA is configured as follows:
- A pair of mongoDB containers are deployed as master and slave
- An arbiter node manages them and promotes the slave to a read-only master if a master failure occurs
