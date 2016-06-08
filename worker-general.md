---
title: Background Processing and Workers in Stratum
category: worker
summary: Learn about background processing and workers in Stratum.
---

# Background Processing and Workers in Stratum

Background processing includes any operations performed outside of the main application. These operations are typically long running tasks that can be more resource intensive. You don't want these operations to detract from the performance of your application and have your customers' experience damaged, so the logical choice is to separate them.

## Stratum Background Processing

Catalyze has designed a feature to help you easily deploy background "worker" processes that exist within your application's code base. Workers are long running processes that will run along side of your application. Workers are ***not*** tasks designed to be run once (rake tasks, database migrations, etc). A couple common frameworks for background processing include [Sidekiq (Ruby)](http://sidekiq.org/) and [Celery (Python)](http://www.celeryproject.org/). These frameworks are convenient because they allow you to develop and maintain your background processing along with your application code in one repository. These frameworks typically operate asynchronously by passing messages via a broker or queue (RabbitMQ and Redis are popular options).

## Stratum Worker Containers
Worker containers are clones of an application container. They are built and run with the same Git repository that an application container uses. They have access to the same resources as the parent application.

Right now, worker containers mirror the resource sizing of their parent applications.
