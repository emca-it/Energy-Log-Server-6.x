# Elsaticsearch Index API - Adding Index #


***Adding Index*** - autormatic method:

	# curl -XPUT -u login:password '127.0.0.1:9200/twitter/tweet/1?pretty=true' -d'{
	    "user" : "elk01",
	    "post_date" : "2017-09-05T10:00:00",
	    "message" : "tests auto index generation"
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

The parameter `action.auto_create_index` must be set on `true`.

***Adding Index*** – manual method:

- settings the number of shards and replicas:

		# curl -XPUT -u login:password '127.0.0.1:9200/twitter2?pretty=true' -d'{
			"settings" : {
			"number_of_shards" : 1,
			"number_of_replicas" : 1
			}
		       }'
	       
You should see the output:

	{
	  "acknowledged" : true
	}

- command for manual index generation:


		# curl -XPUT -u login:password '127.0.0.1:9200/twitter2/tweet/1?pretty=true' -d'{
					"user" : "elk01",
					"post_date" : "2017-09-05T10:00:00",
					"message" : "tests manual index generation"
				}'
		
You should see the output:

	{
	  "_index" : "twitter2",
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

