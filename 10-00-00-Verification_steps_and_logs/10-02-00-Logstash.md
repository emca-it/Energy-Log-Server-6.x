# Verification of Logstash service #


To verify of Logstash service you can use following command:

- control Logstash service via **systemd**:

		# systemctl status logstash
output:

		logstash.service - logstash
		   Loaded: loaded (/etc/systemd/system/logstash.service; enabled; vendor preset: disabled)
		   Active: active (running) since Wed 2017-07-12 10:30:55 CEST; 1 months 23 days ago
		 Main PID: 87818 (java)
		   CGroup: /system.slice/logstash.service
		          └─87818 /usr/bin/java -XX:+UseParNewGC -XX:+UseConcMarkSweepGC

- control Logstash service via **port tcp**:

		# curl -XGET '127.0.0.1:9600'
output:

		{
		   "host": "skywalker",
		   "version": "4.5.3",
		   "http_address": "127.0.0.1:9600"
		}
- control Logstash service via **log file**:

		# tail -f /var/log/logstash/logstash-plain.log

## Debuging ##

- dynamically update logging levels through the logging API (service restart not needed):

         curl -XPUT 'localhost:9600/_node/logging?pretty' -H 'Content-Type: application/json' -d'
         {
             "logger.logstash.outputs.elasticsearch" : "DEBUG"
         }
         '
- permanent change of logging level (service need to be restarted):
   - edit file */etc/logstash/logstash.yml* and set the following parameter:
  			
			*log.level: debug*

  - restart logstash service:

         	*systemctl restart logstash*

- checking correct syntax of configuration files:

		*/usr/share/logstash/bin/logstash -tf /etc/logstash/conf.d*

- get information about load of the Logstash:

		*# curl -XGET '127.0.0.1:9600/_node/jvm?pretty=true'*

output: 

	 {                                                          
	  "host" : "logserver-test",                               
	  "version" : "5.6.2",                                     
	  "http_address" : "0.0.0.0:9600",                         
	  "id" : "5a440edc-1298-4205-a524-68d0d212cd55",           
	  "name" : "logserver-test",                               
	  "jvm" : {                                                
	    "pid" : 14705,                                         
	    "version" : "1.8.0_161",                               
	    "vm_version" : "1.8.0_161",                            
	    "vm_vendor" : "Oracle Corporation",                    
	    "vm_name" : "Java HotSpot(TM) 64-Bit Server VM",       
	    "start_time_in_millis" : 1536146549243,                
	    "mem" : {                                              
	      "heap_init_in_bytes" : 268435456,                    
	      "heap_max_in_bytes" : 1056309248,                    
	      "non_heap_init_in_bytes" : 2555904,                  
	      "non_heap_max_in_bytes" : 0                          
	    },                                                     
	    "gc_collectors" : [ "ParNew", "ConcurrentMarkSweep" ]  
	  }                                                        