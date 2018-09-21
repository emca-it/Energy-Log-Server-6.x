# Elasticsearch Document API - Delete Document

- delte a document by ID:

		# curl -XDELETE -u login:password '127.0.0.1:9200/twitter/tweet/1?pretty=true'
		# curl -XDELETE -u login:password '127.0.0.1:9200/twitter/tweet/AV49sTlM8NzerkV9qJfh?pretty=true'

- delete a document using a wildcard:

		# curl -XDELETE -u login:password '127.0.0.1:9200/twitter/tweet/1*?pretty=true'
(parametr:  action.destructive_requires_name must be set to false)

