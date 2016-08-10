---
title: Containers
category: concepts
summary: Learn what Containers are and how Stratum uses them.
---

On [Stratum](https://catalyze.io/stratum), your application is contained within an [Environment](/stratum/articles/concepts/environments), which contains a number of [Services](/stratum/articles/concepts/services), which each contain a number of [Jobs](/stratum/articles/concepts/jobs). Each of those jobs is mapped to a **Container** - specifically, a [Docker](https://www.docker.com/) container. Each container is started from a Docker image, which comes from either Catalyze's own set of images (for [databases](/stratum/articles/concepts/services#database-services), [caches](/stratum/articles/concepts/services#caches-services), and automatically-added services) or from images built from your [code pushes](/stratum/articles/concepts/services#code-services).

# Why Containers?

Quoting [Docker's documentation](https://docs.docker.com/engine/understanding-docker/):

> Each container is an isolated and secure application platform.

What this means is that every time a new job for a service is started, it's a fresh setup - no need to worry about temporary files, environment variables, or stuck processes. What this means for Stratum, more importantly, is that there is a uniform way to start, stop, and connect to each piece of an environment, giving an incredible amount of power and options.

# Multiple Hosts

Each Stratum [Pod](/stratum/articles/concepts/pods) has its own set of Docker Hosts, each of which is capable of running a number of containers. When a job is deployed, its container is started on the host best-suited for it - typically the one with the lightest load. Catalyze watches all hosts constantly, adding more to the pod as needed. What this means, however, is that it's very likely that a given environment's containers are spread out among a number of hosts. Every environment has its own network, allowing those containers to find each other - and no one else. That is to say, with the exception of [Service Proxies](/stratum/articles/service-proxies.md) (which have ports 80 and 443 exposed to the outside world), your containers are not accessible except to each other.

# Lifecycle

Once a container is started, it will not be stopped unless it is killed - which will typically only happen when it the service that owns its job is [redeployed](stratum/articles/concepts/services#redeploying).

### See also

* [Services](/stratum/articles/concepts/services)
* [Jobs](/stratum/articles/concepts/jobs)
