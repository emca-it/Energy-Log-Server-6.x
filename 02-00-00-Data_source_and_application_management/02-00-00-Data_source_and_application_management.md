# Data source and application management #

## Data source ##

Where does the data come from?

Energy Log Server is a solution allowing effective data processing
from the IT environment that exists in the organization.

The Elsasticsearch engine allows building a database in witch large
amounts of data are stored in ordered indexes. The Logstash module is
responsible for load data into Indexes, whose function is to collect
data on specific tcp/udp ports, filter them, normalize them and place
them in the appropriate index. Additional plugins, that we can use in
Logstash reinforce the work of the module, increase its efficiency,
enabling the module to quick interpret data and parse it.

Below is an example of several of the many available Logstash plugins:

**exec** - receive output of the shell function as an event;

**imap** - read email from IMAP servers;

**jdbc** - create events based on JDC data;

**jms** - create events from Jms broker;

Both Elasticsearch and Logstash are free Open-Source solutions.

More information about Elasticsearch module can be find at:
[https://github.com/elastic/elasticsearch](https://github.com/elastic/elasticsearch)

List of available Logstash plugins:
[https://github.com/elastic/logstash-docs/tree/master/docs/plugins](https://github.com/elastic/logstash-docs/tree/master/docs/plugins)

## System services ##


For proper operation Energy Log Server requires starting the following system services:

- elasticsearch.service - 
  we can run it with a command:

		systemctl start elasticsearch.service

  we can check its status with a command:
  	
	systemctl status elasticsearch.service

![](/media/media/image86.PNG)

- kibana.service - 
  we can run it with a command:
 	
		systemctl start kibana.service

  we can check its status with a command:

	systemctl status kibana.service

![](/media/media/image87.PNG)

- logstash.service - 
   we can run it with a command:

		systemctl start logstash.service

   we can check its status with a command:

	systemctl status logstash.service

![](/media/media/image88.PNG)
## First configuration steps ##

### Run the instalation ###
To install and configure Energy Log Server on the CentOS Linux system you should:

- copy archive Energy Log Server tar.bz2 to the hosted server;
- extract archive Energy Log Server tar.bz2 contain application:

	cd /root/
	tar xvfj archive.tar.bz2

- go to the application directory and run installation script as a root user:

		cd /root/insatll
		./install.sh

### Installation steps ###

During installation you will be ask about following tasks:

- add firewall exception on ports 22(ssh), 5044, 5514 (Logstash), 5601 (Kibana), 9200 (Elastisearch), 9300 (ES cross-JVM);
- installation of Java environment (Open-JDK), if you use your own Java environment - answer "N";
- installation of Logstash application;
- configuration of Logstash with custom Energy Log Server configuration;
- connect to the Energy Log Server CentOS repository, which provides Python libraries, and some fonts;
- installation of Kibana, the Energy Log Server GUI;
- installation of Python dependencies;
- installation of mail components for Energy Log Server notification;
- installation of data-node of Elasticsearch;
- configuration of Elasticsearch as Data Node;
- configuration of Elasticsearch as Master Node.

### Optional installation steps: ###
Optionally you can:

- install and configure the filebeat agent;
- install and configure the winlogbeat agent;
- configure Energy Log Server perf_data to integrated with the Energy Log Server Monitor;
- configure naemonLogs to integrated with the Naemon;
- configure integration with Active Directory and SSO servers. You can find necessary information in [12-00-00-Integration_with_AD](/12-00-00-Integration_with_AD/12-00-00-Integration_with_AD.md) and [13-00-00-Windows-SSO](/13-00-00-Windows-SSO/13-00-00-Windows-SSO.md);
- install and configure monitoring with Marvel:

		cd /usr/share/elasticsearch
		sudo bin/plugin install license
		sudo bin/plugin install marvel-agent
		systemctl restart elasticsearch

- enable predictive functionality in Intelligence module:

		curl -XPOST 'http://localhost:9200/_aliases' -d '{
		 		"actions" : [
		     	{ "add" : { "index" : "intelligence", "alias" : "predictive" } },
		    	 { "add" : { "index" : "perfdata-linux", "alias" : "predictive" } }
		 		]}'

- generate writeback index for Alert service:
	
		*/opt/alert/bin/elastalert-create-index --config /opt/alert/config.yaml*
## First login ##


If you log in to Energy Log Server for the first time, you must
specify the Index to be searched. We have the option of entering the
name of your index, indicate a specific index from a given day, or
using the asterix (\*) to indicate all of them matching a specific
index pattern. Therefore, to start working with Energy Log Server
application, we log in to it (by default the user:
logserver/password:logserver).

![](/media/media/image3.png)

After logging in to the application click the button "Set up index pattern" to add new index patter in Kibana: 

![](/media/media/image3_js.png)

In the "Index pattern" field enter the name of the index or index pattern (after
confirming that the index or sets of indexes exists) and click "Next step" button.

![](/media/media/image4.png)

In the next step, from  drop down menu select the "Time filter field name", after witch individual event (events) should be sorter. By default the *timestamp* is set, which is the time of occurrence of the event, but depending of the preferences. It may also be the time of the indexing or other selected
based on the fields indicate on the event.

![](/media/media/image4_js.png)

At any time, you can add more indexes or index patters by going to the
main tab select „Management" and next select „Index Patterns".
## Index selection ##

After login into Energy Log Server you will going to „Discover" tab,
where you can interactively explore your data. You have access to 
every document in every index that matches the selected index patterns.

If you want to change selected index, drop down menu with
the name of the current object in the left panel. Clicking on the
object from the expanded list of previously create index patterns,
will change the searched index.

![](/media/media/image6.png)

## Changing default users for services ##

### Change Kibana User ###

Edit file */etc/systemd/system/kibana.service*

		User=newuser
		Group= newuser

Edit */etc/default/kibana*

		user=" newuser "
		group=" newuser "

Add appropriate permission:

		chown newuser: /usr/share/kibana/ /etc/kibana/ -R

### Change Elasticsearch User ###

Edit */usr/lib/tmpfiles.d/elasticsearch.conf* and change user name and group:

		d /var/run/elasticsearch 0755 newuser newuser – 
Create directory:

		mkdir /etc/systemd/system/elasticsearch.service.d/

Edit */etc/systemd/system/elasticsearch.service.d/01-user.conf*

		[Service]
		User=newuser
		Group= newuser

Add appropriate permission:

		chown -R newuser: /var/lib/elasticsearch /usr/share/elasticsearch /etc/elasticsearch /var/log/elasticsearch

### Change Logstash User ###

Create directory:

		mkdir /etc/systemd/system/logstash.service.d

Edit */etc/systemd/system/logstash.service.d/01-user.conf*

		[Service]
		User=newuser
		Group=newuser

Add appropriate permission:

		chown -R newuser: /etc/logstash /var/log/logstash

## Custom installation the Energy Log Server ##

If you need to install Energy Log Server in non-default location, use the following steps.

1. Define the variable INSTALL_PATH if you do not want default paths like "/"

		export INSTALL_PATH="/"

1. Install the firewalld service

		yum install firewalld

1. Configure the firewalld service

		systemctl enable firewalld
		systemctl start firewalld
		firewall-cmd --zone=public --add-port=22/tcp --permanent
		firewall-cmd --zone=public --add-port=443/tcp --permanent
		firewall-cmd --zone=public --add-port=5601/tcp --permanent
		firewall-cmd --zone=public --add-port=9200/tcp --permanent
		firewall-cmd --zone=public --add-port=9300/tcp --permanent
		firewall-cmd --reload

1. Install and enable the epel repository

		yum install epel-release

1. Install the Java OpenJDK

		yum install java-1.8.0-openjdk-headless.x86_64

1. Install the reports dependencies, e.g. for mail and fonts

		yum install fontconfig freetype freetype-devel fontconfig-devel libstdc++ urw-fonts net-tools ImageMagick ghostscript poppler-utils

1. Create the nessesery users acounts

		useradd -M -d ${INSTALL_PATH}/usr/share/kibana -s /sbin/nologin kibana
		useradd -M -d ${INSTALL_PATH}/usr/share/elasticsearch -s /sbin/nologin elasticsearch
		useradd -M -d ${INSTALL_PATH}/opt/alert -s /sbin/nologin alert

1. Remove .gitkeep files from source directory

		find . -name ".gitkeep" -delete

1. Install the Elasticsearch 6.2.4 files

		/bin/cp -rf elasticsearch/elasticsearch-6.2.4/* ${INSTALL_PATH}/

1. Install the Kibana 6.2.4 files

		/bin/cp -rf kibana/kibana-6.2.4/* ${INSTALL_PATH}/

1. Configure the Elasticsearch system dependencies

		/bin/cp -rf system/limits.d/30-elasticsearch.conf /etc/security/limits.d/
		/bin/cp -rf system/sysctl.d/90-elasticsearch.conf /etc/sysctl.d/
		/bin/cp -rf system/sysconfig/elasticsearch /etc/sysconfig/
		/bin/cp -rf system/rsyslog.d/intelligence.conf /etc/rsyslog.d/
		echo -e "RateLimitInterval=0\nRateLimitBurst=0" >> /etc/systemd/journald.conf
		systemctl daemon-reload
		systemctl restart rsyslog.service
		sysctl -p /etc/sysctl.d/90-elasticsearch.conf

1. Configure the SSL Encryption for the Kibana

		mkdir -p ${INSTALL_PATH}/etc/kibana/ssl
		openssl req -x509 -nodes -days 365 -newkey rsa:2048 -sha256 -subj '/CN=LOGSERVER/subjectAltName=LOGSERVER/' -keyout ${INSTALL_PATH}/etc/kibana/ssl/kibana.key -out ${INSTALL_PATH}/etc/kibana/ssl/kibana.crt

1. Install the Elasticsearch-auth plugin

		cp -rf elasticsearch/elasticsearch-auth ${INSTALL_PATH}/usr/share/elasticsearch/plugins/elasticsearch-auth

1. Install the Elasticsearh configuration files

		/bin/cp -rf elasticsearch/*.yml ${INSTALL_PATH}/etc/elasticsearch/

1. Install the Elasticsicsearch system indices

		mkdir -p ${INSTALL_PATH}/var/lib/elasticsearch
		/bin/cp -rf elasticsearch/nodes ${INSTALL_PATH}/var/lib/elasticsearch/

1.  Add necessary permission for the Elasticsearch directories

		chown -R elasticsearch:elasticsearch ${INSTALL_PATH}/usr/share/elasticsearch ${INSTALL_PATH}/etc/elasticsearch ${INSTALL_PATH}/var/lib/elasticsearch ${INSTALL_PATH}/var/log/elasticsearch

1. Install the Kibana plugins 

		cp -rf kibana/plugins/* ${INSTALL_PATH}/usr/share/kibana/plugins/

1. Extrace the node_modules for plugins and remove archive

		tar -xf ${INSTALL_PATH}/usr/share/kibana/plugins/node_modules.tar -C ${INSTALL_PATH}/usr/share/kibana/plugins/
		/bin/rm -rf ${INSTALL_PATH}/usr/share/kibana/plugins/node_modules.tar

1. Install the Kibana reports binaries

		cp -rf kibana/export_plugin/* ${INSTALL_PATH}/usr/share/kibana/bin/

1. Create directory for the Kibana reports

		/bin/cp -rf kibana/optimize ${INSTALL_PATH}/usr/share/kibana/

1. Install the python dependencies for reports

		tar -xf kibana/python.tar -C /usr/lib/python2.7/site-packages/

1. Install the Kibana custom sources

		/bin/cp -rf kibana/src/* ${INSTALL_PATH}/usr/share/kibana/src/

1. Install the Kibana configuration

		/bin/cp -rf kibana/kibana.yml ${INSTALL_PATH}/etc/kibana/kibana.yml

1. Generate the iron secret salt for Kibana

		echo "server.ironsecret: \"$(</dev/urandom tr -dc _A-Z-a-z-0-9 | head -c32)\"" >> ${INSTALL_PATH}/etc/kibana/kibana.yml

1. Remove old cache files

		rm -rf ${INSTALL_PATH}/usr/share/kibana/optimize/bundles/*

1. Install the Alert plugin

		mkdir -p ${INSTALL_PATH}/opt
		/bin/cp -rf alert ${INSTALL_PATH}/opt/alert

1. Install the AI plugin
	
		/bin/cp -rf ai ${INSTALL_PATH}/opt/ai

1. Set the proper permissions

		chown -R elasticsearch:elasticsearch ${INSTALL_PATH}/usr/share/elasticsearch/
		chown -R alert:alert ${INSTALL_PATH}/opt/alert
		chown -R kibana:kibana ${INSTALL_PATH}/usr/share/kibana ${INSTALL_PATH}/opt/ai ${INSTALL_PATH}/opt/alert/rules ${INSTALL_PATH}/var/lib/kibana
		chmod -R 755 ${INSTALL_PATH}/opt/ai
		chmod -R 755 ${INSTALL_PATH}/opt/alert

1. Install service files for the Alert, Kibana and the Elasticsearch

		/bin/cp -rf system/alert.service /usr/lib/systemd/system/alert.service
		/bin/cp -rf kibana/kibana-6.2.4/etc/systemd/system/kibana.service /usr/lib/systemd/system/kibana.service
		/bin/cp -rf elasticsearch/elasticsearch-6.2.4/usr/lib/systemd/system/elasticsearch.service /usr/lib/systemd/system/elasticsearch.service

1. Set property paths in service files ${INSTALL_PATH}

		perl -pi -e 's#/opt#'${INSTALL_PATH}'/opt#g' /usr/lib/systemd/system/alert.service
		perl -pi -e 's#/etc#'${INSTALL_PATH}'/etc#g' /usr/lib/systemd/system/kibana.service
		perl -pi -e 's#/usr#'${INSTALL_PATH}'/usr#g' /usr/lib/systemd/system/kibana.service
		perl -pi -e 's#ES_HOME=#ES_HOME='${INSTALL_PATH}'#g' /usr/lib/systemd/system/elasticsearch.service
		perl -pi -e 's#ES_PATH_CONF=#ES_PATH_CONF='${INSTALL_PATH}'#g' /usr/lib/systemd/system/elasticsearch.service
		perl -pi -e 's#ExecStart=#ExecStart='${INSTALL_PATH}'#g' /usr/lib/systemd/system/elasticsearch.service

1. Enable the system services

		systemctl daemon-reload
		systemctl reenable alert
		systemctl reenable kibana
		systemctl reenable elasticsearch


1. Set location for Elasticsearch data and logs files in configuration file

	- Elasticsearch

			perl -pi -e 's#path.data: #path.data: '${INSTALL_PATH}'#g' ${INSTALL_PATH}/etc/elasticsearch/elasticsearch.yml
			perl -pi -e 's#path.logs: #path.logs: '${INSTALL_PATH}'#g' ${INSTALL_PATH}/etc/elasticsearch/elasticsearch.yml
			perl -pi -e 's#/usr#'${INSTALL_PATH}'/usr#g' ${INSTALL_PATH}/etc/elasticsearch/jvm.options
			perl -pi -e 's#/usr#'${INSTALL_PATH}'/usr#g' /etc/sysconfig/elasticsearch

	- Kibana

			perl -pi -e 's#/etc#'${INSTALL_PATH}'/etc#g' ${INSTALL_PATH}/etc/kibana/kibana.yml
			perl -pi -e 's#/opt#'${INSTALL_PATH}'/opt#g' ${INSTALL_PATH}/etc/kibana/kibana.yml
			perl -pi -e 's#/usr#'${INSTALL_PATH}'/usr#g' ${INSTALL_PATH}/etc/kibana/kibana.yml

	- AI

			perl -pi -e 's#/opt#'${INSTALL_PATH}'/opt#g' ${INSTALL_PATH}/opt/ai/bin/conf.cfg



1. What next ?

	- Upload License file to ${INSTALL_PATH}/usr/share/elasticsearch/directory.

	- Setup cluster in ${INSTALL_PATH}/etc/elasticsearch/elasticsearch.yml
		
			discovery.zen.ping.unicast.hosts: [ "172.10.0.1:9300", "172.10.0.2:9300" ] 


	- Redirect GUI to 443/tcp

				firewall-cmd --zone=public --add-masquerade --permanent
				firewall-cmd --zone=public --add-forward-port=port=443:proto=tcp:toport=5601 --permanent
				firewall-cmd --reload
## Plugins management in the Elasticsearch ##

Base installation of the Energy Log Server contains the 
*elasticsearch-auth* plugin.
You can extend the basic Elasticsearch functionality by installing the custom plugins.

Plugins contain JAR files, but may also contain scripts and config files, and must be installed on every node in the cluster. 

After installation, each node must be restarted before the plugin becomes visible.

The Elasticsearch provides two categories of plugins:


- Core Plugins - it is plugins that are part of the Elasticsearch project. 


- Community contributed - it is plugins that are external to the Elasticsearch project

### Installing Plugins ###

Core Elasticsearch plugins can be installed as follows:

	cd /usr/share/elasticsearch/ 
    bin/elasticsearch-plugin install [plugin_name]

Example:

	bin/elasticsearch-plugin install ingest-geoip

    -> Downloading ingest-geoip from elastic
    [=================================================] 100%  
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    @ WARNING: plugin requires additional permissions @
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    * java.lang.RuntimePermission accessDeclaredMembers
    * java.lang.reflect.ReflectPermission suppressAccessChecks
    See http://docs.oracle.com/javase/8/docs/technotes/guides/security/permissions.html
    for descriptions of what these permissions allow and the associated risks.
    
    Continue with installation? [y/N]y
    -> Installed ingest-geoip
    
Plugins from custom link or filesystem can be installed as follow:

	cd /usr/share/elasticsearch/
	sudo bin/elasticsearch-plugin install [url]

Example:

	sudo bin/elasticsearch-plugin install file:///path/to/plugin.zip
	bin\elasticsearch-plugin install file:///C:/path/to/plugin.zip
	sudo bin/elasticsearch-plugin install http://some.domain/path/to/plugin.zip

### Listing plugins ###

Listing currently loaded plugins

	sudo bin/elasticsearch-plugin list

listing currently available core plugins:

	sudo bin/elasticsearch-plugin list --help

### Removing plugins ###

	sudo bin/elasticsearch-plugin remove [pluginname]

### Updating plugins ###

	sudo bin/elasticsearch-plugin remove [pluginname]
	sudo bin/elasticsearch-plugin install [pluginname]