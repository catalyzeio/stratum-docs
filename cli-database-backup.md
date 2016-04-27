---
title: Database Backups
category: database
---

## Backup
The CLI backup command is there for you to create snapshots of your data, perhaps, in preparation for a software upgrade or data migration. A nightly backup of your database is also automated. All of these backups can be viewed with the CLI backup commands. Backups are encrypted and stored in cloud storage and retained for 7 days. The following examples will show you how to create and list database backups performed via the CLI.

### Create a Backup

Execute the following command to create a backup.

```
$ catalyze db backup db01
```

List the backups for a database service. Take note that each of the backups has a corresponding ID that should be used in the case of restoring that specific backup. Also of note, if your database service is part of an HA configuration only the database identified as a primary (or master) node will be backed up nightly. Therefore when you display the backups for a secondary database node you will not see nightly backup entries but will on the primary node.

For CLI version 2.1.0 and above, run

```
$ catalyze db list db01
```

### Download a Backup
> ***Important Note:*** When downloading a database backup be aware that you maybe downloading PHI onto the local hard drive. Proceed with caution and insure that the appropriate disk-level encryption and access controls have been established prior to initiating a database export.

If you would like to download yesterday's database backup to inspect it or maybe assist tracking down a bug you can do so with the backup download command. This command is quite similar to the database export command but in the command you'll specify the ID of the backup to download and the file path of where to download the file. The file downloaded will be of the same file format as expected for a database import (Postgres and MySQL backups will be SQL files and MongoDB backups will be downloaded as gzipped tarballs). Please note that since a downloaded backup and an export are identical, as with exports you **should not** import a downloaded backup.

Run the following command to list the backups.

```
$ catalyze db list db01
```

Now choose the one you want to download and copy its ID and download it.

```
$ catalyze db download db01 {backupID} ./mydbdownload.sql
```
