---
title: Service Proxy
category: concepts
summary: What is a Service Proxy on Compliant Cloud?
---

The **Service Proxy** is a special [service](/compliant-cloud/articles/concepts/services) in each [environment](/compliant-cloud/articles/concepts/environments) that is responsible for routing traffic from the outside world into your environment's network, to specific code services. This is configured using [sites](/compliant-cloud/articles/concepts/sites). The service proxy is the _only_ service exposed outside of your environment's network - all external (non-[console](/compliant-cloud/articles/console), non-[VPN](/compliant-cloud/articles/vpn-stratum)) traffic goes through it and is proxied to other services.

## Client IP Forwarding

Because the traffic is proxied to your code service, the client IP is not directly available. This can be made available in another header, upon request.

## Viewing/Downloading Configurations

Internally, each service proxy uses [NGINX](https://www.nginx.com/), with a configuration file generated for each site. To list these files, use the [CLI](/compliant-cloud/articles/cli-stratum)'s [files list](/compliant-cloud/cli-reference#files-list) command.

```
catalyze -E "<your_env_alias>" files list
```

Then, find the file corresponding to the site you're interested in (named after it - for example, `/etc/nginx/sites-enabled/.example.com`) and download it with the [files download](/compliant-cloud/cli-reference#files-download) command:

```
catalyze -E "<your_env_alias>" files download <filename>
```
