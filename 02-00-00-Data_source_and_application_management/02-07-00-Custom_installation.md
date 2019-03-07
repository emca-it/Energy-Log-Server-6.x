# Custom installation the Energy Log Server #

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
