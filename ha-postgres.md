---
title: HA PostgreSQL
category: database
Summary: How does The Platform manage HA PostgreSQL?
---

**[HA](/compliant-cloud/articles/ha-application) PostgreSQL** [services](/compliant-cloud/articles/concepts/services) on [The Platform](https://datica.com/compliant-cloud) use a [master/slave](https://www.postgresql.org/docs/9.4/static/high-availability.html) (or primary/standby) configuration, with streaming replication enabled between the database [containers](/compliant-cloud/articles/concepts/containers).

Currently, there is no automated failover. Promotion to primary is manually managed by Datica support. If you observe a failure and have not already been notified of support activity, [contact Datica support](/compliant-cloud/articles/contact) immediately.

### See also

* [Python + PostgreSQL Example Application](/compliant-cloud/articles/guides/python-postgres)
