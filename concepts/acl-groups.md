---
title: ACL Groups
category: acls, groups
summary: What is an ACL, what is a group?
alias: ["compliant-cloud/articles/organization-access-controls/"]
---

Organizational access to your products, services, and environments is managed via ACLs (Access Control Lists). Datica allows you to manage organization access by creating groups, which are composed of various ACL rules, and adding individual users within your organization to these groups. You may not assign inidividual ACLs to an individual, they must belong to a group. The reason for this is that administrating ACLs to individuals can be confusing, so only thinking about them once when you create a group is a good way to collect them into meaningful roles that you can then assign to your organization members. This is also more secure than assigning individual ACLs to organization members, as it would be very easy to overlook granting or revoking access to each member when access revision is needed. It is much simpler to modify groups and have the changes cascade to your organization members. You can create and delete groups as you see fit. There is one important exception and that is the **Admins** group which every organization will always have, this is a special group that Datica uses to grant user management privileges. You can revoke and grant access to this group, but you may not delete it or modify it.

Currently Datica supports only two ACLs (though more are coming):

* **Base** Can interact and control any component and product of your environment(s), except they do not have VPN access, and the cannot manage users.
* **VPN Access** Can directly interact with your Datica network, runnings jobs, and services (**Note**: this is only relevant to organizations and environments that have a running VPN).

You can mix and match these ACLs into different groups as you see fit. Let's say you have some operations folks that you want to give environment access to, but you also want to let them deploy code, etc. You could create and "Operations" group that has both ACLs. Perhaps you only want your developers to have **Base** access as there is no need for them to have VPN/environment access, then you could create a "Developers" group that only has **Base** access.

For more on how to manage your organization and create groups with the dashboard you can go to the [organization guide](/compliant-cloud/articles/organization-addremove-users/).
