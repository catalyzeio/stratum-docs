---
title: Managing variables
category: manage
summary: Learn how to manage your variables on Stratum.
---

# Managing Variables

Stratum allows you to define as many variables as you want for each different code service. You can manage variables through the Catalyze CLI.

## Setup the Catalyze CLI for Managing Variables

If you have not already done this, onstall***[Link]*** the Catalyze CLI and associate***[link]*** to the environment and service that you wish to manage.

`catalzye associate MyProdEnvironmentName app01 -a Prod-app01`

`catalyze associate MyProdEnvironmentName app02 -a Prod-app02`

Keep in mind that ***separate*** associations must be made to the same environment for the different code services.

## List variables via the CLI

Below is an example command and the type of output you can expect to see:

`catalyze -E Prod-app01 vars list`

Sample Output:

```
CACHE01_URL=redis://cache01.internal:6379
DATABASE_URL=postgres://catalyze:1234567890@db01-01.internal:5432/catalyzeDB
PORT=8080
REDIS_URL=redis://cache01.internal:6379
```

## Set variables via the CLI

Below is an example command:

`catalyze -E Prod-app01 vars set -v ENV_VAR1="MYVALUE1" -v ENV_VAR2="MYVALUE2"`

For the new variable settings to take effect, you will need to redeploy your service.

`catalyze -E Prod-app01 app01 redeploy`

## Unset variables via the CLI

Below is an example command and the type of output you can expect to see:

`catalyze -E Prod-app01 vars unset ENV_VAR1`

Sample Output:

`Unset.`

For the new variable settings to take effect, you will need to redeploy your service.

`catalyze -E Prod-app01 app01 redeploy`
