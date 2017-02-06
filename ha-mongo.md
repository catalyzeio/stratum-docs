---
title: HA MongoDB
category: database
summary: How does Compliant Cloud manage HA Mongo?
---

**[HA](/compliant-cloud/articles/ha-application) MongoDB** [services](/compliant-cloud/articles/concepts/services) on [Compliant Cloud](https://datica.com/compliant-cloud) use [Replica Sets](https://docs.mongodb.com/manual/replication/) to provide automated failover in the event of a MongoDB node failure. Compliant Cloud handles the replica set configuration within your environment, and expose a single connection URI in an environment variable, `DATABASE_URL`.

> ***Note:*** The variable may not always be named `DATABASE_URL` if the environment also contains more than one database service. There will always also be another variable named after the service, such as `MONGO01_URL` or `DB01_URL`.

The environment variable's value follows the format defined by [MongoDB](https://docs.mongodb.com/manual/reference/connection-string/). An example connection string:

```
mongodb://127.0.1.1:27017,127.0.1.2:27017/myDB?replicaSet=myReplSet
```

In the above example, the primary MongoDb node is at `127.0.1.1:27017` and the secondary node is at `127.0.1.2:27017`. `myDB` will be the default database to connect to. The replica set to connect to is named `myReplSet`.

You will need to write your application using a client that supports replica sets - official driver list [here](https://docs.mongodb.com/ecosystem/drivers/).

To test your application locally using a replica set, we recommend that you follow the instructions in [this blog post](https://blog.ajduke.in/2013/05/31/setup-mongodb-replica-set-in-4-steps/). This will give you a local testing environment that is configured very similarly to your HA MongoDB on Compliant Cloud.

### See also

* [Local Testing](/compliant-cloud/articles/guides/local-testing)
* [Node + MongoDB Example Application](/compliant-cloud/articles/guides/node-mongo)
