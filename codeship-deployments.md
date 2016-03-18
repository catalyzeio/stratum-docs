# How do I setup Codeship to deploy to Catalyze?

First, you need to install the latest version of the Stratum CLI [here](https://github.com/catalyzeio/cli). Once you have the CLI installed, you'll need to follow these three steps.

1. Head over to your project settings in Codeship and find the SSH public key that was setup for your Codeship project. This is under project settings -> general.
2. Save that SSH public key to a file and add it as a `deploy key` with the `catalyze deploy-keys add` command in the CLI. Deploy keys are intended to be used for CI purposes and shared among teams. These are very much like a Github deploy key. Deploy keys have access to one code service only.
    1. Here's an example command `catalyze deploy-keys add codeship ./ssh_key.pub app01`
3. Add a custom deploy script to your Codeship project. This custom script only needs one line: `git push {YOUR_GIT_REMOTE_HERE} $CI_COMMIT_ID:master`
    1. The git remote can be found in your initial environment provisioning emails with Catalyze support engineers or by running `git remote -v` from within your git repo after you've run the `catalyze associate` command with the CLI.

That's it! Codeship is ready to deploy to Catalyze.
