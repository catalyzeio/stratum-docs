---
title: Workers
category: concepts
summary: Learn what Workers are on Stratum.
---

**Workers** on [Stratum](https://catalyze.io/stratum) are applications that are run in their own [container](/stratum/articles/concepts/containers), but do not bind to a port. Workers are typically used for asynchronous background processing.

A [worker job](/stratum/articles/concepts/jobs#worker-jobs) is always associated with a [code service](/stratum/articles/concepts/services#code-services). This means that it runs out of the same codebase as the deploy job, but instead of running the `web` Procfile target, it runs the target it was started with. To start a worker for the very first time (and only the first time), run the [CLI](/stratum/articles/cli-stratum)'s [worker](/paas/paas-cli-reference#worker) command. After it's launched for the first time, it will be [redeployed](/stratum/articles/concepts/services#redeploying) automatically every time the service is redeployed.

Code services by default have an allowed worker count of zero - this means that no workers are allowed to be started. If your original contract did not include workers, but you find that you need some, [contact Catalyze Support](/stratum/articles/contact).

Aside from running a separate Procfile target and not binding to a port, workers function identically to deploy jobs - they have the same access to databases and caches. All logging output will end up in your [Logging Dashboard](/stratum/articles/logging-access). Note, however, that workers run in separate containers from your deploy jobs - if you need to share temporary files, read the [Cloud Storage](/stratum/articles/cloud-storage) article.
