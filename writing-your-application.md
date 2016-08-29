---
title: Writing Your Application
category: getting-started
summary: Writing your application to work with Stratum.
---

## Choosing how your code is built

All builds on [Stratum](https://catalyze.io/stratum) are done using [Buildpacks](/stratum/articles/buildpacks). Each buildpack has its own conditions for autodetection - make sure that your application's file structure allows this to happen.

Sometimes, the wrong buildpack can be chosen if multiple buildpacks' conditions are met. For example, having both a `package.json` and a `requirements.txt` in the root of your repository would match both the [Node.js](https://github.com/heroku/heroku-buildpack-nodejs) and [python](https://github.com/heroku/heroku-buildpack-python) buildpacks, and the result might not be the correct one. If this occurs, you can [pin the correct buildpack](/stratum/articles/buildpacks-pinning). Note, also, that you can create a [custom buildpack](/stratum/articles/buildpacks-custom) if needed.

## Choosing how your application is run

Stratum uses a special file named `Procfile` (no extension) to designate what commands to run to start your application. A Procfile consists of one or more lines, structured like this:

```
<target>: <command>
```

For the main application for your code service, the target is always `web`. For [workers](/stratum/articles/concepts/workers), the target can be named differently - "worker" is typical, but it's up to you.

For a simple python application, your Procfile might look like this:

```
web: python main.py
```

For a python application with a worker named "myworker", it might look like this:

```
web: python main.py
myworker: python worker.py
```

## Binding to PORT

In order to expose your application to the [Service Proxy](/stratum/articles/concepts/service-proxy), and thus accessible the outside world, it needs to bind to a very specific port, stored in the environment variable named `PORT`. If this port is not bound to, your application will continue to run, but it will not be accessible.

> ***Note:*** Do not change the value for `PORT`, or hard code that value in your application. While rare, it is subject to change.

## Database and Cache Connection Strings

The strings needed to connect your application to any databases and caches in your environment are made available in [variables](/stratum/articles/environment-variables) to that code service, created when those services are provisioned. The names are based on the name of the service - for example, if your environment has a PostgreSQL database service named "database-1" and a Redis cache named "cache-1", the corresponding connection strings are likely stored in variables named `DATABASE_1_URL` and `CACHE_1_URL`, respectively.

Some clients require these strings to be in different forms than provided, and some clients only take the parameters separately. Feel free to create additional variables as needed to make this easier - it's your environment.

## Filesystem Structure and Storage

Applications and any build artifacts are always placed at `/app` inside each container. This means that, for example, a file named `README.md` in the root of your repository will be located at `/app/README.md` inside your code service's container(s).

Your application has total read/write access on the local filesystem. If temp space is needed for uploads, `/tmp` is a good choice. Note, however, that any local filesystem changes are lost on redeploy, and local filesystem changes are not visible from one job to another. For more information and solutions, see the [Cloud Storage](/stratum/articles/cloud-storage) article.
