# Troubleshooting #

## Recovery default base indexes ##

If you lost or damage following index:
		
        |Index name		 | Index ID              |
        |----------------|-----------------------|
        | .security      |Pfq6nNXOSSmGhqd2fcxFNg |
        | .taskmanagement|E2Pwp4xxTkSc0gDhsE-vvQ |
        | alert_status   |fkqks4J1QnuqiqYmOFLpsQ |
        | audit          |cSQkDUdiSACo9WlTpc1zrw |
        | alert_error    |9jGh2ZNDRumU0NsB3jtDhA |
        | alert_past     |1UyTN1CPTpqm8eDgG9AYnw |
        | .trustedhost   |AKKfcpsATj6M4B_4VD5vIA |
        | .kibana        |cmN5W7ovQpW5kfaQ1xqf2g |
        | .scheduler_job |9G6EEX9CSEWYfoekNcOEMQ |
        | .authconfig    |2M01Phg2T-q-rEb2rbfoVg |
        | .auth          |ypPGuDrFRu-_ep-iYkgepQ |
        | .reportscheduler|mGroDs-bQyaucfY3-smDpg |
        | .authuser      |zXotLpfeRnuzOYkTJpsTaw |
        | alert_silence  |ARTo7ZwdRL67Khw_HAIkmw |
        | .elastfilter   |TtpZrPnrRGWQlWGkTOETzw |
        | alert          |RE6EM4FfR2WTn-JsZIvm5Q |
        | .alertrules    |SzV22qrORHyY9E4kGPQOtg |



You may to recover it from default installation folder with following steps:

1. Stop Logstash instances which load data into cluster

		systemctl stop logstash

1. Disable shard allocation
	
		PUT _cluster/settings
		{
		  "persistent": {
		    "cluster.routing.allocation.enable": "none"
		  }
		}

1. Stop indexing and perform a synced flush

		POST _flush/synced
1. Shutdown all nodes:

		systemctl stop elasticsearch.service
1. Copy appropriate index folder from installation folder to Elasticsearch cluster data node folder (example of .auth folder)

		cp -rf ypPGuDrFRu-_ep-iYkgepQ /var/lib/elasticsearch/nodes/0/indices/

1. Set appropriate permission

		chown -R elasticsearch:elasticsearch /var/lib/elasticsearch/

1. Start all Elasticsearch instance

		systemctl start elasticsearch

1. Wait for yellow state of Elasticsearch cluster and then enable shard allocation

		PUT _cluster/settings
		{
		  "persistent": {
		    "cluster.routing.allocation.enable": "all"
		  }
		}

1. Wait for green state of Elasticsearch cluster and then start the Logstash instances

		systemctl start logstash