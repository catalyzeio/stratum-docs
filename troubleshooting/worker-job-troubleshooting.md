---
title: Worker Job Troubleshooting
category: troubleshooting
---

# Worker Job Troubleshooting

## My worker is not running 
- *I have run the `catalyze worker worker` command, but my worker is not running*
	- **Potential Issue:** The worker target may not be defined properly in your Procfile.
	- **Potential Solution:** Define a worker target in your application. Check out the [writing your application](https://resources.catalyze.io/stratum/articles/writing-your-application/) guide for more details.
	
- *fatal (3002) Worker Scale Exceeded: Cannot launch a worker job. Environment limit reached. Contact support@catalyze.io to increase limits*
	- **Potential Issue:** You may not have any workers for your subscription or you have reached your woker limit for your subscription.
	- **Potential Solution:** If you would like to add workers please contact customer support.  Otherwise you may need to scale back existing workers in order to deploy new workers. You can use the [worker list](https://resources.catalyze.io/paas/paas-cli-reference/#worker-list) cli command to view your existing and running workers and you can use the [woker scale](https://resources.catalyze.io/paas/paas-cli-reference/#worker-scale) cli command to scale back running workers.
