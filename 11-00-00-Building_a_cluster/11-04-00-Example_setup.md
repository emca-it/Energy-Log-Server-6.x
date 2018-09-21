Example setup
=============

Example of the Elasticsearch cluster configuration:
- file `/etc/elasticsearch/elasticsearch.yml`:

*`cluster.name: tm-lab`*\
*`node.name: "elk01"`*\
*`node.master: true`*\
*`node.data: true`*\
*`network.host: 127.0.0.1,10.0.0.4`*\
*`http.port: 9200`*\
*`discovery.zen.ping.multicast.enabled: false`*\
*`discovery.zen.ping.unicast.hosts: ["10.0.0.4:9300","10.0.0.5:9300","10.0.0.6:9300"]`*\
*`index.number_of_shards: 3`*\
*`index.number_of_replicas: 1`*

- to start the Elasticsearch cluster execute command:

`# systemctl restart elasticsearch`\
- to check status of the Elstaicsearch cluster execute command:
    - check of the Elasticsearch cluster nodes status via tcp port:\
`# curl -XGET '127.0.0.1:9200/_cat/nodes?v'`\
*`host         	  ip           heap.percent ram.percent load node.role master name`*\
*`10.0.0.4 	 10.0.0.4     18           91 		   0.00 -         -      elk01`*\
*`10.0.0.5 	 10.0.0.5     66           91 		   0.00 d        *      elk02`*\
*`10.0.0.6 	 10.0.0.6     43           86         	   0.65 d        m     elk03`*\
*`10.0.0.7 	 10.0.0.7     45           77         	   0.26 d        m     elk04`*\
    
    - check status of the Elasticsearch cluster via log file:\
    `# tail -f /var/log/elasticsearch/tm-lab.log (cluster.name)`
