# Alert Module #

Energy Log Server allows you to create alerts, i.e. monitoring
queries. These are constant queries that run in the background and
when the conditions specified in the alert are met, the specify action
is taken. 

![](/media/media/image91.PNG)

For example, if you want to know when more than 20
„status:500" responscode from on our homepage appear within an one
hour, then we create an alert that check the number of occurrences of
the „status:500" query for a specific index every 5 minutes. If the
condition we are interested in is met, we send an action in the form
of sending a message to our e-mail address. In the action, you can
also set the launch of any script.
## Enabling the Alert Module ##

To enabling the alert module you should:

- generate writeback index for Alert service:

		/opt/alert/bin/elastalert-create-index --config /opt/alert/config.yaml

- configure the index pattern for alert*

![](/media/media/image96.PNG)

- start the Alert service:
  
		systemctl start alert
## Creating Alerts ##

To create the alert, click the "Alerts" button from the main menu bar.

 ![](/media/media/image93.png)

We will display a page with tree tabs: Create new alerts in „Create
alert rule", manage alerts in „Alert rules List" and check alert
status „Alert Status".

 In the alert creation windows we have an alert creation form:
 
 ![](/media/media/image92.PNG)
 
 - **Name** - the name of the alert, after which we will recognize and
search for it.
- **Index pattern** - a pattern of indexes after which the alert will be
searched.
- **Role** - the role of the user for whom an alert will be available
- **Type** - type of alert
- **Description** - description of the alert.
- **Example** - an example of using a given type of alert. Descriptive
field
- **Alert method** - the action the alert will take if the conditions are
met (sending an email message or executing a command)
- **Any** - additional descriptive field.# List of Alert rules #

The "Alert Rule List" tab contain complete list of previously created 
alert rules:

![](/media/media/image94.png)

In this window, you can activate / deactivate, delete and update alerts 
by clicking on the selected icon with the given alert: ![](/media/media/image63.png).
## Alerts status ##

In the "Alert status" tab, you can check the current alert status: if
it activated, when it started and when it ended, how long it lasted,
how many event sit found and how many times it worked.

![](/media/media/image95.png)

Also, on this tab, you can recover the alert dashboard, by clicking the "Recovery Alert Dashboard" button.# Type of the Alert module rules #

The various RuleType classes, defined in Energy Log Server-Log-Aalytics. 
An instance is held in memory for each rule, passed all of the data returned by querying Elasticsearch 
with a given filter, and generates matches based on that data.

- ***Any*** - The any rule will match everything. Every hit that the query returns will generate an alert.
- ***Blacklist*** - The blacklist rule will check a certain field against a blacklist, and match if it is in the blacklist.
- ***Whitelist*** - Similar to blacklist, this rule will compare a certain field to a whitelist, 
and match if the list does not contain the term.
- ***Change*** - This rule will monitor a certain field and match if that field changes. 
- ***Frequency*** - his rule matches when there are at least a certain number of events in a given time frame. 
- ***Spike*** - This rule matches when the volume of events during a given time period is spike_height times 
larger or smaller than during the previous time period.
- ***Flatline*** - This rule matches when the total number of events is under a given threshold for a time period.
- ***New Term*** - This rule matches when a new value appears in a field that has never been seen before.
- ***Cardinality*** - This rule matches when a the total number of unique values for a certain field within 
a time frame is higher or lower than a threshold.
- ***Metric Aggregation*** - This rule matches when the value of a metric within the calculation window 
is higher or lower than a threshold.
- ***Percentage Match*** - This rule matches when the percentage of document in the match bucket within 
a calculation window is higher or lower than a threshold.
## Example of rules ##

### Unix - Authentication Fail ###

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

### Windows - Firewall disable or modify ###

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