---
title: Datica CLI Troubleshooting
category: troubleshooting
---

# Datica CLI Troubleshooting

## Installation
**bash: datica: command not found**

- **Potential Issue:** You have not added the Datica CLI binary to your PATH environment variable.
- **Potential Solution:** You can add the Datica CLI to your `PATH` environment variable by running `export PATH=$PATH:/path/to/your/datica/cli/binary` or adding that line to your bash profile.

## Update

**[fatal] A required update has been applied. Please re-run this command.**

- **Potential Issue:** Your user cannot write to the file or directory the Datica CLI binary is stored in.
- **Potential Solution:** `sudo chown -R $(whoami) /path/to/your/cli/binary` If this doesn't work you can uninstall and [reinstall the CLI](https://github.com/daticahealth/cli).

## Pod ID Not Found

**fatal (0) Pod ID Not Found: Invalid Pod ID specified in X-Pod-ID header**

- **Potential Issue:** The Datica CLI references a configuration file `~/.datica` to cache configuration data.  This configuration file may contain old configuration information that may need to be updated.
- **Potential Solution:** To erase all or some cached configuration data you can use the [datica clear](/compliant-cloud/cli-reference#clear) command.

## Console

**fatal (92005) Console Setup Error: {"code":500,"error":"Internal Error"}**

- **Potential Issue:** You are trying to connect and run a console command on a job that is not running or a code service that has failed to build and deploy.
- **Potential Solution:** Ensure that the service you are trying to run a console command for has jobs that are running. You can [check the status of your services](/compliant-cloud/cli-reference#status) by using the Datica CLI command `datica -E "<your_env_alias>" status`.  If you have ensured that your service is running and you are still receiving this error, contact [Datica support](/compliant-cloud/articles/contact/).

**(0) Command Not Authorized: The command supplied is not a whitelisted command**

- **Issue:** The [console](/compliant-cloud/articles/console/) command you are trying to run is not in the whitelist.
-**Solution** Datica whitelists commands that you are able to run for security purposes. Contact [Datica support](/compliant-cloud/articles/contact/) to request a command be added to the whitelist.

## Git

**You do not have access to the requested repository fatal: Could not read from remote repository. Please make sure you have the correct access rights and the repository exists.**

- **Potential Issue:** You are using a deploy key from a different environment.
- **Potential Solution:** Set up user keys as a replacement for deploy keys. User keys are associated with your user and will work on any environment. See [this guide](/compliant-cloud/articles/ssh-keys/#how-can-i-setup-user-keys-on-my-account?) for instructions on setting up user keys.
