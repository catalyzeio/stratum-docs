---
title: Datica Network Addresses
category: security
summary: List of Datica Networks
---

## Datica Platform IP Addresses

Below is a list of network gateway addresses used by the Datica Platform for outgoing communications. If you need to have traffic whitelisted by a partner or customer for you to send traffic to them, these are the appropriate IPs to give.

| Pod Name | IP Addresses |
|-------|--------|
| Pod02 | 52.0.115.155, 34.192.37.216, 34.193.214.115, 34.192.82.29, 34.193.211.191 |
| Pod03 | 34.200.190.93, 34.200.26.18, 34.203.187.168, 34.202.232.33 |
| Pod04 | 52.55.98.148, 34.193.129.177, 34.193.178.153, 34.193.158.67, 34.193.37.175 |

### How do I choose which IP addresses to whitelist?

Choose the above IP addresses to whitelist based on the Pod URL provided for your Datica environment.

***Example:***

pod02123.catalyzeapps.com is part of Pod02 and should use that set of IP addresses.

pod04123.catalyzeapps.com is part of Pod04 and should use that set of IP addresses.

csb01123.catalyzeapps.com is part of CSB01 and should use that set of IP addresses.

### Does all of my traffic flow over these IP addresses?

No. Users will interact with your environment via an Elastic Load Balancer. That IP is not controlled by Datica. Traffic that originates from inside your environment, such as from a scheduled job pushing data to a health system, will exit the environment through the IP addresses listed above.
