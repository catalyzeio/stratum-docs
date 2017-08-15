---
title: Buildpacks on The Platform
category: buildpack
summary: Learn about buildpacks.
---
# What is a buildpack?
A buildpack is a preconfigured set of available binaries, libraries, and commands used to build and run your application. Buildpacks are extremely flexible and intended to operate as an agent to your environment. The buildpack agent will attempt to understand your stack and install any required dependencies.

In most cases, [The Platform](https://datica.com/platform) detects your needed buildpack automatically, and uses it to build the image that will be used for your [code service](/compliant-cloud/articles/concepts/services#code-services)'s [containers](/compliant-cloud/articles/concepts/containers).

For a list of readily-available buildpacks, [see here](https://github.com/gliderlabs/herokuish/tree/master/buildpacks). Note that The Platform may not match this list exactly.

### See also:
* [Custom Buildpacks](/compliant-cloud/articles/buildpacks-custom)
* [Using a Specific Buildpack Version](/compliant-cloud/articles/buildpacks-pinning)