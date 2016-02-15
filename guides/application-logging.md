---
title: Stratum Application Logging Guide
---

# Stratum Application Logging Guide

## Synopsis

The Stratum platform's logging flexibility makes it easy for you to log data from any software framework that can write to standard out or send JSON messages to a socket.
This guide contains examples for Python + PostgresSQL, Node + MongoDB, PHP + MySQL, and  Ruby + PostgresSQL frameworks.

It is important not only to store logs but to understand how to effectively filter and extract metrics from your logs.  Catalyze uses ELK (Elasticsearch, Logstash and Kibana) to capture and visualize both application and database logs. More information can be found [here](https://www.elastic.co/).

## HIPAA Implications
Logging is an essential part of HIPAA compliance, and the following sections of HIPAA apply directly to logging.  

* Section 164.308(a)(5)(ii)(C) “Log-in Monitoring”  calls for monitoring the systems touching patient information for login and access.  The requirement applies to “login attempts” which implies both failed and successful logins.
* Section 164.312(b) “Audit Controls”  broadly covers audit logging and other audit trails on systems that deal with sensitive health information. Review of such audit logs seem to be implied by this requirement.
* Section 164.308(a)(1)(ii)(D) “Information System Activity Review” prescribes review of various records of IT activities such as logs, systems utilization reports,  incident reports and other indications of security relevant activities.

## Pre-requisites
You have a Stratum account with Catalyze. If you don't, you can sign up for a 30-day trial [here](https://stratum-sandbox.catalyzeapps.com/register).

You have a provisioned environment with a deployed application, perhaps following one of these guides:
[Node + MongoDB Guide](https://resources.catalyze.io/paas/paas-guides/node-mongo/), [PHP + MySQL Guide](https://resources.catalyze.io/paas/paas-guides/php-mysql/),  [Rails + Postgres Guide](https://resources.catalyze.io/paas/paas-guides/rails-postgres/) or [Python + Postgres Guide](https://resources.catalyze.io/paas/paas-guides/python-postgres/).

## Shipping your application logs

### Python, PostgresSQL and Django

See [Python + Postgres Guide](https://resources.catalyze.io/paas/paas-guides/python-postgres/).

#### Logging to Standard Out

Add this code snippet to settings.py:

```
#settings.py
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'file': {
            'level': 'DEBUG',
            'class': 'logging.StreamHandler'
        },
    },
}
```

#### Logging Directly to Logstash

Please send in a support ticket and we will enable direct to socket JSON logging in your environment as well as create an environment variable with the socket endpoint address.

Here's an example from the python-logstash module. The values for the 'host' and 'port' should be pulled from the LOGGING_SERVER environment variable.

```
#settings.py
HOST = os.environ['LOGGING_SERVER'].split(":")[0]
PORT = os.environ['LOGGING_SERVER'].split(":")[1]

LOGGING = {
    'version': 1,andrewawagner@gmail.com
        'file': {
            'level': 'DEBUG',
            'class': 'logging.StreamHandler'
        },
        'logstash': {
          'level': 'DEBUG',
          'class': 'logstash.LogstashHandler',
          'host': HOST,
          'port': PORT,
          'version': 1, # Version of logstash event schema. Default value: 0 (for backward compatibility of the library)
          'message_type': 'logstash',  # 'type' field in logstash message. Default value: 'logstash'.
          'fqdn': False, # Fully qualified domain name. Default value: false.
          'tags': ['tag1', 'tag2'], # list of tags. Default: None.
      },
    },
}
```

### Node and MongoDB

See [Node + MongoDB Guide](https://resources.catalyze.io/paas/paas-guides/node-mongo/).

#### Logging to Standard Out

Insert ```console.log() ```whenever you want to log something.  
For example:
```
console.log('I want to log this thing!');
```

In Kibana, the contents of your logged message will be stored in the "message" field of the stored logs. JSON tags will not have any effect.

#### Logging Directly to Logstash

Please send in a support ticket and we will enable direct to socket JSON logging in your environment as well as create an environment variable with the socket endpoint address.

Here is an example using the winston package. The values of ***port*** and ***host*** should be derived from the LOGGING_SERVER environment variable.

```
var winston = require('winston');
require('winston-logstash');

var logger = new (winston.Logger)({
  transports: [
       new (winston.transports.Logstash)({
           port: 1515,
           host: '127.0.0.5',
           max_connect_retries: 1,
           node_name: 'test',
       })
  ]
});

logger.log('info', 'Test message');
logger.info('Hello log with tags',{animal: 'fish', location: {country:'Canada',city: "Quebec"} });
```

### PHP, MySQL and Laravel

See [PHP + MySQL Guide](https://resources.catalyze.io/paas/paas-guides/php-mysql/).

#### Logging to Standard Out

Logging works easily right out of the box with Laravel. To Enable logging that works with Catalyze, you just need to edit the /config/app.php config file.

```
/*
|--------------------------------------------------------------------------
| Logging Configuration
|--------------------------------------------------------------------------
|
| Here you may configure the log settings for your application. Out of
| the box, Laravel uses the Monolog PHP logging library. This gives
| you a variety of powerful log handlers / formatters to utilize.
|
| Available Settings: "single", "daily", "syslog", "errorlog"
|
*/

'log' => 'daily',
```
Change the value from 'daily' to 'syslog'.  Below are some examples of how to log  other information within your Laravel application.
```
Log::info('This is some useful information.');
Log::warning('Something could be going wrong.');
Log::error('Something is really going wrong.');
```
More information on logging using Laravel can be found [here](http://laravel.com/docs/5.0/errors).

In Kibana, the contents of your logged message will be stored in the "message" field of the stored logs. JSON tags will not have any effect.

### Ruby on Rails and PostgresSQL

See [Rails + Postgres Guide](https://resources.catalyze.io/paas/paas-guides/rails-postgres/).

#### Logging to Standard Out

You can create a new logger to standard output:
```
log = Logger.new(STDOUT)
```
Or standard error:
```
log = Logger.new(STERR)
```
You can also log debug messages, info, or warnings.
```
log.debug("Created logger")
log.info("Program started")
log.warn("Nothing to do!")
```

In Kibana, the contents of your logged message will be stored in the "message" field of the stored logs. JSON tags will not have any effect.

#### Logging Directly to Logstash

Please send in a support ticket and we will enable direct to socket JSON logging in your environment as well as create an environment variable with the socket endpoint address.

Here is an example of how you can implement this with Ruby on Rails via the ![logstash-logger]['https://github.com/dwbutler/logstash-logger'] gem:

```
require 'logstash-logger'

## Create logger using environment variables supplied by Catalyze
logger = LogStashLogger.new(type: :tcp, uri: ENV['LOGGING_SERVER'])

## Send messages
logger.error '{"message": "error"}'

logger.info 'test'
```
