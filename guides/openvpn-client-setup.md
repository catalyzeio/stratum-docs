---
title: The Platform OpenVPN Client Setup Guide
category: vpn
---

# Recommended Clients

**OSX 10.9+**
  * Tunnelblick

**Linux**
  * OpenVPN package

**Windows**
  * OpenVPN GUI

# VPN Security

Communications between your client computer and the The Platform environment over the VPN are encrypted and secure. However, your Datica credentials provide access into this protected environment and should be safeguarded very securely. Please make sure that you are following all applicable information security policies, including what we provide at [https://policy.datica.com](https://policy.datica.com) and [https://hipaa.datica.com](https://hipaa.datica.com).

# OS X OpenVPN Client

Datica recommends using the Tunnelblick client located [here](https://tunnelblick.net/downloads.html). After installation, you should be able to double-click the configuration file provided by Datica to open the tunnel with Tunnelblick.

# Linux OpenVPN Client

On Linux, Datica recommends using the OpenVPN package provided by your official package management utility.

## Ubuntu

`apt install openvpn`

## RedHat/Centos

`yum install openvpn`

## Client Usage

To use the VPN on Linux, execute the following command from the terminal:

`sudo openvpn --config <path to the config file>`

# Windows OpenVPN Clients

Datica recommends using the OpenVPN GUI bundled with the Windows OpenVPN installer. You can download it [here](https://openvpn.net/index.php/open-source/downloads.html).

The README for the OpenVPN GUI is located [here](https://github.com/OpenVPN/openvpn-gui/). Follow the instruction in the README to add the Datica provided configuration file to the appropriate folder.

# Using the VPN

Datica will provide a unique configuration file for each VPN connection. That file contains all of the information to connect to your VPN for each The Platform environment. Add or open that configuration file with your preferred client and enter your Datica credentials to connect.

If your account has two factor auth enabled, you will have to append your temporary authorization code to your password each time you login to the VPN. For example, if your password is "P@ssw0rd" and your temporary authorization code is "123456", you will enter "P@ssw0rd123456" as the full password when authenticating with your VPN appliance.

The VPN connection allows you to connect directly to many of the resources in your environment. Datica will provide you with a service map to use for your environment. If you are attempting to connect a database server, you will need to retrieve the connection credentials from your environment variables.

## Example Service Map

  - Postgresql on database-1: 10.255.0.1:5432
  - Postgresql on database-2: 10.255.0.1:5433
  - Redis on cache-1: 10.255.0.1:6379
  - Application on code-1: 10.255.0.1:8080
  - Application on code-2: 10.255.0.1:8081

## Example Postgres Service Connection

In this example we will use **psql** to connect to our environment' database-2 server running Postgresql. This example assumes we have a VPN connection up and running.

**Example Service Map:** Postgresql on database-2: 10.255.0.1:5433

First, we use the CLI to retrieve our database credentials:

`datica -E my_stage_env_name vars list <service_name>`

Next, we look for the database service that we want the credentials for:

`DATABASE_2_URL=postgres://catalyze:abcdefghijklmnopqrstuvwxyz@postgresql-1234567890.internal:5432/catalyzeDB`

The above environment variable provides the user(`catalyze`), the password(`abcdefghijklmnopqrstuvwxyz`), and the db name(`catalyzeDB`). Note that the port is **not** the same - use the port mapping provided by Datica.

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
