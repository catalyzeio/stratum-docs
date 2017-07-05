---
title: Logging Troubleshooting
category: troubleshooting
---

# Logging Troubleshooting

## Logging is down

**I am getting a 503 when I try to view my logging page pod0XXX.catalyzeapps.com/logging**

- **Potential Issue:** Your logging service may be down or may not have been deployed
- **Potential Solution:** For some sandbox environments, logging services may not have been created. Contact [Datica support](/compliant-cloud/articles/contact/) to create and deploy these services.  Otherwise you can trigger a [redeploy](/compliant-cloud/cli-reference#redeploy) of your logging service by using the Datica CLI command `datica -E "<your_env_name>" redeploy logging`.

**CircuitBreakerException: Data too large**

- **Potential Issue:** This usually occurs when your logging service has accumulated a large amount of data and [Elasticsearch](https://www.elastic.co/products/elasticsearch) is reaching its memory usage cap.
- **Potential Solution:**  Contact [Datica support](/compliant-cloud/articles/contact/) to increase Elasticsearch's memory usage limit or clear Elasticsearch's cache.

## Missing Logs

**I am not seeing any logs for a service in my logging dashboard**

- **Potential Issue:** Logs may not be printing properly to `stdout`. If you are using custom logging, your application may not be configured properly.
- **Potential Solution:** Ensure you are printing log messages to `stdout`.  If you want to use custom logging, ensure it has been set up and configured properly according to [this guide](/compliant-cloud/articles/guides/application-logging/).
