---
title: Database Backups
category: database
summary: How do I perform a database backup?
---

# How do I perform a database backup?

## Backup
All databases are backed up automatically nightly. If you need to create a manual backup, however, the `db` command is there for you. All of these backups can be viewed with the [CLI backup commands](/compliant-cloud/cli-reference#db-backup). Backups are encrypted and stored in cloud storage and retained for 30 days. The following examples will show you how to create and list database backups performed via the CLI.

### Create a Backup
Execute the following command to create a backup.

```
$ datica -E "<your_env_name>" db list <service name>
```

List the backups for a database service. Take note that each of the backups has a corresponding ID that should be used in the case of restoring that specific backup. Also of note, if your database service is part of an HA configuration only the database identified as a primary (or master) node will be backed up nightly. Therefore when you display the backups for a secondary database node you will not see nightly backup entries but will on the primary node.

### Download a Backup
> ***Note:*** When downloading a database backup, be aware that you may be downloading PHI. Proceed with caution and insure that the appropriate disk-level encryption and access controls have been established prior to downloading a backup.

If you would like to download a backup, you can do so with the [`db download`](/compliant-cloud/cli-reference#db-download) command. This command is quite similar to the [`db export`](/compliant-cloud/articles/cli-database-export) command, but downloads a specific backup instead of the database's current state. For Postgres and MySQL databases, the download will be an SQL file, and for MongoDB, the download will be a gzipped tarball (.tar.gz).

> ***Note:*** Database backups are a **full** backup of your database, meaning that they contain users and permissions as well as schema and data. This means that you should **not** use the `db import` command to pull in a downloaded backup into your database.

Use the `db list` command to list the backups:

```
$ datica -E "<your_env_name>" db list <service name>
```

Copy the ID of the backup you'd like to download, and run the download command:

```
$ datica -E "<your_env_name>" db download <service name> <backup ID> <local file path>
```

Example:

```
$ datica -E "<your_env_name>" db download db01 abcd1234-4321-4321-4321-123412341234 ./mydbdownload.sql
```

To facilitate this download, the CLI creates a key pair as part of the request, and the export is encrypted server-side with that public key. The encrypted export is moved to cloud storage for a short window. The CLI downloads it, then decrypts it.

### See also
* [The Platform CLI](/compliant-cloud/articles/cli-stratum)
* [Database Export](/compliant-cloud/articles/cli-database-backup)