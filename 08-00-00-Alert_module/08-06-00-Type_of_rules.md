# Type of the Alert module rules #

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
