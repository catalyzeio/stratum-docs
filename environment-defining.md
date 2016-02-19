# What does a Stratum environment look like?

The outlines below provide a high-level overview of Stratum environments. The number of containers and resources available to each container are scalable.

All environments also contain logging and monitoring containers.

## Standard Stratum Environment

At the simplest level, a Stratum environment contains:

- A Code Service (1 code container)
- A Database Service (1 container)
- A Service Proxy (1 container)

Below is a network flow diagram:

## Fully HA Stratum Environment

- HA Code Service (2 code containers)
- HA Database Service (2 code containers)
- HA Service Proxy (2 code containers)

Below is a network flow diagram:

## Expanded HA Stratum Environment

- HA Code Service App01 (2 code containers, 2 worker containers)
- HA Database Service for App01 (2 containers)
- HA Code Service App02 (2 containers)
- HA Database Service App02 (2 containers)
- HA Redis Service (2 Redis Containers, 3 Redis Sentinels)
- HA Service Proxy (2 containers)

Below is a network flow diagram:
