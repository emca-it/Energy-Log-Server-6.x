# Logstash - Output to Elasticsearch#

This output plugin sends all data to the local Elasticsearch instance and create indexes:	

	output {
		elasticsearch {
		   hosts => [ "127.0.0.1:9200" ]

		   index => "%{type}-%{+YYYY.MM.dd}"

		   user => "logserver"
		   password => "logserver"
		}
	}
