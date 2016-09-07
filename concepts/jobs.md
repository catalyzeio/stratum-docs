---
title: Jobs
category: concepts
summary: Learn what Jobs are on Stratum.
---

**Jobs** on [Stratum](https://catalyze.io/stratum) are what actually run your applications, databases, caches, and everything else that makes up an [environment](/stratum/articles/concepts/environments). Jobs are a one-to-one mapping to [containers](/stratum/articles/concepts/containers) - every running job has exactly one container. What that job's container is running is defined by the [service](/stratum/articles/concepts/jobs). All jobs belong to a service, but a service can have more than one job running.

# Types of Jobs

## Deploy Jobs

**Deploy** jobs are the most common job type. These are the jobs that run a container indefinitely. See the list of [service types](/stratum/articles/concepts/services#types-of-services) for details on what each type's containers run.

## Build Jobs

**Build** jobs are the jobs that build the code you push, creating a new image and [redeploying the service](/stratum/articles/concepts/services#redeploying) if the build is successful.

## Worker Jobs

**Worker** jobs are a special job type that are unique to code services. Worker jobs are functionally similar to [deploy jobs](#deploy-jobs), but execute a different [procfile](/stratum/articles/writing-your-application#choosing-how-your-application-is-run) target. Read more in the [Workers](/stratum/articles/concepts/workers) article.

## Backup and Restore Jobs

**Backup** and **restore** jobs correspond to the database backups and restore, initiated via either [CLI commands](/paas/paas-cli-reference#db) or automated  nightly database backups. When the requested task has been completed, the jobs will terminate.

## Console Jobs

**Console** jobs represent an open [secure console](/stratum/articles/console) session. In the event of an unclean exit, a console job will eventually get cleaned up and stopped.

## Rake Jobs

**Rake** jobs represent a running [rake](https://github.com/ruby/rake) task, and will terminate when finished.

# Job States

The following are common job states that you might see.

* `scheduled` - A host has not yet been found to run the job on.
* `running` - The job is in progress.
* `finished` - The job is complete.

### See also

* [Services](/stratum/articles/concepts/services)
* [Containers](/stratum/articles/concepts/containers)
