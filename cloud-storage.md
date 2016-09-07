---
title: Cloud Storage
category: storage
summary: Learn about using Cloud Storage with your Catalyze Stratum account.
---

# Stratum Cloud Storage

## Temporary In-Container Storage

Every service in your environment has a fixed amount of storage available to it, accessible via the filesystem. Code services can write to the filesystem anywhere they wish (e.g. `/tmp` or `/app/uploads`), but when a service is redeployed (either via build or manually), _any local filesystem changes will be lost_. Do not use the local filesystem for any data you wish to keep. Additionally, local filesystem changes are only available inside a single [container](/stratum/articles/concepts/containers), meaning that [HA](/stratum/articles/ha-application) code services and [workers](/stratum/articles/concepts/workers) will not be able to access them.

## HIPAA-Compliant S3 Buckets

Catalyze can set up an S3 bucket for you, covered under our BAA. Data in S3 is unaffected by redeploys, making it an ideal location to store files or large amounts of data. Read more about how S3 buckets are set up on Stratum [here](/stratum/articles/s3-bucket-access).

If you're interested in adding an S3 bucket to your environment or have questions, [contact us](https://catalyzeio.zendesk.com/hc/en-us/requests/new).
