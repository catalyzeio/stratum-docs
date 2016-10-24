---
title: Service Communication
---

# Service Communication

## How can my code services, databases, and caches communicate over my private, encrypted network?

Catalyze sets up private DNS records for all of your services that are accessible only to your private, encrypted network. These DNS records end in `.internal` and are available for you to use to communicate between services. The most common form of these DNS records are found in database URL environment variables (i.e. `postgres://username:password@postgresql-1234567890.internal:5432/catalyzeDB`). You can look up all of the DNS names for your services by using the CLI and running [catalyze services list](//paas/paas-cli-reference/#services-list). This is available since version 3.3.0 of the CLI.

## What are the `.internal` addresses in my database URL environment variables?

Database URLs are made up of multiple parts and may include some, or all of, the hostname, username, password, and port number. The hostname is the piece that ends in `.internal` and points to a private DNS record available only to your services. Using these `.internal` addresses means your network traffic never leaves your private, encrypted network. You can find out which database services belong to which DNS names by running the [catalyze services list](//paas/paas-cli-reference/#services-list) command. This is available since version 3.3.0 of the CLI.

## How do private DNS records work for HA services?

When you have an [HA](//stratum/articles/ha-application/) service (multiple jobs running for one service), your network traffic will round robin to all services associated with the given private DNS name.
