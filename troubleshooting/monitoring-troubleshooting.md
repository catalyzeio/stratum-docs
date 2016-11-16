---
title: Monitoring Troubleshooting
category: troubleshooting
---

# Monitoring Troubleshooting

## Monitoring is down
- **I am getting a 503 when I try to view my monitoring page pod0XXX.catalyzeapps.com/monitoring**
	- **Potential Issue:** Your monitoring service may be down or may not have been deployed
	- **Potential Solution:** For some sandbox environments, monitoring services may not have been created. Contact [Catalyze support](https://resources.catalyze.io/stratum/articles/contact/) to create and deploy these services.  Otherwise you can trigger a [redeploy](https://resources.catalyze.io/paas/paas-cli-reference/redeploy/#redeploy) of your monitoring service by using the Catalyze CLI command `catalyze redeploy monitoring`.

## Custom Alerts
- **I want to be alerted when a certain event happen**
	- **Potential Issue:** You do not have any custom [Sensu](https://sensuapp.org/) alerts for your environment  
	- **Potential Solution:** Contact [Catalyze support](https://resources.catalyze.io/stratum/articles/contact/) to request to have custom alerts added to your environment. There are many [sensu plugins](https://sensuapp.org/plugins) that provide a wide variety of functionalities and checks that can be added to your monitoring service.

## Alerting
- **One of my services is down and I did not receive a notification**
	- **Potential Issue:** Your monitoring service may not be configured to notify you.
	- **Potential Solution:** Monitoring service can be configured to use a variety of means to contact personnel when an appropriate events occurs. You can be contacted via Slack, email or text. Contact [Catalyze support](https://resources.catalyze.io/stratum/articles/contact/) to set up monitoring notifications.