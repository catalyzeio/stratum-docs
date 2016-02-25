---
title: Deploying Your to Catalyze
---

# How do I deploy to my Catalyze environment?

## Install the Stratum CLI

Get the Stratum CLI [here](https://github.com/catalyzeio/cli)

## Associate Your Environment and Service

Use the CLI to set the Catalyze remote repository for your repo.

Navigate to the root folder for your application repository. This is the folder where the `.git` folder is present.

Run the `associate` command:

Example for `MyProdEnv` with code service named `app03`

`catalyze associate MyProdEnv app03`

Replace the environment name and code service name with the values for your environment that you see in the Stratum Dashboard.

This will create the Catalyze remote entry in the Git repo.

## Push Your Code to Catalyze

Now you can push your code to Catalyze.

`git push catalyze master`

The above example pushes your master branch to the Catalyze master branch.

If you want to push a branch that is not named master, follow this example:

`git push catalyze mybranch_name:master`

The Catalyze repository requires that you push to the master branch and will error if you do not do so.

Every time you do a git push you will see the build output scroll past on your terminal. If you see errors, please fix those and re-deploy.
