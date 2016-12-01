---
title: Catalyze Network Addresses
category: security
summary: List of Catalyze Networks
---

## Catalyze Stratum Platform IP Addresses

Below is a list of network gateway addresses used by the Catalyze Stratum Platform for outgoing communications. If you need to have traffic whitelisted by a partner or customer for you to send traffic to them, these are the appropriate IPs to give.

| Pod Name | IP Addresses |
|-------|--------|
| Pod02 | 52.0.115.155, 34.193.214.115, 34.193.214.115, 34.192.82.29, 34.193.211.191 |
| CSB01 | 54.67.9.105 |
| Pod04 | 52.55.98.148, 34.193.129.177, 34.193.178.153, 34.193.158.67, 34.193.37.175 |

### How do I choose which IP addresses to whitelist?

Choose the above IP addresses to whitelist based on the Pod URL provided for your Catalyze environment.

***Example:***

pod02123.catalyzeapps.com is part of Pod02 and should use that set of IP addresses.

pod04123.catalyzeapps.com is part of Pod04 and should use that set of IP addresses.

csb01123.catalyzeapps.com is part of CSB01 and should use that set of IP addresses.

### Does all of my traffic flow over these IP addresses?

No. Users will interact with your environment via an Elastic Load Balancer. That IP is not controlled by Catalyze. Traffic that originates from inside your environment, such as from a scheduled job pushing data to a health system, will exit the environment through the IP addresses listed above.
