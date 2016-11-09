---
title: Logging Troubleshooting
category: troubleshooting
---

# Logging Troubleshooting

## Logging is down
- **I am getting a 503 when I try to view my logging page pod0XXX.catalyzeapps.com/logging**
	- **Potential Issue:** Your logging service may be down or may not have been deployed
	- **Potential Solution:** For some sandbox environments, logging services may not have been created. Contact [Catalyze support](https://resources.catalyze.io/stratum/articles/contact/) to create and deploy these services.  Otherwise you can trigger a [redeploy](https://resources.catalyze.io/paas/paas-cli-reference/redeploy/#redeploy) of your logging service by using the Catalyze CLI command `catalyze redeploy logging`.

## Missing logs
- **I am not seeing any logs for a service in my logging dashboard**
	- **Potential Issue:** If you have recently added a code service to your environment, your service proxyn may not be updated to reflect this change resulting in logs from your new code service not being forwarded to your code service.
	- **Potential Solution:**  Redeploy your service proxy. To [redeploy](/paas/paas-cli-reference#redeploy) your service proxy via the CLI use the command `catalyze redeploy service_proxy`.