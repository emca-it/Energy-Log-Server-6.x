# Logstash - Input "network" #


This plugin read events over a TCP or UDP socket assigns the appropriate tags:

		input {
		        tcp {
		                port => 5514
		                type => "network"
		
		                tags => [ "LAN", "TCP" ]
		        }
		
		        udp {
		                port => 5514
		                type => "network"
		
		                tags => [ "LAN", "UDP" ]
		        }
		}
