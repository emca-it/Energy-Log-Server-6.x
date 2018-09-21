# System services #


For proper operation Energy Log Server requires starting the following system services:

- elasticsearch.service - 
  we can run it with a command:

		systemctl start elasticsearch.service

  we can check its status with a command:
  	
	systemctl status elasticsearch.service

![](/media/media/image86.PNG)

- kibana.service - 
  we can run it with a command:
 	
		systemctl start kibana.service

  we can check its status with a command:

	systemctl status kibana.service

![](/media/media/image87.PNG)

- logstash.service - 
   we can run it with a command:

		systemctl start logstash.service

   we can check its status with a command:

	systemctl status logstash.service

![](/media/media/image88.PNG)
