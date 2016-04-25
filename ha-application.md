---
title: HA applications
category: application
---

# How does Stratum manage HA applications?

A basic Stratum environment has a single service proxy container routing traffic to a single code service container.

The code service can be made highly available by adding one or more containers to the code service. If your environment contains a code service with multiple containers, Catalyze will add a second service proxy container to make the flow of data fully HA.

AWS will round-robin traffic between the service proxies and re-route traffic if one of the proxies goes down. More importantly, the service proxy containers themselves will route traffic only to available code service containers.

Catalyze can easily scale these Stratum resources to accommodate increased application load as well as achieve high availability.
