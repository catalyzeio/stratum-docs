---
title: Service Files
category: concepts
summary: Learn what Services Files are on the Platform.
---
## Overview
**Service Files** are files that can be used to customize a [Service](/compliant-cloud/articles/concepts/services/). Some Service Files are created when the Service is created — others are created later. For example, every time a [Site](/compliant-cloud/articles/concepts/sites/) is added, a Service File is added to the [Environment](/compliant-cloud/articles/concepts/environments/)'s [Service Proxy](/compliant-cloud/articles/concepts/service-proxy) to manage the NGINX configuration for the site.

[Here's](https://stackoverflow.com/questions/26717013/how-to-edit-nginx-conf-to-increase-file-size-upload) an example of what a customization that a user could make using Service Files.

Service Files can only be created through actions performed on an environment. Users cannot explicitly add Service Files to a service, instead Service Files are added by the following actions:

- When a new service is created
- When a new `site` is added via the Datica CLI

**Please note:** Service Files are added to [Containers](/compliant-cloud/articles/concepts/services/containers) when they're created. What this means is that Service File changes aren't immediately propagated to all containers - **the Service must be [redeployed](/compliant-cloud/articles/concepts/services/#redeploying) to get the new version.**

## Management
Service Files can be managed in two ways:
- 1.) Through [the Platform Dashboard](https://product.datica.com/). To view, download or edit a service navigate to the service in question by clicking "View Environment Details" from the dashboard. Next find the service you need and click "View details". Once you're inside the services details view, select the Files tab from the navigation menu.
- 2.) With [the Platform CLI](/compliant-cloud/articles/cli-platform). To list files with the CLI: [datica files list](/compliant-cloud/cli-reference/#files-list). To download the contents of a Service File with the CLI: [datica files download](/compliant-cloud/cli-reference/#files-download).
