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

You can now execute CLI commands without needed a username/password!
