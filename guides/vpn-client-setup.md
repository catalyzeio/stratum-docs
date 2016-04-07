---
title: Stratum VPN Client Setup Guide
---

# Supported Clients

**OSX 10.9+**
  * Native VPN Client

**Ubuntu/RedHat Linux**
  * StrongSwan VPN Client 5.1+

# OS X Native Client Setup

# Linux strongSwan Client Setup

On Linux, Catalyze recommends using strongSwan directly as opposed to using the plugins for NetworkManager or other VPN clients. Those clients often do not properly configure the connection properly.

## strongSwan Installation

Most Linux distributions provide strongSwan in their package repositories. Execute the following commands to install strongSwan plus other dependencies.

**Ubuntu**
`sudo apt-get install strongswan strongswan-plugin-unity strongswan-plugin-xauth-generic`

## strongSwan Client Connection

There are two pieces of configuration necessary for strongSwan on Ubuntu to function - the connection configuration and the connection secrets.

To create a connection, execute the following command in a terminal window:

`sudo charon-cmd --host ${VPN_GATEWAY_IP} --identity ${YOUR_VPN_IDENTITY} --profile ikev1-xauth-psk`

You will then be prompted for your Pre-Shared Key and User Password.

Catalyze will provide the VPN Gateway IP, VPN Identity, Pre-Shared Key, and User Password.
