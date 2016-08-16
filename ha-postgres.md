---
title: HA PostgreSQL
category: database
Summary: How does Stratum manage HA PostgreSQL?
---

**[HA](/stratum/articles/ha-application) PostgreSQL** [services](/stratum/articles/concepts/services) on [Stratum](https://catalyze.io/stratum) use a [master/slave](https://www.postgresql.org/docs/9.4/static/high-availability.html) (or primary/standby) configuration, with streaming replication enabled between the database [containers](/stratum/articles/concepts/containers).

Currently, there is no automated failover. Promotion to primary is manually managed by Catalyze support. If you observe a failure and have not already been notified of support activity, [contact Catalyze support](/stratum/articles/contact) immediately.

### See also

* [Python + PostgreSQL Example Application](/stratum/articles/guides/python-postgres)
