# Enabling the Alert Module #

To enabling the alert module you should:

- generate writeback index for Alert service:

		/opt/alert/bin/elastalert-create-index --config /opt/alert/config.yaml

- configure the index pattern for alert*

![](/media/media/image96.PNG)

- start the Alert service:
  
		systemctl start alert
