# Example of rules #

# Example of rules #

## Unix - Authentication Fail ##

- index pattern: 

		syslog-*

- Type:

		Frequency

- Alert Method:

		Email

- Any:

		num_events: 4
		timeframe:
		  minutes: 5
		
		filter:
		- query_string:
		    query: "program: (ssh OR sshd OR su OR sudo) AND message: \"Failed password\""

## Windows - Firewall disable or modify ##

- index pattern: 

		beats-*

- Type:

		Any

- Alert Method:

		Email

- Any:

filter:

		- query_string:
		       query: "event_id:(4947 OR 4948 OR 4946 OR 4949 OR 4954 OR 4956 OR 5025)"