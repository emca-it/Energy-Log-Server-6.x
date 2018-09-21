# Elasticsearch Index_API useful commands. #

- get information about Replicas and Shards:

		# curl -XGET -u login:password '127.0.0.1:9200/twitter/_settings?pretty=true'
		# curl -XGET -u login:password '127.0.0.1:9200/twitter2/_settings?pretty=true'

- get information about mapping and alias in the index:

		# curl -XGET -u login:password '127.0.0.1:9200/twitter/_mappings?pretty=true'
		# curl -XGET -u login:password '127.0.0.1:9200/twitter/_aliases?pretty=true'

- get all information about the index:

		# curl -XGET -u login:password '127.0.0.1:9200/twitter?pretty=true'

- checking does the index exist:

		# curl -XGET -u login:password '127.0.0.1:9200/twitter?pretty=true'

- close the index:

		# curl -XGET -u login:password '127.0.0.1:9200/twitter/_close?pretty=true'

- open the index:

		# curl -XGET -u login:password '127.0.0.1:9200/twitter/_open?pretty=true'

- get the status of all indexes:

		# curl -XGET -u login:password '127.0.0.1:9200/_cat/indices?v'

- get the status of one specific index:

		# curl -XGET -u login:password '127.0.0.1:9200/_cat/indices/twitter?v'

- display how much memory is used by the indexes:

		# curl -XGET -u login:password '127.0.0.1:9200/_cat/indices?v&h=i,tm&s=tm:desc'

- display details of the shards:

		# curl -XGET -u login:password '127.0.0.1:9200/_cat/shards?v'