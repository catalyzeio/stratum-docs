---
title: Catalyze Resources - HA Mongo
category: database
summary: How does Stratum manage HA Mongo?
---

# How does Stratum manage HA Mongo?

Highly Available MongoDB on Stratum uses Replica Sets to provide replication and automated failover in the event of a MongoDB node failure. We handle all the replica set configuration within your environment and expose a single connection URI in an environment variable called `DATABASE_URL`. You will need to use that connection string to connect your application's client to MongoDB and make use of the HA features. The `DATABASE_URL` environment variable follows the format defined by [MongoDB](http://docs.mongodb.org/manual/reference/connection-string/). Here is an example connection string:

```
DATABASE_URL="mongodb://127.0.1.1:27017,127.0.1.2:27017/myDB?replicaSet=myReplSet"
```

In the above example, the primary MongoDb node is at 127.0.1.1:27017 and the secondary node is at 127.0.1.2:27017. `myDB` will be the default database to connect to. The replica set to connect to is named `myReplSet`.

You will need to write your application using a client that supports replica sets. To test your application we recommend that you follow the instructions in [this blog post](http://blog.ajduke.in/2013/05/31/setup-mongodb-replica-set-in-4-steps/). This will give you a local testing environment that is configured in the same way as your HA MongoDB on Catalyze. You should use this local MongoDB for testing prior to deploying your code on catalyze.
