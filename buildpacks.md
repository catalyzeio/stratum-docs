---
title: Buildpacks on Stratum
category: buildpack
summary: Learn about buildpacks.
---

A buildpack is a preconfigured set of available binaries, libraries, and commands used to build and run your application. Buildpacks are extremely flexible and intended to operate as an agent to your environment. The buildpack agent will attempt to understand your stack and install any required dependencies.

In most cases, [Stratum](https://catalyze.io/stratum) detects your needed buildpack automatically, and uses it to build the image that will be used for your [code service](/stratum/articles/concepts/services#code-services)'s [containers](/stratum/articles/concepts/containers).

For a list of readily-available buildpacks, [see here](https://github.com/gliderlabs/herokuish/tree/master/buildpacks). Note that Stratum may not match this list exactly.

### See also:

* [Custom Buildpacks](/stratum/articles/buildpacks-custom)
* [Using a Specific Buildpack Version](/stratum/articles/buildpacks-pinning)
