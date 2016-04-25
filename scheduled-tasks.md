---
title: Scheduled Tasks in Stratum
category: manage
---

# Scheduled Tasks in Stratum

Periodic or triggered task scheduling is a common need for many applications. Historically, `cron` has been a system utility widely used for this task. However, if you rely on scheduled tasks getting executed on-time and successfully, we recommend moving this behavior within the control of your application.

## Scheduling Frameworks

Most widely used languages have some kind of scheduling framework available to easily fit inside of your application. These generally have advantages over cron-like services, such as the ability to scale, improved restart behavior, and failed task retries to improve the guarantee that your tasks will run.

Some of our recommended task scheduling frameworks include Sidekiq for Ruby and Celery for Python. These scheduling frameworks are generally backed by a message broker such as Redis or an AMQP-based alternative like Rabbitmq which helps guarantee message delivery. If your scheduling framework requires a message broker, we have both Redis and Rabbitmq available as environment add-ons.

Take a look at our article on ![Stratum Background Workers][background-worker-processing.md] for more information on using workers with scheduled tasks in Stratum.
