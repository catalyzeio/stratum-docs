---
title: The Platform Initial Setup
category: getting-started
Summary: Brand new to The Platform? Start here!
---

**Updated for the 3.0 release.**
 
This article is intended to get you up and running with your new application on the [Datica Platform](https://datica.com/platform). We assume no prior experience with either Datica or any other Platform as a Service provider. If you're looking for further information on the topics discussed in this article please see our listing of Platform concepts located [here](/compliant-cloud/concepts).
 
## Let’s get started!

This article is a journey, and this journey has two paths.
 
**1.)** The first path is intended for customers signing up through the Datica.com website on either the Developer or Growth tier plans as outlined on our [pricing page](https://datica.com/pricing). If you've signed up for Datica via this method after August 15th, 2017 then [visit the updated on-boarding guide](/compliant-cloud/articles/on-boarding).

**2.)** The second path is intended for existing Datica customers, or users that are being invited to an environment that is not self-service, or was created previous to August 15th, 2017. For those customers please keep reading below.

## 1. Gaining Access
Once your new environment has been provisioned and is ready to use, you will receive two emails:

**The first** is simply a notification that it's ready, which you will receive for every environment. This contains your environment's **public hostname** - take note of this for later. (once you have access to your account you can find your hostname on the `service_proxy` details view).

**The second** is an invitation to join the new [**organization**](/compliant-cloud/articles/concepts/organizations/) that has been created for you - this will only be sent for the very first environment that we provision for you. The invitation email contains a link to [The Platform dashboard](https://product.datica.com/environments) - click it!
 
The Platform dashboard will prompt you to sign in with your Datica account - if you don't have one already, you can also create one at this point - just follow the instructions on the page. After signing in (or creating a new account, verifying your email address, then signing in) you should see your organization under the “Organization” section in the left hand navigation bar.
 
![org corner](/compliant-cloud/articles/images/org_corner.png)
 
For more on managing organizations, including how to add additional members and grant them access to your environment(s), see the [Managing Organizations](/compliant-cloud/articles/concepts/organizations) article.
 
> ***Note:*** Occasionally, if interrupted while creating an account, you might find that your invite did not get accepted, and you are not a member of your organization yet. If this happens, just click the link in your email again.
 
Once you've confirmed that you're a member of your organization, you should also see your environment's summary listed in the Platform dashboard, looking something like this:
 
![org summary](/compliant-cloud/articles/images/env_summary.png)
 
From the summary, note your environment's name and your code service's name - in the above image, those are "Customer Portal - Prod" and "code-1", respectively.
 
## 2. Install the Datica CLI
Datica provides a CLI (command-line interface) tool to facilitate interaction with your Platform environment, supporting Windows, OSX, and Linux.
 
To install the CLI, follow the instructions here on [Github](https://github.com/daticahealth/cli).
 
## 3. Initialize Your Code
> ***Note:*** If your environment has more than one code service, you will have to use this command or [`datica git-remote add`](/compliant-cloud/cli-reference#git-remote-add) for for each one.
 
The `init` command does the following:
 
1. Signs you into the Datica Platform.
2. Ensures you have an ssh-key associated to your account.
3. Sets the `datica` [Git remote](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes) of your code service, so that you can push to it.
 
The form of this command in the CLI is: `datica init`. The init command will find all the environments you have access to and give you a prompt to pick one that it will use to find a code service to initialize a git remote for you.

## 4. Upload Your SSL Certificate

Datica provides two methods for installing SSL certificates. The first one is via our Let’s Encrypt feature. This new feature allows customers to easily install Let’s Encrypt SSL certificates with a few commands. The second method is the “bring your own SSL certificate” method.

**The Let’s Encrypt method**
We have an entire guide dedicated to Let's Encrypt. [Have a look](/compliant-cloud/articles/guides/lets-encrypt).

**Bring your own SSL certificate method**
Acquiring SSL certs is a very complex topic - if you'd like to read more information than what is outlined below, please see the [SSL Certificates](/compliant-cloud/articles/guides/self-service-SSL/) article. The certificate and private key must be unencrypted and in PEM format.
 
> ***Note:*** This can be a wildcard cert. Wildcard certs can be reused.
 
The CLI command to upload a cert is `certs create`, taking the form `datica -E "<your_env_name>" certs create <cert name> <path to crt file> <path to key file>`. For example:
 
```
datica -E "<your_env_name>" certs create example.com example.com.crt example.com.key
```
 
If that cert is self-signed, pass the `-s` option:
 
```
datica -E "<your_env_name>" certs create example.com example.com.crt example.com.key -s
```
 
> ***Note:*** Using a self-signed cert can be very useful for development or staging environments.
 
For wildcard certs, the typical nomenclature is `*.domain.tld`:
 
```
datica -E "<your_env_name>" certs create *.example.com wildcard-example.com.crt wildcard-example.com.key
```
 
## 5. Set Your DNS 
Because an environment can have any number of code services, the public hostname for the environment does not point to any of them. What this means is that, in order to access each code service in your application, you will need to set up DNS that will forward to it. This step is executed entirely outside of The Platform - Datica cannot do any of this for you. Datica is not a DNS provider.
 
First, choose the hostname you would like to use for the code service - this can be either an apex domain (such as `example.com`) or a subdomain (such as `api.example.com`).
 
Then, in your DNS provider's control panel, set up a `CNAME` from that hostname to your environment's public hostname (`ALIAS` can be used if `CNAME` is not supported by your provider). We recommend setting a TTL of 300s.
 
To verify that your DNS change has propagated (which typically takes a few minutes), use `nslookup`:
 
```
$ nslookup api.example.com
Non-authoritative answer:
api.example.com canonical name = pod0A1B2C3.catalyzeapps.com.
```
 
> ***Note:*** Some DNS providers may not allow `CNAME` _or_ `ALIAS` records for apex domains. If you discover that your host has this limitation and using a subdomain is not an option, we recommend transferring your domain over to [Cloudflare](https://www.cloudflare.com/).
 
## 6. Set Up a Site
The Platform uses what we call **[Sites](/compliant-cloud/articles/concepts/sites)** to map code services to hostnames, using the cert that was uploaded in step 5.
 
The CLI command to create a cert is `sites create`, taking the form `datica -E "<your_env_name>" sites create <hostname> <code service name> <cert name>`. For example, using the hostname from step 6, the wildcard cert name from step 5, and the code service name noted in step 1:
 
```
datica -E "<your_env_name>" sites create api.example.com app01 *.example.com
```
 
This will generate a new nginx configuration file for the new site.
 
## 7. Redeploy the Service Proxy
In order to pick up on the new site file, your environment's [Service Proxy](/compliant-cloud/articles/concepts/service-proxy) needs be redeployed. This is done via the `redeploy` command:
 
```
datica -E "<your_env_name>" redeploy service_proxy
```
 
After a short period of downtime (usually 20-40 seconds), your service proxy will be responding again. If your site, certs, and DNS are set up correctly, navigating to the hostname in the site you just configured (`api.example.com` in the example above) should result in a 503 error.
 
Once you have an SSL certificate added to an environment and the DNS name you want to resolve pointed at your POD URL, you'll need to create a site for the environment that uses the certificate and listens for that DNS name. Until you create a site, you will **not** be able to route traffic to your application.
 
You can verify that your certificate is correctly being used with `openssl`:
 
`openssl s_client -connect api.example.com:443`
 
## 8. Pushing Code
In order to build an image for your code service's [container](/compliant-cloud/articles/concepts/containers) to run, you do a `git push` to the `datica` remote, pushing the `master` branch:
 
```
git push datica master
```
 
> ***Note:*** The pushed branch **must** be `master`. If the branch you want to push is not `master`, use the following:
>
> ```
git push datica mybranchname:master
```
 
After your push, you will see build output stream to your terminal window. Any build error output will be there. For related information, read the [Writing Your Application](/compliant-cloud/articles/writing-your-application) article.
 
After your build succeeds, a [deploy job](/compliant-cloud/articles/concepts/jobs#deploy-jobs) will be started for it. Shortly after (20-40 seconds, plus however long it takes your application to start up and respond). After that, your application should be available at the hostname you configured in the site. Congratulations!
 
> ***Note:*** If your application builds successfully but is not behaving as expected, read through the [Writing Your Application to Work With The Platform](/compliant-cloud/articles/writing-your-application) article.
 
If your application includes [workers](/compliant-cloud/articles/concepts/workers), use the `worker` command to start them (you only need to do this the first time):
 
```
datica -E "<your_env_name>" worker deploy <service_name> <target>
```
 
Where `target` is the name of the Procfile target to be run (typically "worker").
 
### See also
* [Getting Started Guides](/compliant-cloud/getting-started/)
