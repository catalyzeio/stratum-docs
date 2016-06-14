---
title: Database Export
category: database
summary: Learn how to export database data from the Stratum CLI.
---

## Export
> ***Important Note:*** When exporting data from your database be aware that you maybe downloading PHI onto the local hard drive. Proceed with caution and insure that the appropriate disk-level encryption and access controls have been established prior to initiating a database export.

Using the Catalyze CLI "export" command allows you to download the latest dump of your database service. The export command is easy to use and is equivalent to download a backup of your database. This may be helpful for running metrics collections offline against a production database or a variety of other uses. You **should not** import anything exported with the "export" command. To use the "export" command, you'll specify the name of the database service to export and a file path for the download location. Here is an example usage:

```
$ catalyze db export db01 ./mydbexport.sql
…
```

Please note that you should not give the export command the location of a directory but the full path to a file. This file does not need to exist as it will be created.

'db01' is the name of your database service and './mydbexport.sql' is the path where you want the export saved to. Exports are similar to using the backup process described later in this article. An export first creates a backup of your entire database. When the backup is finished it is encrypted and stored in cloud storage. Once finished, a unique temporary signed URL is retrieved and used to securely download your encrypted backup. This temporary URL will expire shortly after the download completes. Once downloaded, the file is then decrypted on your local machine and stored at the given path. Since the export creates a backup you will be able to see the record of your export when listing the backups for the database service (more about this in the Backup section below).
