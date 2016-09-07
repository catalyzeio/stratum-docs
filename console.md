---
title: Secure Console
category: manage
summary: Using the Secure Console on Stratum.
---

The **Secure Console** offers interactive command-line access to your [environment](/stratum/articles/concepts/environments)'s [database services](/stratum/articles/concepts/services#database-services), using the corresponding shell. These map out as follows:

* PostgreSQL: `psql`
* MySQL (Percona): `mysql`
* MongoDO: `mongo`

## Opening a Secure Console

Using the [CLI](/stratum/articles/cli-stratum)'s [console](/paas/paas-cli-reference#console) command and the name of the database service (found in the [Stratum dashboard](https://product.catalyze.io/stratum)):

```
catalyze console <database name>
```

For example, for a database named `db01`:

```
catalyze console db01
```

## Using the Console With Code Services

The console can also be used with code services, but a command needs to be specified. For example, to run `rake mytask` on a code service named `app01`:

```
catalyze console app01 'rake mytask'
```

You may see an error message about the command you are trying to run not being **whitelisted**. This means we have not yet approved its compatibility with the console, or it's custom tooling in a pattern that we haven't yet whitelisted. For most cases, we can resolve it quickly via [Support Ticket](/stratum/articles/contact).

## Console vs SSH

The secure console is **not** SSH, and is quite limited. It is a temporary tunnel, and can at times close abruptly. To enable SSH to your environment's [containers](/stratum/articles/concepts/containers), a [VPN](/stratum/articles/vpn-stratum) is required.
