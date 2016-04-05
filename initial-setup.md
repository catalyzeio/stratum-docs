---
title: Stratum Initial Setup
---

## Introduction

The following article is intended to get new users up and running in short order. We do have verbose, in depth write ups on each step outlined below. If you'd like to read more please see our official [getting started guide](//resources.catalyze.io/stratum/getting-started/). However it is recommended that you complete this guide before moving on to anything else.

What we'll cover in this guide:

- 1. Gaining access to an organization and related environment(s)
- 2. Downloading the CLI
- 3. Associating a local git repository with a Catalyze remote repository
- 4. Adding an SSH Key
- 5. Adding an SSL certificate
- 6. Deploying code
- 7. Setting up DNS

## Prerequisite: Access

Once you've been notified that your Stratum environment has been provisioned you will receive an email containing an invite link to an organization that Catalyze has created on your behalf.

It's important that you first create a Catalyze user account before clicking the invite link. You can register for an account [here](https://product.catalyze.io/account/register). If you've already created an account it may be helpful to [sign in](https://product.catalyze.io/account/signin) before accepting the invite.

Now that you've successfully created an account and signed in you can now accept the invite as the owner of the newly created organization. From here you can begin building your team by inviting members and admins to join. The video below demonstrates how one would sign in, navigate to an organization, and manage inviting a user. To learn more about the types of roles in an organization and what they mean, visit [here](https://resources.catalyze.io/stratum/articles/organization-access-controls).

<div style="width: 100%; height: 0px; position: relative; padding-bottom: 65.653%;"><iframe src="https://streamable.com/e/ukd0" frameborder="0" allowfullscreen webkitallowfullscreen mozallowfullscreen scrolling="no" style="width: 100%; height: 100%; position: absolute;"></iframe></div>

**Recap:** [Sign up](https://product.catalyze.io/account/register) for a Catalyze account before accepting the org owner invite. Accept the invite, and use the [dashboard](//product.catalyze.io/stratum) to invite new users to your organization.

## Catalyze CLI

After gaining access to your organization you'll want to download the Catalyze CLI (command line interface). Below are the latest available builds:

**Please note:** If you have an existing version of the Catalyze CLI you can simply run `catalyze update` to get the latest version.

### Darwin (Apple Mac)

 * [catalyze\_3.1.4\_darwin\_386.zip](https://github.com/catalyzeio/cli/releases/download/3.1.4/catalyze_3.1.4_darwin_386.zip)
 * [catalyze\_3.1.4\_darwin\_amd64.zip](https://github.com/catalyzeio/cli/releases/download/3.1.4/catalyze_3.1.4_darwin_amd64.zip)

### Linux

 * [catalyze\_3.1.4\_amd64.deb](https://github.com/catalyzeio/cli/releases/download/3.1.4/catalyze_3.1.4_amd64.deb)
 * [catalyze\_3.1.4\_armhf.deb](https://github.com/catalyzeio/cli/releases/download/3.1.4/catalyze_3.1.4_armhf.deb)
 * [catalyze\_3.1.4\_i386.deb](https://github.com/catalyzeio/cli/releases/download/3.1.4/catalyze_3.1.4_i386.deb)
 * [catalyze\_3.1.4\_linux\_386.tar.gz](https://github.com/catalyzeio/cli/releases/download/3.1.4/catalyze_3.1.4_linux_386.tar.gz)
 * [catalyze\_3.1.4\_linux\_amd64.tar.gz](https://github.com/catalyzeio/cli/releases/download/3.1.4/catalyze_3.1.4_linux_amd64.tar.gz)
 * [catalyze\_3.1.4\_linux\_arm.tar.gz](https://github.com/catalyzeio/cli/releases/download/3.1.4/catalyze_3.1.4_linux_arm.tar.gz)

### MS Windows

 * [catalyze\_3.1.4\_windows\_386.zip](https://github.com/catalyzeio/cli/releases/download/3.1.4/catalyze_3.1.4_windows_386.zip)
 * [catalyze\_3.1.4\_windows\_amd64.zip](https://github.com/catalyzeio/cli/releases/download/3.1.4/catalyze_3.1.4_windows_amd64.zip)


 **Recap:** [Download](https://github.com/catalyzeio/cli) the latest version of the Catalyze CLI.

## Environment Association

Once you have the latest build of the Catalyze CLI you'll want to create an association between your CLI install and the newly created Stratum environment.

**Please note:** Before moving on, be advised that the Catalyze CLI is the primary tool for interacting with your environment. Our design philosophy is that the Stratum dashboard is the view layer into your environment and related services. While the CLI is a tool that can perform actions on those environments and related services.

To associate your application repository to your Catalyze remote repository navigate to the root directory of your application. This will always be where the `.git` folder resides.

Once inside you can run the `catalyze associate` command. This command takes two arguments: `ENV_NAME` and `SERVICE_NAME`. The `ENV_NAME` is the name of the provisioned environment that you want associated with the application you're located inside of. The `ENV_NAME` is located on your contract as well as inside the Stratum dashboard. The `SERVICE_NAME` is the name of the related code service. We automatically name this for you so in most cases you'll be looking for `app01`.

**Full example:** `catalyze associate DemoProd app01`

**Recap:** Run the `catalyze associate ENV_NAME SERVICE_NAME [-a] [-r] [-d]` command to associate your application repository with your Catalyze remote repository.

## SSH Keys

The next thing you'll want to do is setup your SSH key so you can push code to your Catalyze remote repository. Any SSH key uploaded to your Catalyze user account will be available for use with all code services and environments that you have access to.

First you'll want to create a brand new public key. This key should not be shared or used anywhere else. The following command should be run with the email you signed up with (**Please note:** After creating your new public key you'll need to add it to your ssh agent: `ssh-add KEY_PATH`).

`ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`

Once you have your new key you can add it to your Catalyze user account with the following command:

`catalyze keys add NAME PUBLIC_KEY_PATH`

**Full example:** `catalyze keys add my_prod_key ~/.ssh/prod_rsa.pub`

**Recap:** Generate a new public SSH key and upload it to your Catalyze user account using the `catalyze keys add NAME PUBLIC_KEY_PATH` command.

## SSL Certificates

Getting an SSL certificate installed is by far the most involved step in this setup guide. If you'd like to read more information than what is outlined below please refer to [this separate guide](https://resources.catalyze.io/stratum/articles/guides/self-service-SSL/).

To start, you’ll need to create a cert. Make sure both the certificate and private key are unencrypted and in PEM format. If you have a Certificate Authority Signed certificate (CA-Signed) you can run the following command using your own correct file names/paths:

`catalyze certs create wildcard_examplecom ./example.crt ./example.key`

If you have a self-signed certifcate you can run the following command using your own correct file names/paths:

`catalyze certs create examplecom ./example_selfsigned.crt ./example_selfsigned.key -s`

**Please note:** If for any reason you find yourself stuck you can run any Catalyze CLI command without arguments to see a full manual on that specific command.

**Recap:** Use a variation of the `catalyze certs create` command to upload your SSL certificate.

## Push Code

The moment of truth. It's time to make our first code push, and it couldn't be easier. Simply navigate to application's code repository (the same place we were in the associate step) and run `git push catalyze master`.

This pushes your master branch to the Catalyze master branch. If you want to push a branch that is not named master, see the following:

`git push catalyze mybranch_name:master`

**Please note:** Even when you're not using your local master branch it is necessary to use the Catalyze remote master branch.

**Recap:** Push your code using the `git push catalyze master` command!

## DNS Setup

Now that we've successfully added an SSL certificate and made our first code push we'll want to point the correct domain name to the appropriate Catalyze public hostname.

The public hostname for your environment is unique, and will be sent to you during the initial onboarding steps. Alternatively you can run the `catalyze sites list` command to see your hostname.

The next step is to simply add the CNAME rules using your appropriate name and domain:

**Full example:** `mysite.wxyz.com CNAME pod0123.catalyzeapps.com`

You can also use an ALIAS:

`wxyz.com ALIAS pod0123.catalyzeapps.com`

## Next Steps

To read more about everything covered in this guide please see the official getting started guide [here](https://resources.catalyze.io/stratum/getting-started/).
