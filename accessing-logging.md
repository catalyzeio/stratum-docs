# Stratum Environment Logging

Catalyze has implemented a complete ELK stack for Stratum to manage acquiring and storing log data.

ELK stands for:

* Elasticsearch - for storing and searching log data
* Logstash - for sending and receiving log data to Elasticsearch
* Kibana - for a visual interface into log data

## Accessing the logging dashboard
Go to the logging url provided for you.

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


## Customizing your dashboard
Creating custom dashboards in Kibana can be useful if you want to visualize and  view logs in different ways.  To add a panel click the + button on the left side of a row and select a panel type.

![Logging add panel](https://catalyze.box.com/shared/static/y2gkcl76u5sd7xohyqomg3a15tm9jg42.png)

Configure your new panel by filling in the appropriate fields.  For this example we will set up a pie chart with panel type terms.

![Logging pie chart configuration](https://catalyze.box.com/shared/static/8ox33rd50xf7p6d0nsqo6cy4qbi2zipk.png)


More information on updating and customizing your Kibana dashboard can be found [here](http://www.elastic.co/guide/en/elasticsearch/reference/current/analysis.html).

### Saving  your dashboard
To save your custom dashboard click the save icon in the upper right hand corner, give your dashboard a name and hit enter.

![Save custom dashboard](https://catalyze.box.com/shared/static/n4la8a5q6159ambixk3o9tz17d2r8szk.png)

Now that your custom dashboard is saved you can load it by clicking the load icon in the upper right hand corner and selecting the dashboard you want to load.

### Updating the default dashboard
Once you have created a custom dashboard and saved it you can set it to be your default dashboard by clicking the save icon going to advanced and clicking save as home.  
