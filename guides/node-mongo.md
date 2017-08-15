---
title: Node + MongoDB Guide
category: guide
---

# Deploying a Node+Mongo Application on The Platform

In this guide we will cover how to deploy a simple [Node.js](https://nodejs.org/) application that stores data in a [MongoDB database](https://www.mongodb.com/).

## Pre-requisites

Before getting started, make sure you created your Platform account, have signed any [business associate agreements](https://datica.com/learn/business-associate-agreements) with Datica as necessary, and have successfully provisioned an environment with a code service and a MongoDB service. If you need help getting started, hop on over to [The Platform Getting Started](/compliant-cloud/getting-started) section available right here on [resources site](https://resources.datica.com/).

You should also have Git, Node.js, NPM, Mongo, and [The Platform CLI](https://github.com/daticahealth/cli) installed and available on your system path.

## Local Node.js Application

To get started we will create a simple Node.js application using the [Express framework](http://expressjs.com/). If you want to cut down on typing you can [clone the sample app from GitHub](https://github.com/catalyzeio/nodejs-example-app) - be sure to start from the initial commit so you can play along at home.

1. Create a `package.json` file with a dependency on `express` and `mongodb`.

   ```
   {
     "name": "catalyze-nodejs-mongo-demo",
     "description": "Sample Node.js application for the Datica Platform",
     "version": "0.0.1",
     "main": "index.js",
     "scripts": {
       "start": "node index.js"
     },
     "engines": {
       "node": ">=0.10.0"
     },
     "dependencies": {
       "express": "^4.9.5",
       "mongodb": "^1.4.12"
     }
   }
   ```

   Make sure to specify the appropriate `node` version in your `engines` section, particularly if you rely on newer Node.js runtime features.

2. Use `npm` to pull down your dependencies.

   ```
   $ npm install
   ```

3. Add a basic `index.js` file. Feel free to adapt the "Hello, world!" one below.

   ```
   'use strict';

   var express = require('express');

   var app = express();
   app.get('/', function (req, res) {
       res.send('Hello, world!');
   });

   var port = process.env.PORT || 8088;
   app.listen(port, function () {
       console.log('listening on port ' + port);
   });
   ```

   When your application runs on The Platform, your application should bind to the port specified in the `PORT` environment variable. For local development you can leave this unset and provide a default value as in the example above.

4. Run it, then point your browser to `http://127.0.0.1:8088` to see it working.

   ```
   $ npm start
   ```

5. Add a `Procfile` to specify how to start your application. For web servers, you will want to use the `web` target as shown below.

   ```
   web: npm start
   ```

5. Create a new Git repo and add your new files.

```
  $ git init
  Initialized empty Git repository in /some/path/test/.git/
  $ echo node_modules > .gitignore
  $ git add .gitignore index.js package.json Procfile
  $ git commit -m "Initial commit"
  [master (root-commit) f90c88b] Initial commit
  4 files changed, 35 insertions(+)
  create mode 100644 .gitignore
  create mode 100644 Procfile
  create mode 100644 index.js
  create mode 100644 package.json
```

## Building and Deploying the Application

To deploy the application, a Git remote must be set up using [The Platform CLI](https://github.com/daticahealth/cli).

1. Set the git remote of the local project with your provisioned environment's code service.

   ```
   $ datica init
   $ datica -E my-node-app git-remote add app01
   ```

   This will add a new git remote named `datica` to that local repo. It will ask for credentials - these are the email and password that you use in the dashboard.

2. Push master to datica to build your code.

   ```
   $ git push datica master
   ```

3. You should see build output after you push. After pushing, you can check the environment status - the build status should now be finished.

   ```
   $ datica -E "<your_env_name>" status
   environment state: deployed
    app01 (size = c1, build status = finished, deploy status = None)
    mongo01 (size = c1, image = mongodb, status = running)
   ```

## Mongo Integration

Next up we will configure the application to fetch and store data in a Mongo database.

1. If you haven't done so already, go ahead and start up your local MongoDB server, and make sure you can connect to it using the Mongo shell.

2. Modify your `index.js` file to use the `mongodb` driver. The example below extends our "Hello, world!" application to store records in the "visits" MongoDB collection.

   ```
   'use strict';

   var express = require('express'),
       MongoClient = require('mongodb').MongoClient;

   var databaseURL = process.env.DATABASE_URL || 'mongodb://127.0.0.1:27017/demo';

   var app = express();
   app.use(function (req, res, next) {
       MongoClient.connect(databaseURL, function (err, db) {
           if (err) {
               next(err);
               return;
           }
           // insert a new record into the visits collection
           var collection = db.collection('visits');
           collection.insert([{
               message: 'Hello, world!',
               date: new Date()
           }], function (err, result) {
               if (err) {
                   next(err);
                   db.close();
                   return;
               }
               // fetch the latest 10 visits, sorted by date
               var cursor = collection.find().sort({'date': -1}).limit(10);
               // accumulate these and send as JSON when done
               var response = [];
               cursor.each(function (err, doc) {
                   if (err) {
                       next(err);
                       db.close();
                       return;
                   }
                   if (doc == null) {
                       res.json(response);
                       db.close();
                       return;
                   }
                   response.push(doc);
               });
           });
       });
   });

   var port = process.env.PORT || 8088;
   app.listen(port, function () {
       console.log('listening on port ' + port);
   });
   ```

   As with the `PORT` environment variable, you should pull your MongoDB connection information from the `DATABASE_URL` environment variable. If you have more than one database in your environment, use the [dashboard](https://product.datica.com/environments/) to determine which environment variable to use.

3. Run your updated code, then point your browser to `http://127.0.0.1:8088` to see it working.

   ```
   $ npm start
   ```

4. Commit the new changes to your Git repo.

   ```
   $ git add index.js
   $ git commit -m "Added MongoDB integration"
   ```

## Redeploying the Application with MongoDB

All that is required to rebuild a codebase with changes and redeploy is a single push.

```
$ git push datica master
```

No need to talk to Datica - the redeploy will happen automatically if the build is successful.

## Working with Logs

In the Datica dashboard, when you select your environment from the left sidebar, a button is available to access an exposed Kibana + ElasticSearch interface.

![dashboard screenshot](http://cdn2.dropmark.com/45280/65a396d67787a5b0c9fcab9ae61b2687fb3d0d93/logging-button.png)

Docs and guides for ElasticSearch and Kibana can be found [here](https://www.elastic.co/guide/index.html).

## Adding Logging with your Application

Anything your application writes to standard out or standard error will be captured, annotated with timestamp and other information, and forwarded to your ElasticSearch interface.

1. Add a `console.log` call after some important action happens in your application. For the running example, we'll add one just after a new entry is added to the "visits" collection.

   ```
   % git diff
   diff --git a/index.js b/index.js
   index 71ecc89..fb466de 100644
   --- a/index.js
   +++ b/index.js
   @@ -23,6 +23,7 @@ app.use(function (req, res, next) {
                    db.close();
                    return;
                }
   +            console.log('inserted a new record in the visits collection');
                // fetch the latest 10 visits, sorted by date
                var cursor = collection.find().sort({'date': -1}).limit(10);
                // accumulate these and send as JSON when done
   ```

2. Run your application again locally and verify the new logging call is firing properly.

   ```
   $ npm start
   ```

4. Commit the new changes to your Git repo.

   ```
   $ git add index.js
   $ git commit -m "Added logging message"
   ```

## Redeploying with Log Integration

Just another push to redeploy.

```
$ git push datica master
```
