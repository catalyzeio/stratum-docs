---
title: HA MySQL
category: database
Summary: How does Compliant Cloud manage HA MySQL?
---

**[HA](/compliant-cloud/articles/ha-application) MySQL** [services](/compliant-cloud/articles/concepts/services) on [Compliant Cloud](https://datica.com/compliant-cloud) use [Percona](https://www.percona.com/) in a master/slave configuration, with streaming replication enabled between the database [containers](/compliant-cloud/articles/concepts/containers).

Currently, there is no automated failover. Promotion to primary is manually managed by Datica support. If you observe a failure and have not already been notified of support activity, [contact Datica support](/compliant-cloud/articles/contact) immediately.

### See also

* [PHP + MySQL Example Application](/compliant-cloud/articles/guides/php-mysql)
