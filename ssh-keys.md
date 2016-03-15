---
title: Stratum SSH Keys
---

# Stratum SSH Keys

SSH keys can be used for authentication and pushing code to the Stratum platform. Any SSH keys added to your user account should not be shared but be treated as private SSH keys. Any SSH key uploaded to your user account will be able to be used with all code services and environments that you have access to.

## How can I setup SSH keys on an account?

To execute the commands below, you'll need to specify your Catalyze username and password, so be sure to have them available.

### Download the Stratum CLI

All SSH key management will be carried out through the Stratum CLI.
Get it [here](https://github.com/catalyzeio/cli).

### Add a Non-Shared Public Key to Your account

Do not use shared public keys for access to your Catalyze account. Your SSH keys should be private.

Below, I add a new SSH key that I made for a Catalyze environment

`catalyze keys add my_prod_key ~/.ssh/prod_rsa.pub`

Once that is done, you can list your public keys and see the key you just added:

`catalyze keys list`

### Set the CLI to Use the Corresponding Private Key

The final step to use SSH key authentication is to point the CLI at the private key that matches a public key stored in Stratum. Using the key we added above:

`catalyze keys set ~/.ssh/prod_rsa`

You can now execute CLI commands without using a username/password!

### SSH Keys and ssh-agent

Git will use ssh-agent to present an identity to the git repository. The ssh-agent program will use the default ssh key file named id_rsa in the .ssh directory of the user's home directory (~/.ssh/id_rsa). Only the default id_rsa key file will be loaded by ssh-agent unless the key is manually added to the agent. The OS X Keychain Access application can be used to load additional ssh keys when the ssh-agent starts. To add a key to the OS X KeyChain Access application, use the command `ssh-add -K <path to ssh key file>` For example: `ssh-add -K ~/.ssh/catalyze-prod.key`. 

Similar tools are available for Windows (Pageant) and Linux (keychain) workstations. On Linux, the ssh-agent program can be added to your bash profile to start when bash loads. 



