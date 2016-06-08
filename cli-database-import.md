---
title: Catalyze Resources - Database Import
category: database
summary: Learn how to import your data into Stratum.
---

## Import
All of the import functionality in the CLI starts with preparing a file to import. Here is a table outlining the file types appropriate for each supported database on the Catalyze platform.

```
Supported Import Filetype: (Database) - (File Extension)
PostgreSQL - .sql
MySQL(Percona)      - .sql
MongoDB    - .tar.gz
```

In general, the import command is to be used **only** when importing data to your database rather than migrating databases from another database service. Importing a dump into Catalyze from a different database is not recommended and poses the risk of data loss. If you need to import another database, we recommend uploading this locally and then performing a data only dump to be importing into Catalyze. You also **should not** import a file retrieved from the "export" command. The export downloads a full backup of your database including users, permissions, and data. The import command should only be used to import data.

### SQL Imports
Make sure you have associated an environment and are currently in the directory of the associated git repo. Let’s create a short script to create a table and insert a few rows.

```
$ cat myimport.sql

CREATE TABLE user_roles (
    id text primary key,
    val text
);
INSERT INTO user_roles (id, val) VALUES ('1', 'admin');
INSERT INTO user_roles (id, val) VALUES ('2', 'user');
```

Now let’s import the script!

```
$ catalyze db import db01 myimport.sql
```

After it successfully finishes importing your script, the CLI will dump the output of the command back to your terminal.

Note - nothing will be dropped/deleted unless the script you import explicitly does so. This goes both ways - if you have a `DROP DATABASE catalyze;` statement in your script, it **will** be executed. Be careful!

### MongoDB Imports

In this example we will start with an empty database and add some data which will be uploaded to our Catalyze database. First sign into your mongo database, we'll switch to a new database ("catalyze") and create a new collection and insert a few rows.

```
> use catalyze
switched to db catalyze
> db.createCollection('user_roles')
{ "ok" : 1 }
> db.user_roles.insert({"id":"1", "val":"admin"})
WriteResult({ "nInserted" : 1 })
> db.user_roles.insert({"id":"2", "val":"user"})
WriteResult({ "nInserted" : 1 })
> exit
bye
```

Now create a dump of the collection:

```
$ mongodump --db=catalyze --collection=user_roles
2015-08-11T11:45:05.128+0000    writing catalyze.user_roles to dump/catalyze/user_roles.bson
2015-08-11T11:45:05.131+0000    writing catalyze.user_roles metadata to dump/catalyze/user_roles.metadata.json
2015-08-11T11:45:05.132+0000    done dumping catalyze.user_roles (2 documents)
```

This will output a directory structure in `dump/`from the same directory as where the mongodump command was run. Now all you need to do is tar the `dump/` folder and perform a similar import command. Just specify the mongo database to import data into!

```
$ tar -cvzf mymongodump.tar.gz dump/
dump/
dump/catalyze/
dump/catalyze/user_roles.metadata.json
dump/catalyze/user_roles.bson
$ catalyze db import db01 mymongodump.tar.gz --mongo-database catalyze
```

While you are using the CLI check out the "console" command to log into the mongo database shell; read [here](https://resources.catalyze.io/paas/paas-cli-reference/console/) for more information about the secure console command.
