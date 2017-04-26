# Logzio es2graphite
This is the codebase for Logz.io's es2graphite docker image.
[Docker Hub](https://hub.docker.com/r/logzio/es2graphite/)

## Overview
This docker container will query your Elasticsearch cluster under /_node/stats and send any numerical metric found to graphite.

## Parameters
The following parameters need to be passed as environment variables to the container (-e ...=... -e ...=... ...)

|Name|Description|Mandatory|Default| 
|---|---|---|---|
|ES2GRAPHITE_ELASTICSEARCH_ADDR|Your Elasticsearch cluster address to monitor (preferably, the ip/hostname of one of your client nodes) - the protocol and port are not required|Yes|-|
|ES2GRAPHITE_GRAPHITE|The graphite server the metrics should end up in|Yes|-|
|ES2GRAPHITE_ELASTICSEARCH_PROTOCOL|http or https. Upper or lower case are acceptable. Typos arre knot.|No|http| 
|ES2GRAPHITE_ELASTICSEARCH_PORT|Port number (usually 9200)|No|9200| 
|ES2GRAPHITE_ELASTICSEARCH_USER_NAME|Username for basic auth|No|| 
|ES2GRAPHITE_ELASTICSEARCH_PASSWORD|Password for basic auth|No||
|ES2GRAPHITE_GRAPHITE_PREFIX|The prefix under graphite you want your metrics in. We will add the cluster name, and the node name after that|No|Elasticsearch|
|ES2GRAPHITE_GRAPHITE_PROTOCOL|Graphite protocol. Allowed values are 'pickle' or 'plaintext'|No|pickle|
|ES2GRAPHITE_GRAPHITE_PORT|Graphite port|No|2004|
|ES2GRAPHITE_INTERVAL_SECONDS|The frequency in which Elasticsearch is to be sampled (preferably, the same value as your graphite configured metrics interval)|No|10|
|ES2GRAPHITE_BULK_SIZE|The amount of metrics to be sent in each bulk request|No|50|
|ES2GRAPHITE_MAX_RETRY_BULK|The number of repeated attempts to send a bulk that failed on IOError (originated from graphite)|No|3|


## Path in graphite
This docker container will take the parameter GRAPHITE_PREFIX you supplied (or the default) and add the cluster name, and node name to the path.
For example:
```
Elasticsearch.prefix.MY_ES_CLUSTER.node1.jvm.mem.heap_used_percent
```

## Run Examples
```bash
docker run --restart=always -d --name="es2graphite" \
                        -e ES2GRAPHITE_ELASTICSEARCH_ADDR="es-client.internal.domain.example" \
                        -e ES2GRAPHITE_GRAPHITE="graphite.internal.domain.example" \
                        -e ES2GRAPHITE_ELASTICSEARCH_PROTOCOL="https" \
                        -e ES2GRAPHITE_ELASTICSEARCH_USER_NAME="mahUser" \
                        -e ES2GRAPHITE_ELASTICSEARCH_PASSWORD="mahPassword" 
                        -e ES2GRAPHITE_GRAPHITE_PREFIX="PROD.Elasticsearch" \
                        -e ES2GRAPHITE_INTERVAL_SECONDS=20 \
                        -e ES2GRAPHITE_BULK_SIZE=10 \
                        logzio/es2graphite
```

## Dashboard Example
You can take the dashboard here under Grafana Dashboard Example as a baseline of your dashboard.
Use find replace to:
 - GRAPHITE_PREFIX - The prefix where this docker holds the metrics
 - COLLECTD_GRAPHITE_PREFIX - The prefix where collectd sends the metrics

## Grafana Screenshots
![Dashboard prtscr 1](https://i.imgsafe.org/fe55084226.png)
![Dashboard prtscr 2](https://i.imgsafe.org/fe551c38d3.png)
![Dashboard prtscr 3](https://i.imgsafe.org/fe550e9a68.png)
![Dashboard prtscr 4](https://i.imgsafe.org/fe55159ae8.png)
