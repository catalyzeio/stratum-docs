---
title: Stratum SSH Keys
category: manage
summary: Learn how to manage your SSH keys in Stratum.
---

# Stratum SSH Keys

There are two types of SSH keys that you can setup in Stratum: user keys and deploy keys. These two types, the differences between the two, and in what situations you would choose deploy over user are described below.

It is important to note that before Stratum was launched, Catalyze only allowed one type of SSH key. Those keys were generically named `keys`. With the launch of Stratum, these keys were automatically converted to a deploy key. If you're looking to switch to user keys, check out the [Make the Switch](#make-the-switch) section on how to do that and troubleshoot any issues you may run into.

# User Keys

User keys are the preferred type of keys for the majority of Catalyze users. User keys allow you to access any Catalyze git repo that belongs to a service within an environment that you have access to. For example, let's say Alice is a Catalyze user with two environments: `production` and `development` and each environment has two services named `app01` and `app02`. A user key will grant Alice access to all services in both environments. Alice can access `app01` in production and development as well as `app02` in production and development with nothing more than a single user key.

User keys are intended to be private and not shared with anyone. These keys are tied to your Catalyze user account and can be used for authentication with the Catalyze APIs or as a method of pushing code to the Stratum platform.

# Deploy Keys

Deploy keys are much more restrictive than user keys. Deploy keys give an entity access to a single service in a single environment. Deploy keys are **not** tied to a user account, but tied to a service instead. The primary use for a deploy key is with a CI/CD server. Deploy keys are intended to be used by automated components of your infrastructure with a single purpose of pushing code to one repository.

There are few situations in which deploy keys are better suited than user keys. Primarily they are used with a CI/CD server such as Codeship, Jenkins, or TeamCity. It is important to note that deploy keys **must** be globally unique. It is also required that all deploy keys be different than your user keys.

# How can I setup user keys on my account?

To execute the commands below, you'll need to specify your Catalyze username and password, so be sure to have them available.

## Download the Stratum CLI

All SSH key management will be carried out through the Stratum CLI. Get it [here](https://github.com/catalyzeio/cli).

## Add a Non-Shared Public Key to Your account

Do not use shared public keys for access to your Catalyze account. Your SSH keys should be private.

Below, I add a new SSH key that I made for a Catalyze environment

```
catalyze -E "<your_env_alias>" keys add my_prod_key ~/.ssh/prod_rsa.pub
```

Once that is done, you can list your public keys and see the key you just added

```
catalyze -E "<your_env_alias>" keys list
```

# How can I use an SSH key for authentication in the CLI?

To use an SSH key for authentication in the CLI, it must be added as a user key. After adding a user key as outlined in the previous section, set that key as the authentication key with the CLI. Be sure to specify the private key path when using the `keys set` command.

```
catalyze -E "<your_env_alias>" keys set ~/.ssh/prod_rsa
```

You can now execute CLI commands without using a username/password!

# How do I add a deploy key?

Before adding a deploy key, please make sure to read the sections above on the specifics of [user keys](#user-keys) vs [deploy keys](#deploy-keys). To add a deploy key, you'll need the CLI. Now run the `deploy-keys` command

```
catalyze deploy-keys add codeship_key ~/.ssh/codeship_rsa.pub app01
```

You can now use the codeship_rsa key pair with your CI/CD server to push code to Catalyze!

# SSH Keys and ssh-agent

Git will use ssh-agent to present an identity to the git repository. The ssh-agent program will use the default ssh key file named id_rsa in the .ssh directory of the user's home directory (~/.ssh/id_rsa). Only the default id_rsa key file will be loaded by ssh-agent unless the key is manually added to the agent. The OS X Keychain Access application can be used to load additional ssh keys when the ssh-agent starts. To add a key to the OS X KeyChain Access application, use the command `ssh-add -K <path to ssh key file>` For example: `ssh-add -K ~/.ssh/catalyze-prod.key`.

Similar tools are available for Windows (Pageant) and Linux (keychain) workstations. On Linux, the ssh-agent program can be added to your bash profile to start when bash loads.

# Make the switch

After the launch of Stratum or when migrating to a newer environment, you'll need to switch from using deploy keys to user keys. There's a few steps required in order to use an existing deploy key as a user key. First, be sure to **remove** all existing deploy keys

```
catalyze -E oldEnvironment deploy-keys list
catalyze -E oldEnvironment deploy-keys rm {keyName} {svc}
```

Next, **associate** to your new environment

```
catalyze associate newEnvironment app01
```

Lastly, **add** the recently removed deploy key as a user key and push code.

```
catalyze -E newEnvironment keys add default_key ~/.ssh/id_rsa
git push catalyze master
```

If necessary, reset your SSH agent and add in your default rsa key

```
killall ssh-agent
ssh-add ~/.ssh/id_rsa
```

You can now use your default rsa key (~/.ssh/id_rsa) to push code to app01 in both the old and new environments.

# Help! I'm getting `You do not have access to the requested repository. fatal: Could not read from remote repository.`

The most common error encountered when pushing code to Catalyze is the `You do not have access to the requested repository. fatal: Could not read from remote repository.` error. This can occurs for a couple of different reasons.

To start, you'll want to make sure your SSH agent is aware of the key you want to use for your git push command. Run `ssh-add -l` to list all keys that your SSH agent is managing. If your key is not listed, you'll need to add it by running `ssh-add ~/.ssh/my_key` giving it the path to your SSH private key.

Next, you want to make sure your SSH agent is not managing a deploy key. Run `ssh-add -l` to list your keys and make sure that each key listed should be there. If you need to remove a key, the only option is to remove all keys with `ssh-add -D` and then add the ones you need back one by one with `ssh-add ~/.ssh/path_to_key`. If necessary, you can use `ssh-keygen -lf ~/.ssh/path_to_key` to match the SHA256 hash that the CLI prints out when running `keys list` or `deploy-keys list`.

The last step you want to try is making sure that your user key is not also a deploy key. To fix this, read the [Make the switch](#make-the-switch) section to ensure that all of your deploy keys are unique or removed.

One of the most effective ways to debug this error is adding the `-v` flag to your git push command. When adding the `-v` flag, you can see which keys are offered by your SSH agent and in what order. Here's a sample partial output

```
debug1: Authentications that can continue: publickey
debug1: Next authentication method: publickey
debug1: Offering RSA public key: /Users/catalyze/.ssh/deploy_rsa
debug1: Authentications that can continue: publickey
debug1: Offering RSA public key: /Users/catalyze/.ssh/id_rsa
debug1: Authentications that can continue: publickey
debug1: Trying private key: /Users/catalyze/.ssh/id_dsa
debug1: Trying private key: /Users/catalyze/.ssh/id_ecdsa
debug1: Trying private key: /Users/catalyze/.ssh/id_ed25519
debug1: No more authentication methods to try.
```

This shows that `deploy_rsa` was offered first and rejected, followed by `id_rsa` which was also rejected. The SSH agent attempted to find other default keys named `id_dsa`, `id_ecdsa`, and `id_ed25519` but those files did not exist. At last, the SSH agent gave up and the push was rejected. This can be an extremely effective tool when fixing the `Could not read from remote repository` error.
