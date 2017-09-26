---
title: Service Files
category: concepts
summary: Learn what Services Files are on the Platform.
---

**Service Files** are files that customize a [Service](/compliant-cloud/articles/concepts/services/). Some Service Files are created when the Service is created - others are created later. For example, every time a [Site](/compliant-cloud/articles/concepts/sites/) is added, a Service File is added to the [Environment](/compliant-cloud/articles/concepts/environments/)'s [Service Proxy](/compliant-cloud/articles/concepts/service-proxy) to manage the nginx configuration for the site.

Service Files are added to [Containers](/compliant-cloud/articles/concepts/services/containers) when they're created. What this means is that Service File changes aren't immediately propagated to all containers - the Service must be [redeployed](/compliant-cloud/articles/concepts/services/#redeploying) to get the new version.

## Managing

Service Files can be managed in two ways: through [the Platform Dashboard](https://product.datica.com/), or with [the Platform CLI](/compliant-cloud/articles/cli-platform).

To list files with the CLI: [datica files list](/compliant-cloud/cli-reference/#files-list)

To download the contents of a Service File with the CLI: [datica files download](/compliant-cloud/cli-reference/#files-download)
