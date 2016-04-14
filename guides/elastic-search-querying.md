---
title: Stratum Elasticsearch Direct Queries
---

# Extracting Messages from Elasticsearch

The guides from [Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/guide/1.x/index.html) will provide the most detail for executing searches with Elasticsearch.  

Using curl and [jq](https://stedolan.github.io/jq/) you can retrieve and filter the syslog messages from Elasticsearch.  The messages are stored as structured data in JSON format.

Elasticsearch can be queried directly by making a request to `https://podhostname/__es`

## Authentication

The Logging dashboard requires authentication to access.  

With curl, you can include the `-u USERNAME` option to specify a username. curl will present a password prompt when using the `-u USERNAME` option.  Or, to avoid re-entering the password repeatedly, use a `.netrc` file in the home directory that stores the user name and password and the `-n` option for curl:

~/.netrc:
```
machine podhostname
  login myusername
  password mypassword
```

Example:

~/.netrc:
```
machine pod09999.catalyzeapps.com
  login jamesbrown
  password ifeelgood
```

## Structuring Your Query

The index and type of the documents are specified in the path of the uri.

The index name is in the format `logstash-YYYY.MM.DD`.  For example, `logstash-2015.11.09`

The type is "syslog"

To return all the records in Elasticsearch from 2015-11-09, the uri would be `https://podhostname/__es/logstash-2015.11.09/syslog/_search`

The request will return a JSON document.  You can pipe the results through `jq` to filter the results and only show the syslog message:

`jq '.hits.hits[] | ._source| .syslog_message'`

The full command would be:

`curl -n -s https://podhostname/__es/logstash-2015.11.09/syslog/_search | jq '.hits.hits[] | ._source| .syslog_message'`

Search parameters can be added to a search by including a json document in the request:

Make a file called `es_params.json` to store the parameters of the request:

es_params.json:
```
{
    "query" : {
        "match" : {
            "syslog_message" : "Error"
        }
    }
}
```
Include the parameters in the request:

`curl -n -s -d @es_params.json https://podhostname/__es/logstash-2015.11.09/syslog/_search | jq '.hits.hits[] | ._source| .syslog_message'`

The results from the request are paginated and by default only 10 results are shown.

Add a `size` query parameter to the uri. Be aware that too many results will sigificantly increase the memory usage of Elasticsearch and negativley impact performance

`curl -n -s -d @es_params.json https://podhostname/__es/logstash-2015.11.09/syslog/_search?size=20 | jq '.hits.hits[] | ._source| .syslog_message'`
