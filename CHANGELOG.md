# **CHANGELOG** #
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
- Energy Log Server Monitor perf data support with filtering and dashboard
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
