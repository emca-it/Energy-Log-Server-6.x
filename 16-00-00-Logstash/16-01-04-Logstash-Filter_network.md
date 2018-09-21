# Logstash Filter "network" #

This filter processing an event data with network type:

	filter {
	 if [type] == "network" {
	     grok {
			named_captures_only => true
			match => {
				"message" => [

				# Cisco Firewall
				"%{SYSLOG5424PRI}%{NUMBER:log_sequence#}:%{SPACE}%{IPORHOST:device_ip}: (?:.)?%{CISCOTIMESTAMP:log_data} CET: %%{CISCO_REASON:facility}-%{INT:severity_level}-%{CISCO_REASON:facility_mnemonic}:%{SPACE}%{GREEDYDATA:event_message}",

				# Cisco Routers
				"%{SYSLOG5424PRI}%{NUMBER:log_sequence#}:%{SPACE}%{IPORHOST:device_ip}: (?:.)?%{CISCOTIMESTAMP:log_data} CET: %%{CISCO_REASON:facility}-%{INT:severity_level}-%{CISCO_REASON:facility_mnemonic}:%{SPACE}%{GREEDYDATA:event_message}",

				# Cisco Switches
				"%{SYSLOG5424PRI}%{NUMBER:log_sequence#}:%{SPACE}%{IPORHOST:device_ip}: (?:.)?%{CISCOTIMESTAMP:log_data} CET: %%{CISCO_REASON:facility}-%{INT:severity_level}-%{CISCO_REASON:facility_mnemonic}:%{SPACE}%{GREEDYDATA:event_message}",
				"%{SYSLOG5424PRI}%{NUMBER:log_sequence#}:%{SPACE}(?:.)?%{CISCOTIMESTAMP:log_data} CET: %%{CISCO_REASON:facility}-%{INT:severity_level}-%{CISCO_REASON:facility_mnemonic}:%{SPACE}%{GREEDYDATA:event_message}",

				# HP switches
				"%{SYSLOG5424PRI}%{SPACE}%{CISCOTIMESTAMP:log_data} %{IPORHOST:device_ip} %{CISCO_REASON:facility}:%{SPACE}%{GREEDYDATA:event_message}"
				]

			}
		}

		syslog_pri { }

		if [severity_level] {

		  translate {
		    dictionary_path => "/etc/logstash/dictionaries/cisco_syslog_severity.yml"
		    field => "severity_level"
		    destination => "severity_level_descr"
		  }

		}

		if [facility] {

		  translate {
		    dictionary_path => "/etc/logstash/dictionaries/cisco_syslog_facility.yml"
		    field => "facility"
		    destination => "facility_full_descr"
		  }

		}

		 #ACL 
		 if [event_message] =~ /(\d+.\d+.\d+.\d+)/ {
		  grok {
		    match => {
			"event_message" => [
				"list %{NOTSPACE:[acl][name]} %{WORD:[acl][action]} %{WORD:[acl][proto]} %{IP:[src][ip]}.*%{IP:[dst][ip]}",
				"list %{NOTSPACE:[acl][name]} %{WORD:[acl][action]} %{IP:[src][ip]}",
				"^list %{NOTSPACE:[acl][name]} %{WORD:[acl][action]} %{WORD:[acl][proto]} %{IP:[src][ip]}.*%{IP:[dst][ip]}"
				]
		    }
		  }
		}

		if [src][ip] {

			cidr {
			   address => [ "%{[src][ip]}" ]
			   network => [ "0.0.0.0/32", "10.0.0.0/8", "172.16.0.0/12", "192.168.0.0/16", "fc00::/7", "127.0.0.0/8", "::1/128", "169.254.0.0/16", "fe80::/10","224.0.0.0/4", "ff00::/8","255.255.255.255/32"  ]
			   add_field => { "[src][locality]" => "private" }
			}

			if ![src][locality] {
			   mutate {
			      add_field => { "[src][locality]" => "public" }
			   }
			}
		}


		if [dst][ip] {
			cidr {
			   address => [ "%{[dst][ip]}" ]
			   network => [ "0.0.0.0/32", "10.0.0.0/8", "172.16.0.0/12", "192.168.0.0/16", "fc00::/7", "127.0.0.0/8", "::1/128",
					"169.254.0.0/16", "fe80::/10","224.0.0.0/4", "ff00::/8","255.255.255.255/32" ]
			   add_field => { "[dst][locality]" => "private" }
			}

			if ![dst][locality] {
			   mutate {
			      add_field => { "[dst][locality]" => "public" }
			   }
			}
		}

		# date format
		date {
		  match => [ "log_data",
			"MMM dd HH:mm:ss",
			"MMM  dd HH:mm:ss",
			"MMM dd HH:mm:ss.SSS",
			"MMM  dd HH:mm:ss.SSS",
			"ISO8601"
		  ]
		  target => "log_data"
		}

	 }
	}

