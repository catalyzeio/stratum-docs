---
title: Deploying Your Application to Datica
category: application
summary: How do I deploy to my Datica environment?
---

# How do I deploy to my Datica environment?

## First-Time Setup

1. If you haven't already, install the [Compliant Cloud CLI](https://github.com/daticahealth/cli).
2. Navigate to the root directory of your local repository (this is the directory that contains the `.git` directory).
3. Run the `init` command if you have not signed in already (the init command does this for you) or run the `git-remote add` command, replacing `MyEnvName` and `my-code-service` with your environment's name and your code service's name both found in the [Compliant Cloud UI](https://product.datica.com/compliant-cloud/):

   ```
   # If you have not signed in with the cli yet
   datica init
   # OR if you have
   datica -E MyEnvName git-remote add my-code-service
   ```

## Deploying Code

Push your local `master` branch to the `datica` remote (created when you ran `init`):

```
git push datica master
```

If the branch you want to push is not master, run this instead:

```
git push datica my-branch-name:master
```

The build output will stream from Datica's servers to your terminal. If the build is successful, your service will be redeployed.

### See also

* [Writing Your Application](/compliant-cloud/articles/writing-your-application)
* [CLI Associate](/compliant-cloud/cli-reference#associate)
