# Upgrades #

## Updating from 6.1.6

1. Client Node

		bash
		yum install energy-logserver-client-node-6.1.7-1.x86_64.rpm

	In case of an error:

		Transaction check error:
		  file /usr/lib/python2.7/site-packages/urllib3/packages/ssl_match_hostname from install of python-urllib3-1.10.2-7.el7.noarch conflicts with file from package energy-logserver-client-node-6.1.6-1.x86_64

	Remove below directories (this files will be replaced by packages from centOS base and epel repositories):

		bash
		rm -rf /usr/lib/python2.7/site-packages/urllib3 /usr/lib/python2.7/site-packages/urllib3-1.22.dist-info/

1. Data Node

		bash
		yum install energy-logserver-data-node-6.1.7-1.x86_64.rpm

1. Reveiw *.rpmnew files (with vimdiff for example):

		bash
		vimdiff /etc/kibana/kibana.yml /etc/kibana/kibana.yml.rpmnew
		vimdiff /etc/elasticsearch/elasticsearch.yml /etc/elasticsearch/elasticsearch.yml.rpmnew

1. Upload new default template (if you have been using old one already):

		bash
		curl -k -XPUT -H 'Content-Type: application/json' -u logserver:logserver 'http://127.0.0.1:9200/_template/default-base-template-0' -d@/usr/share/elasticsearch/default-base-template-0.json

1. Upload default windows Alert rules:

		bash
		/usr/share/kibana/elasticdump/elasticdump --input=/usr/share/kibana/kibana_objects/SIEM_Windows_RulesAlerts.json --type=data --output="http://logserver:logserver@127.0.0.1:9200/"

1. Restart services:  
	**(Elasticsearch may take long time to start after restart due to great number of shards)**

	- Client node
		
			bash
			systemctl restart kibana alert
			systemctl restart elasticsearch

	- Data node (if you run single node setup this can be omitted)

			systemctl restart elasticsearch

	**Elasticsearch may take long time to start after restart due to great number of shards**

## Updating from 6.1.5
### Changes to alert indices (pre-update)

There were changes to alert* indices in the newest version and this index have to be remade. Before the update you need to do:

1. Stop alert service:  
	
		sudo systemctl stop alert

1. Run this only if you want to keep alert* data:

	- PUT Temporary template:

			bash
			curl -ulogserver:********* "elasticsearch_data_node:9200/_template/alert_old" -H 'Content-Type: application/json' -d'{"order":10,"index_patterns":["alert*-old"],"settings":{"index":{"number_of_shards":1,"auto_expand_replicas":"0-2","number_of_replicas":"0"}},"mappings":{"_default_":{"properties":{"match_body":{"type":"object","enabled":false}}}}}' -X PUT

	- Reindex alert* indices:

			for idx_name in alert alert_error alert_past alert_silence alert_status; do echo ${idx_name}; curl -ulogserver:********* -X POST "elasticsearch_data_node:9200/_reindex" -H 'Content-Type: application/json' -d"
			{
			  "source": {
			    "index": "${idx_name}"
			  },
			  "dest": {
			    "index": "${idx_name}-old"
			  }
			}"; echo; done

	- Delete temporary template:

			curl -ulogserver:********* "elasticsearch_data_node:9200/_template/alert_old" -XDELETE

	- Delete default template if you have one installed (you can recover it after installation):

			curl -ulogserver:********* "elasticsearch_data_node:9200/_template/default-system-indices" -XDELETE

1. Delete old alert* indices:

		bash
		curl -u logserver:********* "elasticsearch_data_node:9200/alert,alert_error,alert_past,alert_silence,alert_status" -XDELETE

1. Proceed with the update. We will come back to alert* indices later.

### Data node update:

1. First, you need to remove an older version from rpm database:

		rpm -e --justdb energy-logserver-data-node-6.1.5-1.x86_64

1. Now install it with yum:

		yum install energy-logserver-data-node-6.1.6-1.x86_64.rpm`

1. After the successful installation restart elasticsearch service (depending on the amount of data you have on the node it might take some time):

		sudo systemctl restart elasticsearch

1. Wait for elasticsearch status to return at least yellow status:  

		curl -sS -XGET --insecure --user logserver:********* "elasticsearch_data_node:9200/_cluster/health?wait_for_status=yellow&pretty"`  

	Example output:

		{
		  "cluster_name" : "logserver_node",
		  "status" : "green",
		  "timed_out" : false,
		  "number_of_nodes" : 2,
		  "number_of_data_nodes" : 2,
		  "active_primary_shards" : 96,
		  "active_shards" : 176,
		  "relocating_shards" : 0,
		  "initializing_shards" : 0,
		  "unassigned_shards" : 0,
		  "delayed_unassigned_shards" : 0,
		  "number_of_pending_tasks" : 0,
		  "number_of_in_flight_fetch" : 0,
		  "task_max_waiting_in_queue_millis" : 0,
		  "active_shards_percent_as_number" : 100.0
		}

### Client node update:

1. For the client node part, all you should do is run:  

		yum update energy-logserver-client-node-6.1.6-1.x86_64.rpm

1. Make sure Kibana bundles are removed before the service restart:  

		/bin/rm -rf /usr/share/kibana/optimize/bundles/*

1. Restart the service:  

		sudo systemctl restart kibana

1. You can access to Kibana again after 5-10 minutes.

### Changes to alert indices (post-update)

1. After a successful update you should have newly created (during elasticsearch restart) alert indices: 

		bash
		curl -ulogserver:********* "elasticsearch_data_node:9200/_cat/indices/alert*"

		green open alert_error       1CAsfsk4R0-rRuCu_05O_g 1 1   0 0    460b    230b
		green open alert_past        _310XBMwTNmvISKrjAKFDw 1 1   0 0    460b    230b
		green open alert             ddILkZRkQKCxjeMWNeRXCQ 1 1   0 0    460b    230b
		green open alert_status      XRvTBQN7QPmXdPhcjugQzQ 1 1   0 0    460b    230b
		green open alert_silence     1bCdy2NaSYe5Ctc52cWD9A 1 1   0 0    460b    230b

1. If you decided to keep old data you can now reindex them from indices you have created earlier:

		bash
		for idx_name in alert alert_error alert_past alert_silence alert_status; do echo ${idx_name}; curl -ulogserver:********* -X POST "elasticsearch_data_node:9200/_reindex" -H 'Content-Type: application/json' -d"
		{
		  "source": {
		    "index": "${idx_name}-old"
		  },
		  "dest": {
		    "index\": "${idx_name}"
		  }
		}"; echo; done

	- If you are sure that recovery was successful you can delete alert*-old:

			curl -u logserver:********* "elasticsearch_data_node:9200/alert-old,alert_silence-old,alert_status-old,alert_error-old" -XDELETE

	- Now you can recover the default template as well

			curl -XPUT -H 'Content-Type: application/json' -u logserver:********* "elasticsearch_data_node:9200/_template/default-base-template-0" -d@/usr/share/elasticsearch/default-base-template-0.json

## Updating from 6.1.3 and older

In this case, you should run the same instruction as to when updating from 6.1.5 and above that, you should also run:

### Changes to audit index (pre-update)

 1. There have been changes to audit index and before the update, you should turn of audit logging:

	- Login to Kibana app with an administrator account.  
	- Go to Config tab.
	- Go to Settings tab.
	- In "Update Audit Setting" deselect all options and click the Update button.  

1. Remove the "audit" index:  

		curl -u logserver:********* "elasticsearch_data_node:9200/audit" -XDELETE

### Data node update

1. You should run everything as described in "Updating from 6.1.5" but **before** running `yum update`:
bash

1. Make a copy of elasticsearch-auth plugin:

		/bin/cp -rf /usr/share/elasticsearch/plugins/elasticsearch-auth/ ~/elasticsearch-auth_copy

1. Remove content of elasticsearch-auth directory:

		rm -f /usr/share/elasticsearch/plugins/elasticsearch-auth/*
