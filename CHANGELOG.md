# **CHANGELOG** #

## Version 6.1.6
### Added
+ **BREAKING CHANGE**: Support of simple upgrade procedure *alert* indices have to be reindexed
+ Alerting module upgraded
+ System indices created automaticly durring install
+ Improved settings for system indices (priority, shard count, automatic replicas)
+ Validate playbooks button when updating alert rule
+ Order of plugins is no longer random
+ Reports plugin now takes roles into consideration when creating and browsing generated reports
+ Object permission lists are now sorted
+ Improved CSV export field list (sorting and bigger size)
+ DevTools enabled/disabled directive added to default kibana.yml
+ Timelion enabled/disabled directive added to default kibana.yml


### CHANGED
- bugfix: CVE-2019-7608
- bugfix: CVE-2019-7609
- bugfix: CVE-2018-3830
- bugfix: filtering logo extension during upload and report generation
- bugfix: improved verification for Create User
- bugfix: report scheduling for AD users
- bugfix: downloading jpeg exports now returns correct response header
- bugfix: could not set risk category to zero
- bugfix: IE11 compability fix when creating new alert
- bugfix: Admin users see all alerts
- bugfix: Error message if you try create new alert but it already exists


## Version 6.1.5 

- **BREAKING CHANGE**: audit index is from now on created with type "doc" and date field "@timestamp". Old index is not compatible and should be deleted before update:
- Turn of audit logging. In Kibana -> Settings and unmark all in "Update Audit Setting" section.
    - Delete the audit index
    - Update elasticsearch-auth
    - Turn on audit logging.
+ Risk Management for Alerts - User can create custo categories for field attributes like Hostname, Hostip, Username. Once the alert is triggred, the result get score amplification calculated from object categories.
+ Alert rule importance - introduction of new value for each alerts that is correlated with objedct category and helps identify 
+ When creating alerts now we have the ability Test the rule before scheduling this
+ Playbook introduction - ability to create simple editible instructions(+scripts) that system oerator should follow when Alert is triggered
+ Verify IP on blacklists - if the Alerrt is triggred for IP, Verify button let us check its reputaion
+ When creating alerts operatos get ability to validate the alert and find most suitable playbook for it. Playbook list is automaticly sorted.
+ User get email notification when Incident is attached to them. New email field in user tab.
+ IP's are correlated towards Bad IP reputation list
+ Introduction of Incidents. Alerts are now turned into Incidents, with assigned operator and its status
+ Regular user can configure own Alerts
+ Netflow, jflow, sflow support
+ Provided interface for running custom, external, AI jobs created in own programming language

- Audit index is from now created with type "doc" and date field "@timestamp"
- Better Radius authentication supoort
- System auditing corrections

### CHANGED

- bugfix: in intelligence module api
- bugfix: fixes in sorting alerts


## Version 6.1.3 ##
### Added ###
- Securing all the endpoints of elasticsearch APIs
- New configuration option: elastfilter.proxytimeout
- Cleaning unnecessary objects in kibana indices
- Upgrade default logstash to 6.6.2
- Mobile App for Energy Logserver that works with : Log Analytics, Energy Logsrver, pure ELK. x-pack is extra paid. Available for Android and ios.
https://play.google.com/store/apps/details?id=com.logserver.mobile

### CHANGED ###
- bugfix: problem with creating scheduled reports
- bugfix: SSO login not working due to more secure java.policy
- bugfix: Performance issue while using non admin account
- bugfix: Java exception while useing elasticsearch-plugin (ES_JAVA_OPTS moved to jvm.options)
- bugfix: default encoding for es2csv changed to utf-8 (csv export with polish characters)
## Version 6.1.2 ##
### Added ###
- Intelligence API
- Kibana API update
- Caching for index list and roles for user to handle the high CPU usage on master node
- Export task as HTML
- Dashboard report as JPEG
- Additional logging in debug mode in elasticsearch-auth plugin
- GC1 used as default Garbage Collector
- NioFS as default Store Type
- Compression for http & transport enabled
- Product Version tab in Config module
- New Agents feature for central beats/agents management
### CHANGED ###
- bugfix: user session timeouts
- bugfix: problem with reports generation using 5601->443 port redirection
- bugfix: problem with removing a large number of objects from Kibana
- bugfix: timepicker on export to csv reports
- bugfix: special chars in passwords
- bugfix: java.policy - binding elasticsearch to 0.0.0.0
- bugfix: service_principal_name - is no longer required directive when configuring work with AD/LDAP

## Version 6.1.1 ##
### Added ###
- Default template with compression only [elasticsearch]
- Secured LDAP/AD password in configuration files [elasticsearch]
### CHANGED ###
- bugfix: filter config - linux-geoip [logstash]
- bugfix: intelligence template

## Version 6.1.0 ##
- Upgrade core to 6.2.4 [elasticsearch,kibana,logstash]
- Support for all beats agents in filters and dashboards
- Providing default Audit and Alert dashboard
- Change in Intelligence Spark data provide - 1:20 speed improvement
- Intelligence not sensitive on data types
- Better Intelligence preview capabilities·
- Intelligence Count & Trend improvement
- Technology specific dashboards : Windows, Linux, Network
- Technology specific alerts : Windows, Linux, Network
- Energy Logserver Monitor perf data support with filtering and dashboard
- UTF-8 support in custom PDF reports

### CHANGED ###
- bugfix: logo/title/comment in reports module now as optional
- bugfix: java.policy·
- bugfix: Alert Status in Alert module
- bugfix: Percentagematch and Metricaggregation rules fix in Alert module
- bugfix: Deleting Alert rule cause Alert Disable

## Version 6.0.2 ##
### Added ###
- SSO onboarded to 6.x stack
- Custom Logo on PDF Reports including title and comment
- Data Table Head - new visualization type·
- Controls Plus - new vizualization type·
- "Count in Time" as type in Intelligence module
- Nasted.fields support in Intelligence module
- GUI for Scheduler module
- support for all beats agents in filters and dashboards
- providing default audit dashboard
### CHANGED ###
- bugfix: Removed ':' from escaped characters - "Boo" message
- bugfix: Missing directories for reports
- bugfix: Removed unessesary files from deps

## Version 6.0.1 ##
### Added ###
- Functional indexess with dots .kibana, .security, .auth
- Login module onboarded to 6.x stack
- License module onboarded to 6.x stack
- Default roles: alert, intelligence, kibana·
- Export to CSV [Task Management] onboarded to 6.x stack
- Export do PDF [Reports] onboarded to 6.x stack
- PDF Scheduler onboarded to 6.x stack
- AD integrations onboarded to 6.x stack· 
