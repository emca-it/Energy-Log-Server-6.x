# Verificatoin of Energy Log Server GUI service #

To verify of Energy Log Server GUI service you can use following command:

- control the Energy Log Server GUI service via **systemd**:

		# systemctl status kibana

output:

		● kibana.service - Kibana                                                                                                         
		   Loaded: loaded (/etc/systemd/system/kibana.service; disabled; vendor preset: disabled)                                         
		   Active: active (running) since Mon 2018-09-10 13:13:19 CEST; 23h ago                                                           
		 Main PID: 1330 (node)                                                                                                            
		   CGroup: /system.slice/kibana.service                                                                                           
		           └─1330 /usr/share/kibana/bin/../node/bin/node --no-warnings /usr/share/kibana/bin/../src/cli -c /etc/kibana/kibana.yml 

- control the Energy Log Server GUI via **port tcp/http**:

		# curl -XGET '127.0.0.1:5601/'

output:

		  <script>var hashRoute = '/app/kibana';
		  var defaultRoute = '/app/kibana';
		  var hash = window.location.hash;
		  if (hash.length) {
		    window.location = hashRoute + hash;
		  } else {
		    window.location = defaultRoute;
		  }</script>

- Control the Energy Log Server GUI via **log file**:

		# tail -f /var/log/messages