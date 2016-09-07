---
title: Organizations
category: organization
summary: What is a Catalyze Organization?
alias: ["stratum/articles/organization-access-controls/", "stratum/articles/organization-addremove-users/"]
---

An **Organization** is a group of user accounts, usable across all Catalyze products. Typically, an organization maps to (and is named after) your company. Organizations are created for you during the provisioning process. It is possible to be a member of multiple organizations.

In [Stratum](https://catalyze.io/stratum), each [environment](/stratum/articles/concepts/environments) belongs to a single organization.

## Roles

Every user in an organization has a role.

* **Members** can see the environments that belong to the organization, but can only view the details of and interact with environments that they have been specifically been granted permission on.
* **Admins** can view the details of any of the organization's environments, grant **Members** permission to do the same, and invite new **Members** to the organization.
* **Owners** can do anything an **Admin** can, as well as promote/demote other members of the organization to new roles.

Typically, an organization only has one **Owner**. When a new organization is created for you, the first user invited becomes the first **Owner**.

## Managing Your Organization

Organization management tools are located in the [Stratum Dashboard](https://product.catalyze.io/stratum) - click on your organization's name in the dropdown.

![org_dropdown](/stratum/articles/images/organization_dropdown.png)

## Adding Members to Your Organization

New members are added to your organization by sending them an invite, from the management tools.

![org_invite](/stratum/articles/images/organization_invite.png)

Filling out this form will send an email to the specified address, including instructions on how to join the organization. After the invite is sent, it can be revoked at any time until it is accepted.

![org_invite_pending](/stratum/articles/images/organization_invite_pending.png)

After it is accepted, it will disappear from this table.

> ***Note:*** The [CLI](/stratum/articles/cli-stratum) [invites send](/paas/paas-cli-reference/#invites-send) command can also be used to send an invite.

## Removing Members, Changing Roles, and Granting Environment Access

In the management tools, there is a list of all members of the organization.

![org_members](/stratum/articles/images/organization_members.png)

Click on the Edit button to enter the details view for a member.

![org_member_details](/stratum/articles/images/organization_member_details.png)

To change that member's role, select the new role from the dropdown and click Save.

To remove the member from the organization, click the `X` button under "Remove User."

To grant a **Member** access to specific environments (or revoke previously-granted access), adjust the radio controls as desired. Changes will be saved automatically.

> ***Note:*** These screenshots are how an **Owner** sees the management tools. Lower roles have a simplified view.

## Notes on Specific Environment Access

If a member does not have granted access and their role is not admin or higher, they will not be able to interact with the environment in any way, beyond seeing that it exists. This includes but is not limited to:

* Pushing code
* Accessing logging
* Accessing monitoring
* Opening consoles
* Associating to it
