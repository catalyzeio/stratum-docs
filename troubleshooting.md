---
title: Stratum Troubleshooting Guide
category: troubleshooting
---

# Stratum Troubleshooting Guide

Welcome to the Stratum troubleshooting guide. In this guide we will give you an overview of common issues users may see when working the platform. Beyond that we'll walk you through common resolutions for these issues.

## Components

In order to help you resolve your issue, we first need to outline the components of Stratum. At a high level Stratum is comprised of:

- [Stratum Dashboard](https://product.catalyze.io/stratum)
- [Catalyze CLI](https://github.com/catalyzeio/cli)
- [Account Manager](https://product.catalyze.io/account)

The Stratum dashboard is where users can check the status of their environment and related services, manage users, and navigate to logging and monitoring.

The Catalyze CLI contains the bulk of platform functionality. For a comprehensive reference check out [this guide](/paas/paas-cli-reference/).

The Account Manager is where users can manage their authentication information, view and edit organization information, and find related Catalyze products.

Below we've listed common problems and resolutions for the previous components.

## Stratum Dashboard

### Sign in
- *400: Account not found for given credentials.*
     - **Issue:** You have entered an invalid email or password.
     - **Solution:** Reset your password or use the correct email/password combination.

### Register
- *Validation: Email is required*
    - **Issue:** You've attempted to register an account with an invalid email.
    - **Solution:** Ensure the email you are signing up with is valid.
- *400: Email already in use. If you've forgotten your password, you can reset it.*
    - **Issue:** You've attempted to register a new account with an email already associated with an account.
    - **Solution:** Ensure that you're using a new email for all new accounts.
- *400: Password is weak. Valid passwords are at least 8 characters in length and include at least one uppercase letter, at least one lowercase letter, and at least one number.*
    - **Issue:** The password you've entered does not adhere to the security requirements.
    - **Solution:** Ensure the password you are entering contains at least 8 characters, and include at least one uppercase letter, at least one lowercase letter, and at least one number.
- *Validation: Password is required*
    - **Issue:** You have attempted to register a new account without a password.
    - **Solution:** Enter a unique password into both the password and confirm password inputs.

## Catalyze CLI

### Git
- **You do not have access to the requested repository fatal: Could not read from remote repository. Please make sure you have the correct access rights and the repository exists.**
    - **Potential Issue:** You are using a deploy key from a different environment.
    - **Potential Solution:** Set up user keys as a replacement for deploy keys. User keys are associated with your user and will work on any environment. See [this guide](/stratum/articles/ssh-keys/#how-can-i-setup-user-keys-on-my-account?) for instructions on setting up user keys.

### Miscellaneous

- **404: Page Not Found NGINX error**
    - **Potential Issue:** You have not configured an SSL Certificate or Sites object with that services in the environment.
    - **Potential Solution:** Configure both a site an a cert with your environment. See this [guide on certs](/stratum/articles/guides/self-service-SSL/) and this [guide on sites](/stratum/articles/initial-setup/#sites-setup) for more details.
