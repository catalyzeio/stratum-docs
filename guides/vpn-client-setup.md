---
title: Stratum VPN Client Setup Guide
---

# Supported Clients

**OSX 10.9+**
  * Native VPN Client

**Ubuntu/RedHat Linux**
  * StrongSwan VPN Client 5.1+

# OS X Native Client Setup

On OS X, the VPN can be created via the Native OS X client. In **System Preferences**, open **Network Connections**.

Click the "+" to create a new connection. You will see the screen below pop-up:

![vpn_creation](images/vpn_name.png)

Ensure that **Interface** is set to "VPN" and that **VPN Type** is "Cisco IPsec". You can name the VPN whatever you wish.

Click ok after setting those values. You will see the following screen:

![vpn_userpass](images/vpn_userpass.png)

Fill in the **Server Address**, **Account Name**, and **Password** fields with the VPN Gateway IP, VPN Account Name, and VPN Account Password credentials provided by Catalyze.

Finally, click on **Authentication Settings**. You will see the following window:

![vpn_psk](vpn_psk.png)

Choose the **Shared Secret** radio button. In that field, place the VPN Pre-Shared Key supplied to you by Catalyze. Click "Ok".

Now, click the **"Apply"** button for the connection and connect by clicking the **"Connect"** button.

![vpn_userpass](images/vpn_userpass_highlights.png)

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
