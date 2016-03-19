# How do I setup Codeship to deploy to Catalyze?

When thinking about the best tools for continuous integration Codeship is always near the top of the list. In fact we even use it here at Catalyze for many of our web properties. If you're a Stratum user we've made it very simple to use Codeship with your environments. The first thing you'll want todo is to make sure you have the latest version of our CLI [here](https://github.com/catalyzeio/cli). Once that's installed and you've associated to your environment, you can follow these three steps:

1. Head over to your project settings in Codeship and find the SSH public key that was setup for your Codeship project. This is under project settings -> general.
2. Save that SSH public key to a file and add it as a `deploy key` with the `catalyze deploy-keys add` command in the CLI. Deploy keys are intended to be used for CI purposes and shared among teams. These are very much like a Github deploy key. Deploy keys have access to one code service only.
    1. Here's an example command `catalyze deploy-keys add codeship ./ssh_key.pub app01`
3. Add a custom deploy script to your Codeship project. This custom script only needs one line: `git push {YOUR_GIT_REMOTE_HERE} $CI_COMMIT_ID:master`
    1. The git remote can be found in your initial environment provisioning emails with Catalyze support engineers or by running `git remote -v` from within your git repo after you've run the `catalyze associate` command with the CLI.

That's it! Codeship is ready to deploy to Catalyze.
