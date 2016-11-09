---
title: Application Deployment Troubleshooting
category: troubleshooting
---

# Application Deployment Troubleshooting

## Unable to view application
- **I can't see my application after I have deployed to Catalyze successfully**
	- **Potential Issue:** You may be exposing your app to a port other than 8080.
	- **Potential Solution:** Try changing the port your app is exposed on in your Procfile. Check out the [writing your application](https://resources.catalyze.io/stratum/articles/writing-your-application/) guide for more details. For or security purposes we prefer customers to expose their app on port 8080.  If you wish to expose you app on another port please [contact Catalyze support](https://resources.catalyze.io/stratum/articles/contact/).
	
- **404: Page Not Found NGINX error**
    - **Potential Issue:** You have not configured an SSL Certificate or Sites object with that services in the environment.
    - **Potential Solution:** Configure both a site an a cert with your environment. See this [guide on certs](/stratum/articles/guides/self-service-SSL/) and this [guide on sites](/stratum/articles/initial-setup/#sites-setup) for more details.

## Broken Build
- **I triggered a build that is broken**
	- **Potential Issue:** The build may have completed successfully, but your application may be broken or not functioning properly.
	- **Potential Solution:** You can check your logging dashboard to view application logs which may be useful for debugging your application. You can use the [rollback](https://resources.catalyze.io/paas/paas-cli-reference/#rollback) command in the CLI to roll back your application to a previous working version while you are debugging your application.  If you continue having issues with your application you can contact.

## Zero Downtime Deployments
- **I want to deploy my app with no downtime**
	- **Potential Issue:** You may not have deploy overlaps configured for your environment.  
	- **Potential Solution:** Deploy overlaps set a duration of time for old jobs to persist while new build and deploy jobs start running.  To configure deploy overlaps contact Catalyze customer support.  If you still experience downtime after configuring deploy overlaps, there may be additional configuration options that can reduce or eliminate downtime.

## New code service, same domain name
- **I have deployed a build to a new code service but I can not see my application**
	- **Potential Issue:** Your service proxy has not been updated to expose your new code service 
	- **Potential Solution:** You need to [create a new site](https://resources.catalyze.io/paas/paas-cli-reference/#sites-create) that references your new code service.  If you want a more custom configuration for exposing your new code service such as exposing it on a specific route, you may need to contact Catalyze support.


