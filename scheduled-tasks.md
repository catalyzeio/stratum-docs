---
title: Scheduled Tasks in Stratum
category: manage
summary: Learn about scheduled tasks in Stratum.
---

# Scheduled Tasks in Stratum

Periodic or triggered task scheduling is a common need for many applications. Historically, `cron` has been a system utility widely used for this task.  For an alternative to `cron`, newer applications can take advantage of scheduling frameworks that can provide additional scalability and reliability.

`cron` can be launched as a single worker container for a code service. However, if you rely on scheduled tasks getting executed on-time and successfully, we recommend moving this behavior within the control of your application.

## Scheduling Frameworks

Most widely used languages have some kind of scheduling framework available to easily fit inside of your application. These generally have advantages over cron-like services, such as the ability to scale, improved restart behavior, and failed task retries to improve the guarantee that your tasks will run.

Some of our recommended task scheduling frameworks include Sidekiq for Ruby and Celery for Python. These scheduling frameworks are generally backed by a message broker such as Redis or an AMQP-based alternative like Rabbitmq which helps guarantee message delivery. If your scheduling framework requires a message broker, we have both Redis and Rabbitmq available as environment add-ons.

Take a look at our article on [Stratum Background Workers](/stratum/articles/worker-general/) for more information on using workers with scheduled tasks in Stratum.


## Cron Containers

If adding a scheduling framework isn't an option, then `cron` may work to periodically call commands within your application. To enable `cron` please, follow the steps below.

- Verify you are contracted for a worker process to run the `cron` Procfile target.
- Contact support and request that `cron` be enabled for your worker.
- Create a `crontab` formatted file named `my-crontab` in your application directory that has your commands and schedules.
- Create a `cron.sh` file in your application directory that looks like:

```
#!/bin/sh -e
LOGFILE=/app/cron.log

crontab my-crontab

while true; do
    if [ -f "${LOGFILE}" ]; then
        echo "watching cron job output"
        tail -F "${LOGFILE}"
    fi
    sleep 1
done
```

- Add a target to your Procfile to run `cron.sh`.  An example rails Procfile may look like:

```
web: bundle exec rails server -p $PORT
cron: ./cron.sh
```

- Start your cron worker via the CLI and specify your `cron` Procfile target.

```
catalyze -E "<your_env_alias>" worker deploy <service_name> cron
```
