---
title: Pods
category: concepts
summary: Learn what Pods are to Compliant Cloud.
---

[Compliant Cloud](https://datica.com/compliant-cloud) places each environment in what we call a **Pod**. A Pod consists of the runtimes, hosts, and internal applications needed to deploy, control, and monitor the environments they contain. Between pods, features can vary (in some cases, we roll new features out to sandbox pods before others, and in other cases, older pods cannot support some features), but the [environment](/compliant-cloud/articles/concepts/environments) model is the same.

Typically, users will not need to worry about which pod their environment is on, and will never need to interact with it directly. Occasionally, maintenance will affect only environments in a specific pod - when this happens, that pod will be called out. To determine which pod your environment is in, look in the public hostname. As an example:

<span style="color: blue">pod01A1B2C3</span>.catalyzeapps.com

The blue portion, "pod01A1B2C3", is the environment's **namespace** - the first portion of that, "pod01", indicates that the environment is located in the pod named **pod01**.
