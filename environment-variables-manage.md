---
title: Managing variables
category: manage
summary: Learn how to manage your variables on The Platform.
---

# Managing Variables

The Platform allows you to define as many variables as you want for each different code service. You can manage variables through the Datica CLI.

## List variables via the CLI
Below is an example command and the type of output you can expect to see:

`datica -E Prod-app01 vars list app01`

Sample Output:

```
CACHE01_URL=redis://cache01.internal:6379
DATABASE_URL=postgres://catalyze:1234567890@db01-01.internal:5432/catalyzeDB
PORT=8080
REDIS_URL=redis://cache01.internal:6379
```

## Set variables via the CLI
Below is an example command:

`datica -E Prod-app01 vars set app01 -v ENV_VAR1="MYVALUE1" -v ENV_VAR2="MYVALUE2"`

For the new variable settings to take effect, you will need to redeploy your service.

`datica -E Prod-app01 redeploy app01`

## Unset variables via the CLI
Below is an example command and the type of output you can expect to see:

`datica -E Prod-app01 vars unset app01 -v ENV_VAR1`

Sample Output:

`Unset.`

For the new variable settings to take effect, you will need to redeploy your service.

`datica -E Prod-app01 redeploy app01`
