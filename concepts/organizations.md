---
title: Organizations
category: organization
summary: What is a Datica Organization?
alias: ["compliant-cloud/articles/organization-addremove-users/"]
---

An **Organization** is a group of user accounts, usable across all Datica products. Typically, an organization maps to (and is named after) your company. Organizations are created for you during the provisioning process. It is possible to be a member of multiple organizations.

In [The Platform](https://datica.com/compliant-cloud), each [environment](/compliant-cloud/articles/concepts/environments) belongs to a single organization.

## Groups

Every user in an organization can belong to any number of groups ([more on "groups" and how to manage them](/compliant-cloud/articles/concepts/acl-groups)). Groups, via ACLs, allow their members to access Datica resources and products. You can create and delete groups as you see fit. When a new organization is created an **Admins** group will be created automatically. Typically, an organization only has a few **Admins**.

## ACLs

Groups are [composed of ACLs](/compliant-cloud/articles/concepts/acl-groups), currently Datica supports two ACLs, though more are coming:

* **Base** Can interact and control any component and product of your environment(s), except they do not have VPN access, and the cannot manage users.
* **VPN Access** Can directly interact with your Datica network, runnings jobs, and services (**Note**: this is only relevant to organizations and environments that have a running VPN).

## Managing Your Organization

Organization management tools are located in the [The Platform Dashboard](https://product.datica.com/compliant-cloud) - click on your organization's name in the dropdown.

![org_dropdown](/compliant-cloud/articles/images/organization_dropdown.png)

## Adding Members to Your Organization

New members are added to your organization by sending them an invite, from the management tools.

![org_invite](/compliant-cloud/articles/images/organization_invite.png)

Filling out this form will send an email to the specified address, including instructions on how to join the organization. After the invite is sent, it can be revoked at any time until it is accepted.

![org_invite_pending](/compliant-cloud/articles/images/organization_invite_pending.png)

After it is accepted, it will disappear from this table.

> ***Note:*** The [CLI](/compliant-cloud/articles/cli-stratum) [invites send](/compliant-cloud/cli-reference#invites-send) command can also be used to send an invite.

## Creating and Deleting Groups
If you want to create a new group that has a [different set of ACLs](/compliant-cloud/articles/concepts/acl-groups) click the `Add Group` button. If you want to create a group that has access to everything, you check the "Full Access" checkbox. Note: groups with "Full Access" are implictly granted access to manage users in your organization.

![add_group](/compliant-cloud/articles/images/add_group.png)

Name the group, and select the ACLs you want the group to have.

![make_group](/compliant-cloud/articles/images/make_group.png)

## Removing Members, Changing Groups, and Granting Environment Access

This view is visible to all members of groups that have "Full Access".

In the management tools, there is a list of all members of the organization.

![org_members](/compliant-cloud/articles/images/organization_members.png)

Click on the Edit button to enter the details view for a member.

![org_member_details](/compliant-cloud/articles/images/organization_member_details.png)

To remove a user from a group, click the `X` button by the group name.

To add a user to a group, select the group from the dropdown and click Save.

To remove the member from the organization, click the `Remove User from Organization` button.