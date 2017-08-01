---
title: Let's Encrypt Certificates
category: guide
---

# Let's Encrypt Certificates

With the release of version 3.0, Compliant Cloud supports Let's Encrypt certificates. Let's Encrypt certificates allow you to create free SSL certificates that automatically renew before expiring. When Let's Encrypt certificates renew, the updated certificate will be placed in your service proxy **without** needing a redeploy. Let's Encrypt certificates are available to all Compliant Cloud customers.

## Setting up DNS

Before creating a Let's Encrypt certificate, you must setup a CNAME DNS record pointing to your environment's default domain. You can find out your environment's default domain by running the `domain` command with the Compliant Cloud CLI:

```
datica -E "<your_env_name> domain"
```

A CNAME DNS record is required due to the way domain verification is done through Let's Encrypt. A `GET` request will be made to a URL of the requested domain. You don't need to worry about responding to the `GET` request in your application, this is automatically handled by Compliant Cloud. You can read more about Let's Encrypt domain validation [here](https://letsencrypt.org/how-it-works/).

## Creating a Let's Encrypt certificate

To create a Let's Encrypt certificate, you can use the [sites create](//compliant-cloud/cli-reference/#sites-create) command. This command takes the form

```
datica -E "<your_env_name>" sites create <domain_name> <code_service> --lets-encrypt
```

This will automatically create a Let's Encrypt certificate with the same domain name as the site. Wildcard domains are [currently not supported](https://letsencrypt.org/2017/07/06/wildcard-certificates-coming-jan-2018.html). You need to create one certificate for each subdomain. If you wanted to create a site for the domain code-1.example.com forwarding traffic to your code-1 service with a Let's Encrypt certificate, you would run:

```
datica -E "<your_env_name>" sites create code-1.example.com code-1 --lets-encrypt
```

Let's Encrypt certificates are issued asynchronously. Until the certificate has been successfully issued by Let's Encrypt, you may see a temporary self-signed placeholder certificate. As always, when you create a new site, you must redeploy the service proxy for the changes to go live:

```
datica -E "<your_env_name>" redeploy service_proxy
```

Once the Let's Encrypt certificate is successfully issued, the placeholder certificate will automatically be replaced **without** needing a second redeploy.

## Checking the issuance status

You can check on the issuance status of Let's Encrypt certificates by using the [certs list](//compliant-cloud/cli-reference/#certs-list) command.

```
datica -E "<your_env_name>" certs list
```

## Managing Let's Encrypt Certificates

Aside from automatically creating a Let's Encrypt certificate with the sites command, they can be created, shown, or deleted using the [certs](//compliant-cloud/cli-reference/#certs) commands. Let's Encrypt certificates cannot be modified with the update command. To create a Let's Encrypt certificate using the `certs` command, run:

```
datica -E "<your_env_name>" certs create code-1.example.com --lets-encrypt
```

After creating the certificate, you can use it with the `sites create` command as normal by running:

```
datica -E "<your_env_name>" sites create code-1.example.com code-1 code-1.example.com
```

Notice in this case you do not specify `--lets-encrypt` when running `sites create`. The `--lets-encrypt` flag is required only when you want a new certificate automatically generated. In this case we already created one with the `certs create` command so we don't want to generate another.
