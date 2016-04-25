---
title: Custom Buildpacks
category: buildpack
---

# How can I modify the Stratum buildpack process?

Catalyze has created several ways that users can modify the standard buildpack process to help them meet their application needs.

## Dependencies Outside The Buildpack

Sometimes an application will have dependencies outside of the buildpack that cannot be retrieved by other means such as Ruby gems. Catalyze provides the means to install software from the apt repositories for Ubuntu 14.04 on Pod02 and Ubuntu 12.04 on Pod01.

In your application repository, create a file at the following path:

`/your_git_repository_root/.catalyze/packages`

List the packages that you would like installed in your environment, one package per line, in the `packages` file. Keep in mind that these packages must be available in the base Ubuntu repositories. The standard location of binaries installed via this method is `/usr/bin`.

## Pre and Post Build Hooks

On our Pod02 infrastructure, Catalyze has added the ability to run pre-build and post-build scripts during the application build process.

These scripts have access to the dedicated, encrypted network of your environment, which means that they can talk to the existing services such as databases or message brokers.

The scripts run as an unprivileged user with minimal permissions. They are able to write to your application’s working directory and run any software installed by the buildpack (e.g., `bundle exec rake db:migrate`) but not much else.

If either the pre- or post-build hook fails, the overall build will be marked as failed and your application will not be redeployed. However, any operations that were performed by your hook scripts will not be rolled back, so please exercise caution if you are performing database modifications during builds.

Build hook failures should be indicated in the usual non-zero exit code manner. If you are running a shell script as your hook you should strongly consider using `#!/bin/sh -e` or `set -e`.

Build executions have a time limit of 30 minutes, so be careful not to do anything overly lengthy with these scripts.

### Pre-Build Hooks

The pre-build script should be located at `/your_git_repository_root/.catalyze/pre-build`. The file ***must be executable*** to run during the build process.

Keep in mind that the pre-build script runs before the buildpack, so most utilities (rake, npm, etc.) will not be available.

### Post-Build Hooks

The pre-build script should be located at `/your_git_repository_root/.catalyze/post-build`. The file ***must be executable*** to run during the build process.

This is a common location to place database migration commands so that the running database is prepared prior to the new code base deployment.
