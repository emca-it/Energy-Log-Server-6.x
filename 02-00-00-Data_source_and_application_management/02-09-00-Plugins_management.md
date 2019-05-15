# Plugins management in the Elasticearch #

Base installation of the Energy Log Server contains the 
*elasticsearch-auth* plugin.
You can extend the basic Elasticsearch functionality by installing the custom plugins.

Plugins contain JAR files, but may also contain scripts and config files, and must be installed on every node in the cluster. 

After installation, each node must be restarted before the plugin becomes visible.

The Elasticsearch provides two categories of plugins:


- Core Plugins - it is plugins that are part of the Elasticsearch project. 


- Community contributed - it is plugins that are external to the Elasticsearch project

## Installing Plugins ##

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

## Listing plugins ##

Listing currently loaded plugins

	sudo bin/elasticsearch-plugin list

listing currently available core plugins:

	sudo bin/elasticsearch-plugin list --help

## Removing plugins ##

	sudo bin/elasticsearch-plugin remove [pluginname]

## Updating plugins ##

	sudo bin/elasticsearch-plugin remove [pluginname]
	sudo bin/elasticsearch-plugin install [pluginname]