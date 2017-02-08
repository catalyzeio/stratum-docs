---
title: Services
category: concepts
summary: Learn what Services are on Compliant Cloud.
---

**Services** on Compliant Cloud are the pieces of an [Environment](/compliant-cloud/articles/concepts/environments) that define the work to be done. Specifically, a service defines how [Jobs](/compliant-cloud/articles/concepts/jobs) should be started out of them. This includes:

* The image and size of the [containers](/compliant-cloud/articles/concepts/containers) that `deploy` and `worker` jobs run.
* [Variables](/compliant-cloud/articles/environment-variables), both automatic and user-set.
* The number of jobs that should be started when the service is redeployed (see the [High Availability](/compliant-cloud/articles/ha-application) article).

# Types of Services

## Code Services

**Code** services start jobs using the images built when you push code. Code services are, essentially, your actual application. For related information, read the [Writing Your Application](/compliant-cloud/articles/writing-your-application) article.

## Database Services

**Database** services start jobs whose containers contain the specified database runtime and version, and, depending on configuration, contain the data stored by that database as well.

Database service types include but are not limited to **PostgreSQL**, **MySQL**, and **MongoDB**.

## Cache Services

**Cache** services start jobs whose containers contain the specified cache runtime and version.

Cache service types include but are not limited to **Redis** and **memcached**.

## Service Proxies

**Service Proxies** are a special service that route traffic from the internet into your environment's services, according on how your environment's [sites](/compliant-cloud/articles/concepts/sites) are configured. For more information on service proxies, read [the Service Proxy article](/compliant-cloud/articles/concepts/service-proxy).

## Logging Services

A **Logging Service** is included in every environment. Every other container in the environment forwards its logs to this service's container, and this container is responsible for serving the logging dashboard. For more information on how logging works, read [the Logging article](/compliant-cloud/articles/logging-access).

Logging services cannot be redeployed via the CLI.

## Monitoring Services

A **Monitoring Service** is included in every non-sandbox environment. This container runs a set of checks periodically against the other services in your environment. For more information on monitoring, read [the Monitoring article](/compliant-cloud/articles/monitoring).

Monitoring Services cannot be redeployed via the CLI.

# Deploying

When a service is **deployed**, a number of [deploy jobs](/compliant-cloud/articles/concepts/jobs#deploy-jobs) are started for it. The number of jobs is set by the `scale` attribute of the service. Database, cache, and automatically-included services (logging, monitoring, and service proxy) are all deployed by default, but code services are not deployed until the first [code push](/compliant-cloud/articles/code-deployment). For code services, [worker jobs](/compliant-cloud/articles/concepts/jobs#worker-jobs) aren't deployed until the [worker command](/compliant-cloud/cli-reference#worker) is run.

# Redeploying

**Redeploying** a service means to stop all existing deploy and worker jobs for it, and start new jobs based on its current spec. This is how updates to images,  [variables](/compliant-cloud/articles/environment-variables), [sites](/compliant-cloud/articles/concepts/sites), and other configurations take effect. A redeploy can happen in one of three ways:

1. Manually, via the [redeploy command](/compliant-cloud/cli-reference#redeploy).
2. Automatically, when new code is pushed to a code service.
3. Via a [support ticket](/compliant-cloud/articles/contact). This is only needed if that service has been marked as not redeployable, meaning that it has a customization applied to it that its [Pod](/compliant-cloud/articles/concepts/pods) cannot automatically regenerate.

> **Note**: Redeploying a service will result in short downtime while the new container and the applications within it start up. This time is typically just a few seconds, but some applications can take a considerably longer time to start or become responsive. If your code service has an [HA](/compliant-cloud/articles/ha-application) configuration, we can configure your service to have a specific **Overlap Period** - meaning that in a redeploy, old jobs will wait for a period of time before being killed, instead of being killed right away. This is not enabled by default, to minimize confusion and unexpected behavior - if you'd like to set this up, send us a [support ticket](/compliant-cloud/articles/contact).

### See also

* [Environments](/compliant-cloud/articles/concepts/environments)
* [Jobs](/compliant-cloud/articles/concepts/jobs)
