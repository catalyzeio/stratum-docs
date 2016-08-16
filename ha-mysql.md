---
title: HA MySQL
category: database
Summary: How does Stratum manage HA MySQL?
---

**[HA](/stratum/articles/ha-application) MySQL** [services](/stratum/articles/concepts/services) on [Stratum](https://catalyze.io/stratum) use [Percona](https://www.percona.com/) in a master/slave configuration, with streaming replication enabled between the database [containers](/stratum/articles/concepts/containers).

Currently, there is no automated failover. Promotion to primary is manually managed by Catalyze support. If you observe a failure and have not already been notified of support activity, [contact Catalyze support](/stratum/articles/contact) immediately.

### See also

* [PHP + MySQL Example Application](/stratum/articles/guides/php-mysql
