---
title: Database General Information
category: database
summary: Learn about databases on Compliant Cloud.
---

## What versions of the [supported databases](/compliant-cloud/articles/supported-databases/) does Datica use?

- Percona 5.6
- PostgreSQL 9.4
- MongoDB 2.6

### How do database backups work?

We perform automated backups each night. Access to the backups is provided via the Compliant Cloud CLI. You can read about how to interact with backups [here](../cli-database-backup).

## How do I connect to my database?

The endpoint connection information for the resources your application will consume is available in the environment variables for your environment.  You can view your environment variables in the Datica Dashboard or with the catalyze cli client program with the command:  `datica -E "<your_env_alias>" vars list <service_name>`

You can download the CLI here - https://github.com/daticahealth/cli. All  install instructions are also available in the readme.

Note that this information will only be available *after* you have signed an order and we have provisioned your environment.
