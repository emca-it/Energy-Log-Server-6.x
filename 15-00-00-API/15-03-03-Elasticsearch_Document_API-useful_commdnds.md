# Elasticsearch Document API - useful commands #

- get information about the document:

		# curl -XGET -u login:password '127.0.0.1:9200/twitter/tweet/1?pretty=true'

You should see the output:

	{
	  "_index" : "twitter",
	  "_type" : "tweet",
	  "_id" : "1",
	  "_version" : 1,
	  "found" : true,
	  "_source" : {
	    "user" : "lab1",
	    "post_date" : "2017-08-25T10:00:00",
	    "message" : "testuje Elasticsearch"
	  }
	}

- get the source of the document:

		# curl -XGET -u login:password '127.0.0.1:9200/twitter/tweet/1/_source?pretty=true'

You should see the output:

	{
	  "user" : "lab1",
	  "post_date" : "2017-08-25T10:00:00",
	  "message" : "test of Elasticsearch"
	}

- get information about all documents in the index:

		# curl -XGET -u login:password '127.0.0.1:9200/twitter*/_search?q=*&pretty=true'

You should see the output:

	{
	  "took" : 7,
	  "timed_out" : false,
	  "_shards" : {
	    "total" : 10,
	    "successful" : 10,
	    "failed" : 0
	  },
	  "hits" : {
	    "total" : 3,
	    "max_score" : 1.0,
	    "hits" : [ {
	      "_index" : "twitter",
	      "_type" : "tweet",
	      "_id" : "AV49sTlM8NzerkV9qJfh",
	      "_score" : 1.0,
	      "_source" : {
	        "user" : "lab1",
	        "post_date" : "2017-08-25T10:10:00",
	        "message" : "auto generated ID"
	      }
	    }, {
	      "_index" : "twitter",
	      "_type" : "tweet",
	      "_id" : "1",
	      "_score" : 1.0,
	      "_source" : {
	        "user" : "lab1",
	        "post_date" : "2017-08-25T10:00:00",
	        "message" : "Elasticsearch test"
	      }
	    }, {
	      "_index" : "twitter2",
	      "_type" : "tweet",
	      "_id" : "1",
	      "_score" : 1.0,
	      "_source" : {
	        "user" : "elk01",
	        "post_date" : "2017-09-05T10:00:00",
	        "message" : "manual index created test"
	      }
	    } ]
	  }
	}

- the sum of all documents in a specified index:

		# curl -XGET -u login:password '127.0.0.1:9200/_cat/count/twitter?v'

You should see the output:

		epoch      		timestamp count
		1504281400 	17:56:40     2

- the sum of all document in Elasticsearch database:

		# curl -XGET -u login:password '127.0.0.1:9200/_cat/count?v'

You should see the output:

		epoch      		timestamp count
		1504281518 	17:58:38    493658
