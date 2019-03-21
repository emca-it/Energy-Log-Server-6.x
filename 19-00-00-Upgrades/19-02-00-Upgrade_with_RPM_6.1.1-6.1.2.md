# Upgrade the Energy Log Server with RPM packets #

## Data node (every Elasticsearch installation) update ##

1. Currently updateing with RPMs is not supported - refer to manual update section when upgrading data node.

## Client Node update (GUI) ##

1. Stop the Kibana and Alert services:

		systemctl stop kibana
		systemctl stop alert

1. Delete bundles */usr/share/kibana/optimize/bundles/*

		rm -rf /usr/share/kibana/optimize/bundles/*

1. Delete the contents of the directory */usr/share/kibana/plugins*

		rm -rf /usr/share/kibana/plugins/*

1. Run update with following command:

		rpm -Uvh --replacefiles Energy Log Server-log-analytics-client-node-6.1.2-1.x86_64.rpm

1. Give the right permissions to the Kibana directory

		chown -R kibana:kibana /usr/share/kibana

1. Perform the start of Kibana and Alert

		systemctl start kibana
		systemctl start alert
