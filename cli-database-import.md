---
title: Database Import
category: database
summary: Learn how to import your data into Compliant Cloud.
---

## Basics

Importing into a database is the process through which data is transferred from a local file into your Compliant Cloud database. This is a very powerful operation, and can potentially cause problems - read on in the sections for your database type below.

The general form of the [command](/compliant-cloud/cli-reference#import):

```
datica -E "<your_env_name>" db import <service name> <local file path>
```

The [CLI](/compliant-cloud/cli-reference) will stream back to you any output from executing the import - any errors should be contained therein.

> ***Note:*** For all database types, no data will be deleted and no tables/database/schemas/collections will be dropped automatically.



### MySQL and PostgreSQL

Both MySQL and PostgreSQL databases' imports are expected to be in `sql` file format. An example file:

```
$ cat /path/to/my/data.sql

CREATE TABLE user_roles (
    id  text primary key,
    val text
);
INSERT INTO user_roles (id, val) VALUES ('1', 'admin');
INSERT INTO user_roles (id, val) VALUES ('2', 'user');
```

Example command:

```
$ datica -E "<your_env_name>" db import db01 /path/to/my/data.sql
```

For these databases, this script can contain any SQL you'd like - tables, databases, and users can be created, and data can be inserted. This goes both ways - databases and tables can be dropped, data can be truncated, and users can be removed. Be careful!

> ***Note:*** The [CLI's `console`](/compliant-cloud/cli-reference#console) command depends on the `catalyze` user and database to exist for both MySQL and PostgreSQL databases. If your import removes either of these or changes the `catalyze` user's password, the `console` will cease to function and you will need to [contact Datica support](/compliant-cloud/articles/contact) to resolve the issue.

### MongoDB Imports

MongoDB imports are expected to be gzipped+tar-compressed (.tar.gz) mongodumps. To create one of these from a local database, you'll need to use the `mongodump` and `tar` commands.

Creating an example database and collection with data:

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

Creating a dump of that example database and collection:

```
$ mongodump --db=catalyze --collection=user_roles
2015-08-11T11:45:05.128+0000    writing catalyze.user_roles to dump/catalyze/user_roles.bson
2015-08-11T11:45:05.131+0000    writing catalyze.user_roles metadata to dump/catalyze/user_roles.metadata.json
2015-08-11T11:45:05.132+0000    done dumping catalyze.user_roles (2 documents)
```

This will output a directory structure in `dump/`from the same directory as where the mongodump command was run. Using `tar` to compress that:

```
$ tar -cvzf mymongodump.tar.gz dump/
dump/
dump/catalyze/
dump/catalyze/user_roles.metadata.json
dump/catalyze/user_roles.bson
```

Mongo dumps only contain collections, so the CLI needs to be told what database the dumped collections should be imported into. To import, specifying the database:

```
$ datica -E "<your_env_name>" db import db01 mymongodump.tar.gz --mongo-database catalyze
```

MongoDB imports can only **add** data, not remove or alter it. If you need to delete collections or otherwise edit your data, use the [console command](/compliant-cloud/cli-reference#console).

### See also

* [Console](/compliant-cloud/cli-reference#console)
