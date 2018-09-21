
# Verification of Elasticsearch service #


To verify of Elasticsearch service you can use following command:

- Control of the Elastisearch system service via **systemd**:

		# sysetmctl status elasticsearch
  output:

		  ● elasticsearch.service - Elasticsearch
		   Loaded: loaded (/usr/lib/systemd/system/elasticsearch.service; disabled; vendor preset: disabled)
		   Active: active (running) since Mon 2018-09-10 13:11:40 CEST; 22h ago
		 Docs: http://www.elastic.co
		 Main PID: 1829 (java)
		   CGroup: /system.slice/elasticsearch.service
		   └─1829 /bin/java -Xms4g -Xmx4g -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=75 -XX:+UseCMSInitiatingOccupancyOnly -XX:+AlwaysPreTouch -Xss1m ...


- Control of Elasticsearch instance via **tcp port**:

 		 # curl -XGET '127.0.0.1:9200/'

  output:
			
			{
			  "name" : "dY3RuYs",
			  "cluster_name" : "elasticsearch",
			  "cluster_uuid" : "EHZGAnJkStqlgRImqwzYQQ",
			  "version" : {
			    "number" : "6.2.3",
			    "build_hash" : "c59ff00",
			    "build_date" : "2018-03-13T10:06:29.741383Z",
			    "build_snapshot" : false,
			    "lucene_version" : "7.2.1",
			    "minimum_wire_compatibility_version" : "5.6.0",
			    "minimum_index_compatibility_version" : "5.0.0"
			  },
			  "tagline" : "You Know, for Search"
			}

- Control of Elasticsearch instance via **log file**:

		# tail -f /var/log/elasticsearch/elasticsearch.log

- other control commands via ***curl* application**:


		curl -xGET "http://localhost:9200/_cat/health?v"
		curl -XGET "http://localhost:9200/_cat/nodes?v"
		curl -XGET "http://localhost:9200/_cat/indicates"
