---
title: Self-Service SSL Guide
category: ssl
---

# Self-Service SSL Guide

With our latest release of the Stratum CLI, we have implemented a whole slew of features and bug fixes. Perhaps the most intriguing pieces of functionality are two sets of commands: the [certs](https://resources.catalyze.io/paas/cli/sections/certs/) and [sites](https://resources.catalyze.io/paas/cli/sections/sites/) commands. These two sets of commands will allow self-service SSL management. The following sections will explain how to use these two sets of commands to their fullest extent and manage your SSL certificates with ease and on your own schedule.

## Certs

A cert represents a reusable component composed of an SSL certificate chain and a private key. These are securely uploaded and stored on the Catalyze platform and become available to your entire environment.  The CLI should always be used to send certificates to Stratum.  NEVER send certificates or private keys via email or Zendesk to Catalyze support.

The [certs](https://resources.catalyze.io/paas/cli/sections/certs/) command group in the CLI has four subcommands to manage your SSL certificates:

- [certs create](/paas/paas-cli-reference/certs-create/)
- [certs list](/paas/paas-cli-reference/certs-list/)
- [certs rm](/paas/paas-cli-reference/certs-rm/)
- [certs update](/paas/paas-cli-reference/certs-update/)

### Create a Cert

To start, you'll need to create a cert. Make sure both the certificate and private key are **unencrypted** and in **PEM** format. Then run

***CA-Signed SSL Cert***
```
catalyze certs create wildcard_examplecom ./example.crt ./example.key
```

***Self-Signed SSL Cert***
```
catalyze certs create examplecom ./example_selfsigned.crt ./example_selfsigned.key -s
```

Note that the cert hostname does not need to correspond to the actual domain that the SSL certificate is valid for. The cert hostname is used for organizational purposes only. A descriptive name indicating the domain name, TLD, and whether or not it is a wildcard is recommended.

#### Cert Creation Behind-The-Scenes

This command will do a lot of work behind the scenes! First, the certificate and private key are checked to make sure they match cryptographically. Next, the given hostname is checked against the Subject of the certificate. Lastly, the command checks if a chain from your certificate all the way to a root CA can be found. This ensures your certificate and private key will be trusted by web browsers. The last two checks will only pass if your certificate is not self signed. If you are uploading a self signed certificate, use the `-s` flag to tell the CLI to skip the hostname and root CA chain check.

If a chain from your certificate to a root CA cannot be found, the CLI will attempt to resolve this and download intermediate certificates and a root CA. This is enabled by default and should be enabled to ensure a proper certificates and private keys are uploaded to Stratum. However, you can disable certificate chain resolution by passing in `-r false` to the [certs create](/paas/paas-cli-reference/certs-create/) command. If you would like to perform the certificate chain resolution as a distinct task, the new [ssl resolve](/paas/paas-cli-reference/ssl-resolve/) command will do just that.

Once all the checks from above pass, the certificate and private key are uploaded to Stratum and your cert instance is created with the given hostname. It is important to note that simply creating a cert does not imply use. For a cert to be used, it must be applied to one or more sites as described in the next section. Allowing a single cert instance to apply to more than one site allows easier certificate management, especially when certificates expire. Using the [certs update](/paas/paas-cli-reference/certs-update/) command will upload new certificates and private keys. Upon next service proxy redeploy, your new certificates and private keys will be applied to all sites that use the updated cert.

The redeploy process does take a couple of minutes to complete.  The redeploy launches a new docker container, a new AWS Elastic Load Balancer and DNS record to resolve the Public DNS name of the environment, pod02????.catalyzeapps.com to the DNS record of the ELB. The number of containers, ELBs and DNS records is equal to the scale limit of the service proxy service. The deployment of those resources is relatively quick, under one minute. The lag primarily lies in the DNS propagation, with the record type having a TTL of 60 seconds.

## Sites

Sites are an individual component that apply to a single code service and utilize a single cert. The [sites](/paas/paas-cli-reference/sites/) command group in the CLI has four subcommands to manage your site configurations:
- [sites create](/paas/paas-cli-reference/sites-create/)
- [sites list](/paas/paas-cli-reference/sites-list/)
- [sites rm](/paas/paas-cli-reference/sites-rm/)
- [sites show](/paas/paas-cli-reference/sites-show/).

Before creating a site, you'll first need to have created a cert as outlined in the previous section. You will need the hostname used during the [certs create](/paas/paas-cli-reference/certs-create/) command. To create a site, run

```
catalyze sites create .example.com app01 wildcard_examplecom
```

Site names need to match the domain that they will respond to and do not need to match the cert hostname used in the [certs create](/paas/paas-cli-reference/certs-create/) command. For example, a site that responds on a single domain should be named with `example.com` or some other single domain name. If the site uses a wildcard cert and should respond on the APEX domain as well as all subdomains should be named `.example.com` notice the leading `.`. The site name is directly injected into the nginx configuration file as the `server_name`.

In the example shown, we are naming our site `.example.com` and we are assigning this site to our `app01` code service. We are also using the cert we created in the previous section called `wildcard_examplecom` which is a wildcard certificate for `*.example.com`. Now we have a proper site created and are ready to redeploy our services! The only thing remaining is to redeploy the service proxy to make the configurations go live. Simply run

```
catalyze redeploy service_proxy
```

to see your changes go live.

Some older Stratum environments and any environments with heavy customizations to their service proxy will need to have Catalyze support redeploy their service proxy.  The CLI will inform you if this is the case for your environment.  This is to prevent any unwanted downtime by deploying a service proxy with incorrect nginx configurations.

There is one other difference between sites and certs that should be noted. Certs can be updated after creation while sites cannot. Once a site is created, no information about it can be changed. You must remove a site with the [sites rm](/paas/paas-cli-reference/sites-rm/) command and then recreate it.

## Nginx Configuration

Once a site is created, an nginx config file is created and stored on your service proxy. This file, along with other downloadable service files, can be viewed using the [files](/paas/paas-cli-reference/files/) command group in the CLI. To start, list your downloadable service files by running

```
catalyze files list
```

Find the nginx configuration file, typically named similar to `/etc/nginx/sites-enabled/.example.com`, and download it with the following command

```
catalyze files download /etc/nginx/sites-enabled/.example.com
```

This will allow you to view your current nginx configuration and test it out locally. You can make changes, test out new parameters, and see what will work best for you environment! Once you have a set of changes you would like applied, you can [submit a support ticket](https://catalyzeio.zendesk.com/) to request the changes be made.
