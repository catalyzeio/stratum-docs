---
title: HA Postgres
category: database
Summary: How does Stratum manage HA Postgres?
---

# How does Stratum manage HA Postgres?

Our HA Postgresql is setup as a master/slave pair with streaming replication enabled between the two database containers.

Slave promotion to master is manually managed by Catalyze engineers.
