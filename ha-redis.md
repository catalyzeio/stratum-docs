# How does Stratum Manage HA Redis?

The Stratum Redis HA solution leverages Redis Sentinel to check the health of master and slave nodes. Sentinel provides automatic failover functionality in the event one of the nodes goes down. You can read more about Redis Sentinel here: [Sentinel docs](http://redis.io/topics/sentinel). To leverage this HA solution your client application code will have to connect and talk to the Sentinels to determine which node is the master node. The following will outline to do so.

## Applicable Environment Variables
The following environment variables will be available to your application:

```
REDIS_SENTINEL_URL="redis-sentinel://127.0.0.14:26379"
REDIS_SENTINEL_LIST="127.0.0.14:26379,127.0.0.15:26379,127.0.0.16:26379"
REDIS_SENTINEL_MASTER="redis01"
```

The `REDIS_SENTINEL_LIST` variable is really the key variable. Depending on the redis client, you can pass the list of sentinels to the client and then it automatically determines which instance is the master and that commands get sent to.

For the connection, if needed/accepted by the client, the name of the master is the name, or label,
of the first redis service ("redis_01", for example) also specified in the `REDIS_SENTINEL_MASTER`
environment variable.

## Connection Examples
Redis clients have made integrating Redis Sentinel very easy. In most cases, all you need to do is provide the connection with a list of the Sentinel addresses. The client then takes care of the negotiations to determine the node to send the Redis commands to.

### Node.js
Node packages available to connect to redis sentinels:

* [ioredis](https://www.npmjs.com/package/ioredis#sentinel)

    ```
    var Redis = require('ioredis');

    var addresses = process.env.REDIS_SENTINEL_LIST.split(',');
    var endpoints = new Array(addresses.length)
    addresses.forEach(function(element, index, array) {
        addr = element.split(':');
        endpoints[index] = {host: addr[0], port: addr[1]};
    });

    var redisClient = new Redis({
        sentinels: endpoints,
        name: process.env.REDIS_SENTINEL_MASTER
    });
    ```

* [redis-sentinel](https://www.npmjs.com/package/redis-sentinel) (wrapper around node_redis)

    ```
    var sentinel = require('redis-sentinel');

    var addresses = process.env.REDIS_SENTINEL_LIST.split(',');
    var endpoints = new Array(addresses.length)
    addresses.forEach(function(element, index, array) {
        addr = element.split(':');
        endpoints[index] = {host: addr[0], port: addr[1]};
    });

    var redisClient = sentinel.createClient(endpoints, process.env.REDIS_SENTINEL_MASTER);
    ```

### Ruby
Ruby packages available to connect to redis sentinels:

* [redis-rb](https://github.com/redis/redis-rb#sentinel-support)

    ```
    require "redis"

    sentinel_master = ENV['REDIS_SENTINEL_MASTER']
    addresses = ENV['REDIS_SENTINEL_LIST'].split(',')
    endpoints = Array.new(addresses.length)
    addresses.each_with_index { |addr, index| a = addr.split(':'); endpoints[index] = {:host => a[0], :port => a[1]} }

    redis = Redis.new(:url => "redis://#{sentinel_master}", :sentinels => endpoints, :role => :master)
    ```

### Python
Python packages available to connect to redis sentinels:

* [redis-py](https://github.com/andymccurdy/redis-py#sentinel-support)

    ```
    import os
    from redis.sentinel import Sentinel

    sentinel_master = os.environ['REDIS_SENTINEL_MASTER']
    sentinel_address_list = os.environ['REDIS_SENTINEL_LIST']
    sentinel = Sentinel([tuple(address.split(':')) for address in sentinel_address_list.split(',')])
    master = sentinel.master_for(sentinel_master)
