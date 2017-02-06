---
title: Worker Job Troubleshooting
category: troubleshooting
---

# Worker Job Troubleshooting

## Worker is not running 

**I ran the `catalyze worker deploy <svcName> <target>` command, but my worker is not running**

- **Potential Issue:** The worker target may not be defined properly in your Procfile.
- **Potential Solution:** Define a worker target in your application. Check out the [writing your application](https://resources.datica.com/compliant-cloud/articles/writing-your-application/) guide for more details.

**fatal (3002) Worker Scale Exceeded: Cannot launch a worker job. Environment limit reached. Contact support@datica.com to increase limits**

- **Potential Issue:** You may not have any workers for your subscription or you have reached your worker limit for your subscription.
- **Potential Solution:** If you would like to add workers please contact customer support.  Otherwise you may need to scale back existing workers in order to deploy new workers. You can use the [worker list](https://resources.datica.com/paas/paas-cli-reference/#worker-list) cli command to view your existing and running workers and you can use the [worker scale](https://resources.datica.com/paas/paas-cli-reference/#worker-scale) cli command to scale back running workers.
