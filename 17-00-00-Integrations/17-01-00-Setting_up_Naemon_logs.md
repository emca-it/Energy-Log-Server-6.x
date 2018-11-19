# Setting up Naemon logs #
## Logstash ##
1. In `Energy Log Servernaemon_beat.conf` set up `ELASTICSEARCH_HOST, ES_PORT, FILEBEAT_PORT`
1. Copy `Energy Log Servernaemon_beat.conf` to `/etc/logstash/conf.d`
1. Based on `“FILEBEAT_PORT”` if firewall is running:

		sudo firewall-cmd --zone=public --permanent --add-port=FILEBEAT_PORT/tcp
		sudo firewall-cmd --reload

1. Based on amount of data that elasticsearch will receive you can also choose whether you want index creation to be based on moths or days:

		index => "Energy Log Server-naemon-%{+YYYY.MM}"

	or

		index => "Energy Log Server-naemon-%{+YYYY.MM.dd}"

1. Copy naemon file to `/etc/logstash/patterns` and make sure it is readable by 
*logstash* process

1. Restart logstash configuration e.g.:
 
		sudo kill -1 LOGSTASH_PID

## Elasticsearch ##

1.	Connect to Elasticsearch node via SSH and Install index pattern for naemon logs. Note that if you have a default pattern covering settings section you should delete/modify that in *naemon_template.sh*:

			  "settings": {
			    "number_of_shards": 5,
			    "auto_expand_replicas": "0-1"
			  },

1. Install template by running: *./naemon_template.sh* ### Energy Log Server Monitor
1. On Energy Log Server Monitor host install filebeat (for instance via rpm https://www.elastic.co/downloads/beats/filebeat)
1. In */etc/filebeat/filebeat.yml* add:

		#=========================== Filebeat inputs =============================
		filebeat.config.inputs:
		  enabled: true
		  path: configs/*.yml

1. Create */etc/filebeat/configs* catalog.
1. Copy *naemon_logs.yml* to a newly created catalog.
1. Restart filebeat:

		sudo systemctl restart filebeat # RHEL/CentOS 7
		sudo service filebeat restart # RHEL/CentOS 6

## Kibana ##

At this moment there should be a new index on the Elasticsearch node:

	curl -XGET '127.0.0.1:9200/_cat/indices?v'

Example output:

	health status index                 uuid                   pri rep docs.count docs.deleted store.size pri.store.size
	green  open   Energy Log Server-naemon-2018.11    gO8XRsHiTNm63nI_RVCy8w   1   0      23176            0      8.3mb          8.3mb

If the index has been created, in order to browse and visualize the data, “index pattern” needs to be added in Kibana.
