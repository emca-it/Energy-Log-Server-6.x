# Elasticsearch Search API #

- searching for documents by the string:


		# curl -XPOST -u login:password '127.0.0.1:9200/twitter*/tweet/_search?pretty=true' -d '{
	    			"query": {
	        				"bool" : {
	            					"must" : {
	                					"query_string" : {
	                    						"query" : "test"
	                					}
	            					}
					}		
	   			 }
			}'

- searching for document by the string and filtering:

		# curl -XPOST -u login:password '127.0.0.1:9200/twitter*/tweet/_search?pretty=true' -d'{
    			"query": {
        				"bool" : {
            					"must" : {
                					"query_string" : {
                    						"query" : "testuje"
                						}
            						},
            					"filter" : {
                					"term" : { "user" : "lab1" }
            					}
        				}
    			}
		}'
	
- simple search in a specific field (in this case user) uri query:

			# curl -XGET -u login:password '127.0.0.1:9200/twitter*/_search?q=user:lab1&pretty=true'

- simple search in a specific field:

		# curl -XPOST -u login:password '127.0.0.1:9200/twitter*/_search?pretty=true' -d '{
					 "query" : {
		       				 "term" : { "user" : "lab1" }
		  	  		}
				}'
