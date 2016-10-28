---
title: Catalyze CLI Troubleshooting
category: troubleshooting
---

# Catalyze CLI Troubleshooting

## Installation
	TODO: Find common CLI installation issues in zendesk and add the solutions here 

## Git
- **You do not have access to the requested repository fatal: Could not read from remote repository. Please make sure you have the correct access rights and the repository exists.**
    - **Potential Issue:** You are using a deploy key from a different environment.
    - **Potential Solution:** Set up user keys as a replacement for deploy keys. User keys are associated with your user and will work on any environment. See [this guide](/stratum/articles/ssh-keys/#how-can-i-setup-user-keys-on-my-account?) for instructions on setting up user keys.