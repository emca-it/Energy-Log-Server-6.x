# Changing password for the system account #

After you change password for one of the system account ( alert, intelligence, logserver, scheduler), you must to do appropriate changes in the application files.

1. Account **Logserver**
		
	- Update */etc/kibana/kibana.yml*:
		
			vi /etc/kibana/kibana.yml
			
			elasticsearch.password: new_logserver_passowrd
			elastfilter.password: "new_logserver_password"


1. Account **Intelligence**

	- Update */opt/ai/bin/conf.cfg*

			vi /opt/ai/bin/conf.cfg
			password=new_intelligence_password

1. Account **Alert**
		
	- Update file /opt/alert/config.yaml

			vi /opt/alert/config.yaml
			es_password: alert

1. Account **Scheduler**
		
	- Update */etc/kibana/kibana.yml*:

			vi /etc/kibana/kibana.yml	
			elastscheduler.password: "new_scheduler_password"
