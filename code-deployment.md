---
title: Deploying Your Application to Catalyze
category: application
summary: How do I deploy to my Catalyze environment?
---

# How do I deploy to my Catalyze environment?

## First-Time Setup

1. If you haven't already, install the [Stratum CLI](https://github.com/catalyzeio/cli).
2. Navigate to the root directory of your local repository (this is the directory that contains the `.git` directory).
3. Run the `associate` command, replacing `MyEnvName` and `my-code-service` with your environment's name and your code service's name (both found in the [Stratum UI])[https://product.catalyze.io/stratum/):

   ```
   catalyze associate MyEnvName my-code-service
   ```

## Deploying Code

Push your local `master` branch to the `catalyze` remote (created when you ran `associate`):

```
git push catalyze master
```

If the branch you want to push is not master, run this instead:

```
git push catalyze my-branch-name:master
```

The build output will stream from Catalyze's servers to your terminal. If the build is successful, your service will be redeployed.

### See also

* [How Stratum Builds Work](/stratum/articles/builds)
* [Multiple Associations](/stratum/articles/associate-multiple)
* [CLI Associate](/paas/paas-cli-reference#associate)
