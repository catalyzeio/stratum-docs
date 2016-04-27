---
title: Stratum VPN Client Setup Guide
category: vpn
---

# Supported Clients

**OSX 10.9+**
  * Native VPN Client

**Ubuntu Linux**
  * StrongSwan VPN Client 5.1+

# VPN Security

Communications between your client computer and the Stratum environment over the VPN are encrypted and secure. However, your VPN credentials provide access into this protected environment and should be safeguarded very securely. Please make sure that you are following all applicable information security policies, including what we provide at [https://policy.catalyze.io](https://policy.catalyze.io) and [https://hipaa.catalyze.io](https://hipaa.catalyze.io).

# OS X Native Client Setup

On OS X, the VPN can be created via the Native OS X client. In **System Preferences**, open **Network Connections**.

Click the "+" to create a new connection. You will see the screen below pop-up:

![vpn_creation](../images/vpn_name.png)

Ensure that **Interface** is set to "VPN" and that **VPN Type** is "Cisco IPsec". You can name the VPN whatever you wish.

Click ok after setting those values. You will see the following screen:

![vpn_userpass](../images/vpn_userpass.png)

Fill in the **Server Address**, **Account Name**, and **Password** fields with the VPN Gateway IP, VPN Account Name, and VPN Account Password credentials provided by Catalyze.

Finally, click on **Authentication Settings**. You will see the following window:

![vpn_psk](../images/vpn_psk.png)

Choose the **Shared Secret** radio button. In that field, place the VPN Pre-Shared Key supplied to you by Catalyze. Click "Ok".

Now, click the **"Apply"** button for the connection and connect by clicking the **"Connect"** button.

![vpn_userpass](../images/vpn_userpass_highlights.png)

# Ubuntu strongSwan Client Setup

On Linux, Catalyze recommends using strongSwan directly as opposed to using the plugins for NetworkManager or other VPN clients. Those clients often do not properly configure the connection properly. This connection setup was tested on Ubuntu 14.04.

## strongSwan Installation

Most Linux distributions provide strongSwan in their package repositories. Execute the following commands to install strongSwan plus other dependencies.

`sudo apt-get install strongswan strongswan-plugin-unity strongswan-plugin-xauth-generic`

## strongSwan Client Connection

There are two pieces of configuration necessary for strongSwan on Ubuntu to function - the connection configuration and the connection secrets.

To create a connection, execute the following command in a terminal window:

`sudo charon-cmd --host ${VPN_GATEWAY_IP} --identity ${YOUR_VPN_IDENTITY} --profile ikev1-xauth-psk`

You will then be prompted for your Pre-Shared Key and User Password.

Catalyze will provide each VPN user with the VPN Gateway IP, VPN Account Name, VPN Account Password, and VPN Pre-Shared Key.

## Example Connection Command

`sudo charon-cmd --host 55.55.55.55 --identity bob@catalyze.io --profile ikev1-xauth-psk`

# Using the VPN Connection

The VPN connection allows you to connect directly to many of the resources in your environment. Catalyze will provide you with a service map to use for your environment. If you are attempting to connect a database server, you will need to retrieve the connection credentials from your environment variables.

## Example Service Map

  - Postgresql on database-1: 10.255.0.1:5432
  - Postgresql on database-2: 10.255.0.1:5433
  - Redis on cache-1: 10.255.0.1:6379
  - Application on code-1: 10.255.0.1:8080
  - Application on code-2: 10.255.0.1:8081

## Example Postgres Service Connection

In this example I will use **psql** to connect to my environment' database-2 server running Postgresql. This example assumes I have a VPN connection up and running.

**Example Service Map:** Postgresql on database-2: 10.255.0.1:5433

First, I use the CLI to retrieve my database credentials:

`catalyze -E my_stage_env_name vars list`

Next, I look for the database service that I want the credentials for:

`DATABASE_2_URL=postgres://catalyze:abcdefghijklmnopqrstuvwxyz@postgresql-1234567890.internal:5432/catalyzeDB`

The above environment variable provides the user(`catalyze`), the password(`abcdefghijklmnopqrstuvwxyz`), and the db name(`catalyzeDB`). Note that the port is **not** the same - use the port mapping provided by Catalyze.

`psql -h 10.255.0.1 -p 5433 -U catalyze catalyzeDB`

You will be prompted for the catalyze user password.

This drops me onto the database-2 Postgresql shell on the catalyzeDB database. Other database types can be accessed through similar procedures with their respective clients. Other programs such as database visualizers could also be connected.

## Example Redis Service Connection

**Example Service Map:** Redis on cache-1: 10.255.0.1:6379

Redis does not have authentication credentials within the encrypted environment. You can use the `redis-cli` to connect to your Redis instances.

`redis-cli -h 10.255.0.1 -p 6379`

## Example Code Service Connection

**Example Service Map:** Application on code-1 : 10.255.0.1:8080

This exposes your application container without going through the environment's service proxy.

Below is a simple example that will curl the port that the web server is on.

`curl 10.255.0.1:8080`
