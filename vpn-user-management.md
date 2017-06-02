---
title: VPN User Management
category: vpn
summary: How can I manage VPN users?
---

# How can I manage VPN access to my environment?

Datica offers a VPN appliance that is fully integrated with our user authentication API. This means that you only need to [create a group](/compliant-cloud/articles/concepts/organizations/#creating-and-deleting-groups) that has "VPN Access", and add members of your organization to that group in order to give them access to your VPN appliance. After adding one of your organization users to such a group they will automatically be able to login to the VPN with their credentials. If your account has two factor auth enabled, you will have to append your temporary authorization code to your password each time you login to the VPN. For example, if your password is "P@ssw0rd" and your temporary authorization code is "123456", you will enter "P@ssw0rd123456" as the full password when authenticating with your VPN appliance.

When Datica creates your VPN we will provide you with a generic configuration that all users can use, adjusting for your own credentials.

[See the vpn guide](/compliant-cloud/articles/guides/openvpn-client-setup/) for more information on how to set up a VPN client connection with Datica.
