---
title: Stratum Elasticsearch Direct Queries
category: logging
---

# Extracting Messages from Elasticsearch

The guides from [Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/guide/1.x/index.html) will provide the most detail for executing searches with Elasticsearch.  

Using curl and [jq](https://stedolan.github.io/jq/) you can retrieve and filter the syslog messages from Elasticsearch.  The messages are stored as structured data in JSON format.

Elasticsearch can be queried directly by making a request to `https://podhostname/__es`

## Authentication

The Logging dashboard requires authentication to access.  

After the release of Stratum 2.1 on July 21, the only way to get access to your logging dashboard is via a session token from the Catalyze authentication server. Below is a sample script that will automatically generate a session token and then allow you to supply an Elasticsearch URL as a query:

```
#!/bin/bash

corl () {
	nonce="X-Request-Nonce: `python -c \"import base64, os; print base64.b64encode(os.urandom(32))\"`"
        ts="X-Request-Timestamp: `python -c \"import time; print int(time.time())\"`"
        curl "$@" -H "${nonce}" -H "${ts}" -H 'Content-Type: application/json' -H 'Accept: application/json'
        }

        URL=$1
        ARG1=$2
        ARG2=$3
        ARG3=$4

        export RESPONSE=$(corl https://auth.catalyze.io/auth/signin -XPOST -d '{"identifier": "bob@catalyze.io", "password": "test123456"}')
	      ENCODEDTOKEN=$(python -c "import urllib; import os; import json; print urllib.quote(json.loads(os.environ['RESPONSE'])['sessionToken'])")

        corl -H "Cookie: sessionToken=${ENCODEDTOKEN}" ${URL} ${ARG1} ${ARG2} ${ARG3}
```

Be sure to make this script executable with `chmod +x <script_name>`. All the examples below will have this script saved as *esquery*.

## Structuring Your Query

The index and type of the documents are specified in the path of the uri.

The index name is in the format `logstash-YYYY.MM.DD`.  For example, `logstash-2015.11.09`

The type is "syslog"

To return all the records in Elasticsearch from 2015-11-09, the command would be:

`./esquery https://podhostname/__es/logstash-2015.11.09/syslog/_search`

The request will return a JSON document.  You can pipe the results through `jq` to filter the results and only show the syslog message:

`jq '.hits.hits[] | ._source| .syslog_message'`

The full command would be:

`./esquery https://podhostname/__es/logstash-2015.11.09/syslog/_search | jq '.hits.hits[] | ._source| .syslog_message'`

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

`./esquery https://podhostname/__es/logstash-2015.11.09/syslog/_search -d @es_params.json | jq '.hits.hits[] | ._source| .syslog_message'`

The results from the request are paginated and by default only 10 results are shown.

Add a `size` query parameter to the URI. Be aware that too many results will significantly increase the memory usage of Elasticsearch and negatively impact performance

`./esquery https://podhostname/__es/logstash-2015.11.09/syslog/_search?size=2 -d @es_params.json | jq '.hits.hits[] | ._source| .syslog_message'`
