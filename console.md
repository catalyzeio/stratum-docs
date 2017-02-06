---
title: Secure Console
category: manage
summary: Using the Secure Console on Compliant Cloud.
---

The **Secure Console** offers interactive command-line access to your [environment](/compliant-cloud/articles/concepts/environments)'s [database services](/compliant-cloud/articles/concepts/services#database-services), using the corresponding shell. These map out as follows:

* PostgreSQL: `psql`
* MySQL (Percona): `mysql`
* MongoDO: `mongo`

## Opening a Secure Console

Using the [CLI](/compliant-cloud/articles/cli-stratum)'s [console](/paas/paas-cli-reference#console) command and the name of the database service (found in the [Compliant Cloud dashboard](https://product.datica.com/compliant-cloud)):

```
catalyze -E "<your_env_alias>" console <database name>
```

For example, for a database named `db01`:

```
catalyze -E "<your_env_alias>" console db01
```

## Using the Console With Code Services

The console can also be used with code services, but a command needs to be specified. For example, to run `rake mytask` on a code service named `app01`:

```
catalyze -E "<your_env_alias>" console app01 'rake mytask'
```

You may see an error message about the command you are trying to run not being **whitelisted**. This means we have not yet approved its compatibility with the console, or it's custom tooling in a pattern that we haven't yet whitelisted. For most cases, we can resolve it quickly via [Support Ticket](/compliant-cloud/articles/contact).

## Console vs SSH

The secure console is **not** SSH, and is quite limited. It is a temporary tunnel, and can at times close abruptly. To enable SSH to your environment's [containers](/compliant-cloud/articles/concepts/containers), a [VPN](/compliant-cloud/articles/vpn-stratum) is required.
