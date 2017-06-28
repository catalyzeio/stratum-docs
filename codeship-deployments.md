---
title: Setting Up Codeship to Deploy to Datica
category: manage
summary: How do I setup Codeship to deploy to Datica?
---

# How do I setup Codeship to deploy to Datica?

When thinking about the best tools for continuous integration Codeship is always near the top of the list. In fact we even use it here at Datica for many of our web properties. If you're a Compliant Cloud user we've made it very simple to use Codeship with your environments. The first thing you'll want to do is to make sure you have the latest version of our CLI [here](https://github.com/daticahealth/cli). Once that's installed, you can follow these three steps:

- 1. Head over to your project settings in Codeship and find the SSH public key that was setup for your Codeship project. This is under project settings -> general.
- 2. Save that SSH public key to a file and add it as a `deploy key` with the `datica -E "<your_env_name>" deploy-keys add` command in the CLI. Deploy keys are intended to be used for CI purposes and shared among teams. These are very much like a Github deploy key. Deploy keys have access to one code service only.
- 2a. Here's an example command `datica -E "<your_env_name>" deploy-keys add codeship ./ssh_key.pub app01`
- 3. Add a custom deploy script to your Codeship project. This custom script only needs one line: `git push {YOUR_GIT_REMOTE_HERE} $CI_COMMIT_ID:master`
- 3a. The git remote can be found by running [the command `git-remote show`](/compliant-cloude/cli-reference#git-remote-show) in your code repository.

That's it! Codeship is ready to deploy to Datica.
