# Upgrade the Energy Log Server from RPM packets #

## Client Node update ##

1. Stop the Kibana and Alert services:

		systemctl stop kibana
		systemctl stop alert

1. Delete the contents of the directory */usr/share/kibana/optimize/bundles/*

		rm -rf /usr/share/kibana/optimize/bundles/*

1. Delete the contents of the directory */usr/share/kibana/plugins*

		rm -rf /usr/share/kibana/plugins/*

1. Run update with following command

		rpm -Uvh --replacefiles client-node-6.1.2-1.x86_64.rpm

1. Give the right permissions to the Kibana directory

		chown -R kibana:kibana /usr/share/kibana

1. Perform the start of Kibana and Alert

		systemctl start kibana
		systemctl start alert