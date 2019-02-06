# Changing default users for services #

## Change Kibana User ##

Edit file */etc/systemd/system/kibana.service*

		User=newuser
		Group= newuser

Edit */etc/default/kibana*

		user=" newuser "
		group=" newuser "

Add appropriate permission:

		chown newuser: /usr/share/kibana/ /etc/kibana/ -R

## Change Elasticsearch User ##

Edit */usr/lib/tmpfiles.d/elasticsearch.conf* and change user name and group:

		d /var/run/elasticsearch 0755 newuser newuser – 
Create directory:

		mkdir /etc/systemd/system/elasticsearch.service.d/

Edit */etc/systemd/system/elasticsearch.service.d/01-user.conf*

		[Service]
		User=newuser
		Group= newuser

Add appropriate permission:

		chown -R newuser: /var/lib/elasticsearch /usr/share/elasticsearch /etc/elasticsearch /var/log/elasticsearch

## Change Logstash User ##

Create directory:

		mkdir /etc/systemd/system/logstash.service.d

Edit */etc/systemd/system/logstash.service.d/01-user.conf*

		[Service]
		User=newuser
		Group=newuser

Add appropriate permission:

		chown -R newuser: /etc/logstash /var/log/logstash

