---
title: Database General Information
category: database
summary: Learn about databases on The Platform.
---

## What versions of the [supported databases](/compliant-cloud/articles/supported-databases/) does Datica use?

- Percona 5.6
- PostgreSQL 9.4
- MongoDB 2.6

### How do database backups work?
We perform automated backups each night. Access to the backups is provided via The Platform CLI. You can read about how to interact with backups [here](../cli-database-backup).

## How do I connect to my database?
The endpoint connection information for the resources your application will consume is available in the environment variables for your environment.  You can view your environment variables in the Datica Dashboard or with the Datica CLI client program with the command: `datica -E "<your_env_name>" vars list <service_name>`.

You can download the CLI here - https://github.com/daticahealth/cli. All  install instructions are also available in the readme.

To view environment variables in the dashboard, navigate to the environment that contains the database service you're interested in. Once you find the environment you're looking for, expand the services list by clicking on the `View Environment Details` link. Once you see the database service click the `View details` link. This will open up the services details view. Next you'll want to click on the Environment Variables tab. There you will find the necessary environment variables for your database.

Note that this information will only be available *after* you have signed an order and we have provisioned your environment, or once you have successfully gone through self-service on-boarding.