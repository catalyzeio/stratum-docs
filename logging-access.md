---
title: Stratum Environment Logging
---

# Stratum Environment Logging

Catalyze has implemented a complete ELK stack for Stratum to manage acquiring and storing log data.

ELK stands for:

* Elasticsearch - for storing and searching log data
* Logstash - for sending and receiving log data to Elasticsearch
* Kibana - for a visual interface into log data

## Accessing the logging dashboard
Go to the logging URL provided for you.

`https://{catalyze provided domain}/logging/`

For example:

`https://pod01163.catalyze.io/logging/`

 Or you can log into the Catalyze dashboard and click the logging link under your deployed environment. Log in using your Catalyze credentials.

![Logging login prompt](https://catalyze.box.com/shared/static/21gxyjjf8n3v4bo59vbctv17b0ze0nkn.png)

Your dashboard will look something like this.
![Logging Dashboard](https://catalyze.box.com/shared/static/b5cn6i4y9uy02ubj9g67qy5upxxe3y03.png)

## Searching and Filtering
Use the search bar at the top of the page to search for logs.  For example, to filter application logs you can use the query:
```
 syslog_program : supervisord
```
![Logging Query Example](https://catalyze.box.com/shared/static/8obuino907zpdhcivage9awn11zctct1.png)

You can view various time ranges of your logs by highlighting a portion of your events over time bar graph.
![Logging filtering](https://catalyze.box.com/shared/static/hfe832wrjasujv4ktm01cvevs393iy8u.png)

More information on how to filter and search for logs can be found [here](https://www.elastic.co/guide/en/kibana/current/discover.html).

## Direct ElasticSearch Queries

Check out this [guide](/guides/logging-direct-elasticsearch.md)
