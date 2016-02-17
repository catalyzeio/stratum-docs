# Self-Service SSL Guide

With our latest release of the Stratum CLI, we have implemented a whole slew of features and bug fixes. Perhaps the most intriguing pieces of functionality are two sets of commands: the [certs](https://resources.catalyze.io/paas/cli/sections/certs/) and [sites](https://resources.catalyze.io/paas/cli/sections/sites/) commands. These two sets of commands will allow self-service SSL management. The following sections will explain how to use these two sets of commands to their fullest extent and manage your SSL certificates with ease and on your own schedule.

## Certs

A cert represents a reusable component composed of an SSL certificate chain and a private key. These are securely uploaded and stored on the Catalyze platform and become available to your entire environment.

The [certs](https://resources.catalyze.io/paas/cli/sections/certs/) command group in the CLI has four subcommands to manage your SSL certificates:

- [certs create](https://resources.catalyze.io/paas/cli/sections/certs-create/)
- [certs list](https://resources.catalyze.io/paas/cli/sections/certs-list/)
- [certs rm](https://resources.catalyze.io/paas/cli/sections/certs-rm/)
- [certs update](https://resources.catalyze.io/paas/cli/sections/certs-update/)

### Create a Cert

To start, you'll need to create a cert. Make sure both the certificate and private key are **unencrypted** and in **PEM** format. Then run

***CA-Signed SSL Cert***
```
catalyze certs create mysite.com ./mysite.crt ./mysite.key
```
***Self-Signed SSL Cert***
```
catalyze certs create -s mysite.com ./mysite_selfsigned.crt ./mysite_selfsigned.key
```

#### Cert Creation Behind-The-Scenes

This command will do a lot of work behind the scenes! First, the certificate and private key are checked to make sure they match cryptographically. Next, the given hostname is checked against the Subject of the certificate. Lastly, the command checks if a chain from your certificate all the way to a root CA can be found. This ensures your certificate and private key will be trusted by web browsers. The last two checks will only pass if your certificate is not self signed. If you are uploading a self signed certificate, use the `-s` flag to tell the CLI to skip the hostname and root CA chain check.

If a chain from your certificate to a root CA cannot be found, the CLI will attempt to resolve this and download intermediate certificates and a root CA. This is enabled by default and should be enabled to ensure a proper certificates and private keys are uploaded to Stratum. However, you can disable certificate chain resolution by passing in `-r false` to the [certs create](https://resources.catalyze.io/paas/cli/sections/certs-create/) command. If you would like to perform the certificate chain resolution as a distinct task, the new [ssl resolve](https://resources.catalyze.io/paas/cli/sections/ssl-resolve/) command will do just that.

Once all the checks from above pass, the certificate and private key are uploaded to Stratum and your cert instance is created with the given hostname. It is important to note that simply creating a cert does not imply use. For a cert to be used, it must be applied to one or more sites as described in the next section. Allowing a single cert instance to apply to more than one site allows easier certificate management, especially when certificates expire. Using the [certs update](https://resources.catalyze.io/paas/cli/sections/certs-update/) command will upload new certificates and private keys. Upon next service proxy redeploy, your new certificates and private keys will be applied to all sites that use the updated cert.

## Sites

Sites are an individual component that apply to a single code service and utilize a single cert. The [sites](https://resources.catalyze.io/paas/cli/sections/sites/) command group in the CLI has four subcommands to manage your site configurations:
- [sites create](https://resources.catalyze.io/paas/cli/sections/sites-create/)
- [sites list](https://resources.catalyze.io/paas/cli/sections/sites-list/)
- [sites rm](https://resources.catalyze.io/paas/cli/sections/sites-rm/)
- [sites show](https://resources.catalyze.io/paas/cli/sections/sites-show/).

Before creating a site, you'll first need to have created a cert as outlined in the previous section. You will need the hostname used during the [certs create](https://resources.catalyze.io/paas/cli/sections/certs-create/) command. To create a site, run

```
catalyze sites create app01.mysite.com app01 mysite.com
```

Since a site is tied to a single service, you should give it a name that uniquely represents it and the purpose it will serve. The name of a server will be used as the `server_name` for the site's nginx configuration file. It is not required to match the hostname given in the [certs create](https://resources.catalyze.io/paas/cli/sections/certs-create/) command, however it will often be close due to wildcard certificates and subdomains as shown above.

In the example shown, we are naming our site `app01.mysite.com` and we are assigning this site to our `app01` code service. We are also using the cert we created in the previous section called `mysite.com` which is a wildcard certificate for `*.mysite.com`. Now we have a proper site created and are ready to redeploy our services! The only thing remaining is to redeploy the service proxy to make the configurations go live. Simply run

```
catalyze redeploy service_proxy
```

to see your changes go live.

There is one other difference between sites and certs that should be noted. Certs can be updated after creation while sites cannot. Once a site is created, no information about it can be changed. You must remove a site with the [sites rm](https://resources.catalyze.io/paas/cli/sections/sites-rm/) command and then recreate it.

## Nginx Configuration

Once a site is created, an nginx config file is created and stored on your service proxy. This file, along with other downloadable service files, can be viewed using the [files](https://resources.catalyze.io/paas/cli/sections/files/) command group in the CLI. To start, list your downloadable service files by running

```
catalyze files list
```

Find the nginx configuration file, typically named similar to `/etc/nginx/sites-enabled/app01.mysite.com`, and download it with the following command

```
catalyze files download /etc/nginx/sites-enabled/app01.mysite.com
```

This will allow you to view your current nginx configuration and test it out locally. You can make changes, test out new parameters, and see what will work best for you environment! Once you have a set of changes you would like applied, you can [submit a support ticket](https://catalyzeio.zendesk.com/) to request the changes be made.
