---
title: Catalyze Resources - Organization Concepts
category: organization
summary: Learn how to manage your organization on Catalyze.
---

# Stratum Organization Concepts

After completing the paperwork for your first environment, Catalyze will create an organization for you in the [Stratum Dashboard](https://stratum.catalyze.io) and invite the contract holder to the organization. From there, you will be free to manage access to your organization and environments.

## Organization Roles

Organizations have three levels of roles:

- ***Member***
  - Belong to the organization but must be given access to specific environments
  - Cannot manage other users
- ***Admin***
  - Has full access to all environments and can manage member-level users
  - Can invite users and manage their environment access settings
- ***Owner***
  - Full access to environments and user management
  - Assigned ownership of organization
  - Cannot be removed or modified by other users
  - The first account in each organization is an owner account


|Permission|Member|Admin|Owner|
|---|---|---|---|
|  List Users of Org |x  |x  |x  |
| Invite Users to Org | | x  |x   |
| Add/Remove Admins  |   |   |x   |
| Access Authorized Environments |x |   |   |
| Access All Environments  |   |  x|x   |
| Give Members Access to an Environment  |   | x  |  x |

## Environment Access Controls (Available early March 2016)

Currently, environment access is an all or nothing permission in the organization. If an admin or owner gives a member access to an environment, the member has permission to:

- View monitoring UI
- View logging UI
- Push Code to Environment Repositories
- Use all CLI commands related to environments
