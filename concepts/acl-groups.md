---
title: ACL Groups
category: organization
summary: What is an ACL, what is a group?
alias: ["compliant-cloud/articles/organization-access-controls/"]
---
# Groups & ACLs on The Platform

Access to your products, services, and environments is managed via ACLs (Access Control Lists). Datica allows you to manage organization access by creating Groups, which are composed of various ACL rules, and adding individual users within your organization to these groups.

As a rule, you may not assign individual ACLs to a specific individual - they must belong to a group. You can create and delete groups as you see fit. There is one important exception and that is the **Admins** group. You can revoke and grant access to this group, but you may not delete it or modify it.

## Currently Datica supports the following set of ACLs:

**Environment ACLs**
### Base
Can interact and control any component and product of your environment(s), except they do not have VPN access, and the cannot manage users.

### Allow Modifications
This ACL provides users with the ability to edit the services in the environment.

### VPN Access
Can directly interact with your Datica network, running jobs, and services (**Note**: this is only relevant to organizations and environments that have a running VPN).

**Organization ACLs**
### View Billing
This gives users the ability to view organization billing details, including the last 4 digits of a credit card number, the invoice history as well as executed contracts.

### Modify Billing
This gives users the ability to update payment information.

### Create Groups
This ACL gives users the ability to create new groups.

### Update Groups
This ACL gives users the ability to update a given groups ACLs.

### Delete Groups
This ACL gives users the ability to remove groups entirely.

### Manage Group Membership
This ACL gives users the ability to edit the members of a group.

### View Invites
This ACL gives users the ability to view invited users and their associated email.

### Send Invites
This ACL gives users the ability to send out invitations to other users to join that organization.

### Revoke Invites
This ACL gives users the ability to remove invites that have been sent, but not activated.

### Update Organization Information
This ACL gives users the ability to update organization information, such as the organization name.

### Update Organization MFA Required
This ACL gives users the ability to update an organizations Multi-factor Authentication preferences.

A group can have multiple ACLs, and a member of an organization can be in any number of groups. For example, if an operations team only needs access to production servers, but not the ability to deploy new code, a group called "Operations" could be created with only the VPN ACL. 

Likewise, a "Developers" team can be given the ability to deploy code but not access production servers with the Base ACL. Then, if there are any members with cross-team duties needing cross-team access, those members could be added to both groups - effectively giving them both the VPN and Base ACLs.

For more on how to manage your organization and create groups with the dashboard you can go to the [organization guide](/compliant-cloud/articles/organization-addremove-users/).