---
title: Migrating to CLI version 4
category: guide
---

# Compliant Cloud CLI Version 4

The release of version 4.0.0 of the Compliant Cloud CLI brings major changes to existing commands as well as changes in CLI workflows. This guide will walk through upgrading to version 4 and explain the changes that it brings.

## Upgrading to version 4

Upgrading to CLI version 4 does not happen automatically or by running the `datica update` command. Please visit the [releases page](https://github.com/daticahealth/cli/releases) to download the latest version of the CLI. Unzip the package and replace your old CLI binary with the new one. You can find the location of your existing CLI binary by running `which datica`. After upgrading, running `datica --version` should reflect the version downloaded. Automatic updates will begin working after this one time manual upgrade.

## The removal of associate

Prior to version 4, the `associate` command was the primary entry point for getting started with the Compliant Cloud CLI. `associate` performed two functions:

1. Added the specified environment to the local cache
1. Set up a git remote in the current directory

The `associate` command has been removed in version 4. The first task is now automatic (see the next section for more information). The second is accomplished with the new `init` command (see the section titled _`init` command_ below) or by the `git-remote add` command. Run `datica git-remote add --help` for more information.

## No setup required

With version 4 of the CLI you can run any command directly with the `-E` flag without any prior configuration or setup. With version 3 and below, you needed to run the `associate` command before running any other commands with the `-E` flag. With the removal of the `associate` command, you must now specify `-E` with the full name of the environment.

> Note: You do not need to run the `init` command before using `-E` with any command

For example, if you wanted to set an environment variable for a code service on staging and production environments with version 3 of the CLI you had to perform the following commands:

```
cd /path/to/staging/git/repo
datica associate staging-env code-1
datica -E staging-env vars set TEST=true
datica -E staging-env redeploy code-1

cd /path/to/production/git/repo
datica associate production-env code-1
datica -E production-env vars set TEST=false
datica -E production-env redeploy code-1
```

With version 4 of the CLI, the same process is as follows:

```
datica -E staging-env vars set code-1 TEST=true
datica -E staging-env redeploy code-1

datica -E production-env vars set code-1 TEST=false
datica -E production-env redeploy code-1
```

The CLI automatically finds the appropriate environment based on the `-E` flag without the need for associating.

## Removal of aliases

The `associate` command allowed you to specify a local alias for your environments. With the removal of the `associate` command, support for aliases has also been removed. The value of the `-E` flag will be the full environment name.

## `init` command

The entry point for new CLI users is the `init` command. To get started with the CLI you have to follow a three step process:

1. Get an environment setup by visiting the product dashboard and download the CLI
1. Run `datica init`
1. Run `git push datica master`

The `init` command performs all the same functionality as the `associate` command and more. Specifically, the `init` command will create a git repository in the current directory if one does not exist, add the proper `datica` git remote for the chosen code service, and add an SSH key to your user account if one was not found.

The `init` command should only be run for setting up new environments or for setting up a local development environment. For all other purposes you can run the CLI commands directly by specifying `-E` with the full environment name.

## 32-bit support removed

Datica no longer publishes 32-bit binaries. If you are in need of a 32-bit version of the CLI please [contact Datica support](https://datica.com/support).

## Detailed release notes

Be sure to check out the [release notes](https://github.com/daticahealth/cli/releases/tag/4.0.0) for a detailed list of all changes.
