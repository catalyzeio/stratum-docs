---
title: Using a Specific Buildpack Version
category: buildpack
summary: While Stratum can detect and apply a buildpack automatically, Catalyze recommends that Stratum users pin their buildpacks to a specific version.
---

# Using a Specific Buildpack Version

While Stratum can detect and apply a buildpack automatically, Catalyze recommends that Stratum users pin their buildpacks to a specific version.

If the buildpack is not pinned, users run the risk that a buildpack version change unexpectedly breaks their code.

## How do I pin the buildpack version?

The below procedure outlines how to pin the buildpack version for your application.

### Set the BUILPACK_URL environment variable

The Stratum build procedure constructs the application based on automatically detecting the code type or reading in the `BUILDPACK_URL` environment variable.

Every buildpack release is tagged with a version number. You can view the releases on the Github page for the buildpacks. Below is the Python buildpack for example:

Navigate to the [Python Buildpack](https://github.com/heroku/heroku-buildpack-python) in a web browser. You'll see the Github page. Click on the `Releases` link as highlighted below:

![Python](images/buildpack_release_frontpage.png)

This displays all of the buildpack releases as highlighted below.

![Python_Releases](images/buildpack_release_github.png)

Choose the release you want to pin. In our example, we'll choose `v68`. The buildpack URL will look like this:

`https://github.com/heroku/heroku-buildpack-python#v68`

Ensure you have the correct environment and service association for the application you want to modify. Remember, you can only have one code service associated with each environment alias!

Then set the URL. In this example, I've associated my app03 to the alias `MyProdEnv-app03`:

`catalyze -E MyProdEnv-app03 vars set -v BUILDPACK_URL="https://github.com/heroku/heroku-buildpack-python#v68"`

### Rebuild Application

This change will only take effect on a rebuild of the application. Rebuilds are triggered by commits to the code service.
