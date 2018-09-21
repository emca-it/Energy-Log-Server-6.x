# Elasticsearch Index API  #


***Delete Index***  - to delete *twitter* index you need use the following command:

	# curl -XDELETE -u login:password '127.0.0.1:9200/twitter?pretty=true'

The delete index API can also be applied to more than one index, by either using 
a comma separated list, or on all indice by using _all or * as index:

	# curl -XDELETE -u login:password '127.0.0.1:9200/twitter*?pretty=true'

To allowing to delete indices via wildcards set `action.destructive_requires_name`
setting in the config to `false`.
