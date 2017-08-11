---
title: VPN Access
category: vpn
summary: Can I access my The Platform environment via VPN?
---

# Can I access my The Platform environment via VPN?

Yes! Datica offers a VPN appliance that can be added to your environment that provides direct access to your environment's internal network. VPN access is provided through OpenVPN.

# What can I access through the The Platform VPN?

The VPN provides direct access to the internal network. For example, this access allows users to hook up database visualization software or directly connect to the Redis CLI by talking to the exposed ports on the containers that are on the network.

# How does the VPN work?

If a client purchases the VPN appliance for an environment, Datica will provide them with the unique configuration file for their VPN. Once the VPN is connected, interaction with the services will take place over a private IP address.

# What VPN clients are supported?

Recommended Clients:

  **OSX 10.10+**
    - Tunnelblick
  **Ubuntu/RedHat Linux**
    - OpenVPN via the package manager
  **Windows**
    - OpenVPN GUI

# How do I setup my VPN client?

Check out our guide [here](/compliant-cloud/articles/guides/openvpn-client-setup/) for OpenVPN.

Legacy Stronsgwan IPSEC VPNs should follow the instructions [here](/compliant-cloud/articles/guides/vpn-client-setup/)
