---
title: Compliant Cloud Initial Setup
category: getting-started
Summary: Brand new to Compliant Cloud? Start here!
---

This article is intended to get you up and running on your new [Compliant Cloud](https://datica.com/compliant-cloud) [environment](/compliant-cloud/articles/concepts/environments). If you'd like to read more, please look through the rest of the [Getting Started guides](/compliant-cloud/getting-started/).

If you don't yet have an environment, or you have questions on Compliant Cloud's capabilities and offerings, [please get in touch](/compliant-cloud/articles/contact).

## 1. Access

Once your new environment has been provisioned and is ready to use, you will receive two emails. The first is simply a notification that it's ready, which you will receive for every environment. This contains your environment's **public hostname** - take note of this for later. The second is an invitation to join the new **organization** that has been created for you - this will only be sent for the very first environment that we provision for you. The invitation email contains a link to the [Compliant Cloud dashboard](https://product.datica.com/compliant-cloud) - click it!

The Compliant Cloud dashboard will prompt you to sign in with your Datica account - if you don't have one already, you can also create one at this point - just follow the instructions on the page. After signing in (or creating a new account, verifying your email address, then signing in), click the "Compliant Cloud" link from the product list. You should see your organization in the upper-right corner:

<center>![org corner](/compliant-cloud/articles/images/org_corner.png)</center>

For more on managing organizations, including how to add additional members and grant them access to your environment, see the [Managing Organizations](/compliant-cloud/articles/concepts/organizations) article.

> ***Note:*** Occasionally, if interrupted while creating an account, you might find that your invite did not get accepted, and you are not a member of your organization yet. If this happens, just click the link in your email again.

Once you've confirmed that you're a member of your organization, you should also see your environment's summary listed in the Compliant Cloud dashboard, looking something like this:

<center>![org summary](/compliant-cloud/articles/images/env_summary.png)</center>

From the summary, note your environment's name and your code service's name - in the above image, those are "MyEnvironment-production" and "app01", respectively.

## 2. Install the Datica CLI

Datica provides a CLI (command-line interface) tool to facilitate interaction with your Compliant Cloud environment, supporting Windows, OSX, and Linux.

To install the CLI, follow the instructions in its [Github repository](https://github.com/daticahealth/cli).

## 3. Add Your Public Key

In order to push code, Datica needs to have a public key attached to your account. If you don't have one yet, you can create one via the following on OSX/Linux:

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

To add your key to your account, use the `keys add` command, which has the form `datica -E "<your_env_alias>" keys add <key name> <path to key>`. For example, if you want to call your key "my-datica-key" and the path to it is `~/.ssh/id_rsa.pub`, the command you would run would be:

```
datica keys add my-datica-key ~/.ssh/id_rsa.pub
```

This will prompt you to sign in with your Datica account. You can then validate that the key was added using `datica -E "<your_env_alias>" keys list`.

## 4. Associate to Your Environment

> ***Note:*** If your environment has more than one code service, this step and all steps after will need to be repeated for each one.

In order for the CLI to know which environment it should be interacting with (and, coincidentally, who you are), you first need to **associate** your local code repo to your new environment. "Associating" does the following:

1. Find the internal identifiers for your environment and code service, and cache those locally.
2. Set the `datica` [Git remote](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes).

The form of this command in the CLI is: `datica associate <environment name> <code service name>`. For the example from Step 1, that would be `datica associate MyEnvironment-production app01`.

For options for the `associate` command, see the [CLI reference](/compliant-cloud/cli-reference#associate).

## 5. Upload Your SSL Certificate

Acquiring SSL certs is a very complex topic - if you'd like to read more information than what is outlined below, please see the [SSL Certificates](/compliant-cloud/articles/guides/self-service-SSL/) article. The certificate and private key must be unencrypted and in PEM format.

> ***Note:*** This can be a wildcard cert. Wildcard certs can be reused.

The CLI command to upload a cert is `certs create`, taking the form `datica -E "<your_env_alias>" certs create <cert name> <path to crt file> <path to key file>`. For example:

```
datica -E "<your_env_alias>" certs create example.com example.com.crt example.com.key
```

If that cert is self-signed, pass the `-s` option:

```
datica -E "<your_env_alias>" certs create example.com example.com.crt example.com.key -s
```

> ***Note:*** Using a self-signed cert can be very useful for development or staging environments.

For wildcard certs, the typical nomenclature is `*.domain.tld`:

```
datica -E "<your_env_alias>" certs create *.example.com wildcard-example.com.crt wildcard-example.com.key
```

## 6. Set Your DNS

Because an environment can have any number of code services, the public hostname for the environment does not point to any of them. What this means is that, in order to access each code service in your application, you will need to set up DNS that will forward to it. This step is executed entirely outside of Compliant Cloud - Datica cannot do any step of this for you. Datica is not a DNS provider.

First, choose the hostname you would like to use for the code service - this can be either an apex domain (such as `example.com`) or a subdomain (such as `api.example.com`).

Then, in your DNS provider's control panel, set up a `CNAME` from that hostname to your environment's public hostname (`ALIAS` can be used if `CNAME` is not supported by your provider). We recommend setting a TTL of 300s.

To verify that your DNS change has propagated (which typically takes a few minutes), use `nslookup`:

```
$ nslookup api.example.com
Non-authoritative answer:
api.example.com canonical name = pod0A1B2C3.catalyzeapps.com.
```

> ***Note:*** Some DNS providers may not allow `CNAME` _or_ `ALIAS` records for apex domains. If you discover that your host has this limitation and using a subdomain is not an option, we recommend transferring your domain over to [Cloudflare](https://www.cloudflare.com/).

## 7. Set Up a Site

Compliant Cloud uses what we call **[Sites](/compliant-cloud/articles/concepts/sites)** to map code services to hostnames, using the cert that was uploaded in step 5.

The CLI command to create a cert is `sites create`, taking the form `datica -E "<your_env_alias>" sites create <hostname> <code service name> <cert name>`. For example, using the hostname from step 6, the wildcard cert name from step 5, and the code service name noted in step 1:

```
datica -E "<your_env_alias>" sites create api.example.com app01 *.example.com
```

This will generate a new nginx configuration file for the new site.

## 8. Redeploy the Service Proxy

In order to pick up on the new site file, your environment's [Service Proxy](/compliant-cloud/articles/concepts/service-proxy) needs be redeployed. This is done via the `redeploy` command:

```
datica -E "<your_env_alias>" redeploy service_proxy
```

After a short period of downtime (usually 20-40 seconds), your service proxy will be responding again. If your site, certs, and DNS are set up correctly, navigating to the hostname in the site you just configured (`api.example.com` in the example above) should result in a 503 error.

Once you have an SSL certificate added to an environment and the DNS name you want to resolve pointed at your POD URL, you'll need to create a site for the environment that uses the certificate and listens for that DNS name. Until you create a site, you will **not** be able to route traffic to your application.

You can verify that your certificate is correctly being used with `openssl`:

`openssl s_client -connect api.example.com:443`

## 9. Push Code

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

> ***Note:*** If your application builds successfully but is not behaving as expected, read through the [Writing Your Application to Work With Compliant Cloud](/compliant-cloud/articles/writing-your-application) article.

If your application includes [workers](/compliant-cloud/articles/concepts/workers), use the `worker` command to start them (you only need to do this the first time):

```
datica -E "<your_env_alias>" worker deploy <service_name> <target>
```

Where `target` is the name of the Procfile target to be run (typically "worker").

### See also

* [Getting Started Guides](/compliant-cloud/getting-started/)
