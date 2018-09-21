# Elasticsearch Cluster API #


Example of use:

- information about the cluster state:

		# curl -XGET -u login:password '127.0.0.1:9200/_cluster/health?pretty=true'

- information about the role and load of nodes in the cluster:

		# curl -XGET -u login:password '127.0.0.1:9200/_cat/nodes?v'

- information about the available and used place on the cluster nodes:
		
		# curl -XGET -u login:password '127.0.0.1:9200/_cat/allocation?v'

- information which node is currently in the master role:
	
		# curl -XGET -u login:password '127.0.0.1:9200/_cat/master?v'

- information abut currently performed operations by the cluster:
		
		# curl -XGET -u login:password '127.0.0.1:9200/_cat/pending_tasks?v'

- information on revoceries / transferred indices:
		
		# curl -XGET -u login:password '127.0.0.1:9200/_cat/recovery?v'

- information about shards in a cluster:

		# curl -XGET -u login:password '127.0.0.1:9200/_cat/shards?v'

- detailed inforamtion about the cluster:
	
		# curl -XGET -u login:password '127.0.0.1:9200/_cluster/stats?human&pretty'

- detailed information about the nodes:
			
		# curl -XGET -u login:password '127.0.0.1:9200/_nodes/stats?human&pretty'
