# Elasticsearch Document API - Create_Document #

- create a document with a specify ID:
	 
		# curl -XPUT -u login:password '127.0.0.1:9200/twitter/tweet/1?pretty=true' -d'{
				"user" : "lab1",
				"post_date" : "2017-08-25T10:00:00",
				"message" : "testuje Elasticsearch"
			}'

You should see the output:

	{
	  "_index" : "twitter",
	  "_type" : "tweet",
	  "_id" : "1",
	  "_version" : 1,
	  "_shards" : {
	    "total" : 2,
	    "successful" : 1,
	    "failed" : 0
	  },
	  "created" : true
	}

- creating a document with an automatically generated ID: (note: PUT-> POST):

		# curl -XPOST -u login:password '127.0.0.1:9200/twitter/tweet?pretty=true' -d'{
				"user" : "lab1",
				"post_date" : "2017-08-25T10:10:00",
				"message" : "testuje automatyczne generowanie ID"
				}'
    
You should see the output:

	{
	  "_index" : "twitter",
	  "_type" : "tweet",
	  "_id" : "AV49sTlM8NzerkV9qJfh",
	  "_version" : 1,
	  "_shards" : {
	    "total" : 2,
	    "successful" : 1,
	    "failed" : 0
	  },
	  "created" : true
	}

