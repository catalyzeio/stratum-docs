---
title: ACL Groups
category: organization
summary: What is an ACL, what is a group?
alias: ["compliant-cloud/articles/organization-access-controls/"]
---
Access to your products, services, and environments is managed via ACLs (Access Control Lists). Datica allows you to manage organization access by creating groups, which are composed of various ACL rules, and adding individual users within your organization to these groups. You may not assign inidividual ACLs to an individual - they must belong to a group. You can create and delete groups as you see fit. There is one important exception and that is the **Admins** group. You can revoke and grant access to this group, but you may not delete it or modify it.

Currently Datica supports a limited set of ACLs, including:

* **Base** Can interact and control any component and product of your environment(s), except they do not have VPN access, and the cannot manage users.
* **VPN Access** Can directly interact with your Datica network, runnings jobs, and services (**Note**: this is only relevant to organizations and environments that have a running VPN).

A group can have multiple ACLs, and a member of an organization can be in any number of groups. For example, if an operations team only needs access to production servers, but not the ability to deploy new code, a group called "Operations" could be created with only the VPN ACL. Likewise, a "Developers" team can be given the ability to deploy code but not access production servers with the Base ACL. Then, if there are any members with cross-team duties needing cross-team access, those members could be added to both groups - effectively giving them both the VPN and Base ACLs.

For more on how to manage your organization and create groups with the dashboard you can go to the [organization guide](/compliant-cloud/articles/organization-addremove-users/).
