---
title: VPN User Management
category: vpn
summary: How can I manage VPN users?
---

# How can I manage VPN access to my environment?

Datica offers a VPN appliance that is fully integrated with our user authentication API. This means that you only need to [create a group](/compliant-cloud/articles/concepts/organizations/#creating-and-deleting-groups) that has "VPN Access", and add members of your organization to that group in order to give them access to your VPN appliance. After adding one of your organization users to such a group they will automatically be able to login to the VPN with their credentials. If you include 2-factor auth they will simply have to append their temporary authorization code to their password each time they login to the VPN.

When Datica creates your VPN we will provide you with a generic configuration that all users can use, simply adjusting for their own credentials.

[See the vpn guide] for more information on how to set up a VPN client connection with Datica.