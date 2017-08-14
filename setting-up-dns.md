---
title: Setting Up DNS
category: ssl
summary: How do I setup DNS to point to my Datica environment?
---

# How do I setup DNS to point to my Datica environment?
The Platform environments are designed to have CNAMES or ALIAS records pointed at them. We create a public hostname for the environment that functions as the target of your DNS entries. You will receive your public hostname as part of your on-boarding process after we provision your environment. ***Datica Public Hostname:*** `pod0123.catalyzeapps.com`

## CNAME Records
Use CNAMES for pointing specific subdomains at the Datica public hostname. ***CNAME Example:*** `mysite.wxyz.com CNAME pod0123.catalyzeapps.com`

## ALIAS Records
Use ALIAS records for pointing the bare domain at the Datica public hostname. ***ALIAS Example:*** `wxyz.com ALIAS pod0123.catalyzeapps.com`

## Wildcard Redirect
If you wish to redirect non-matched DNS traffic to your Datica public hostname, you can create a wildcard CNAME record with your DNS provider. `*.wxyz.com CNAME pod0123.catalyzeapps.com`
