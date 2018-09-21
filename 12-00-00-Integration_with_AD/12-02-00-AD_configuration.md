# AD configuration #

The AD configuration should be done in the `/etc/elasticsearch/elasticsearch.yml` 
file.

Below is a list of settings to be made in the `elasticsearch.yml` file
(the commented section in the file in order for the AD settings to
start working, this fragment should be uncommented):


	|**Direcitve**                          		| **Description**               							|
	| ------------------------------------------------------|---------------------------------------------------------------------------------------|
	| # LDAP                                		|                              								|
	| #ldaps:                               		|                               							|
	| # - name: \"example.com\"             		|# domain that is configured    							|
	| # host: \"127.0.0.1,127.0.0.2\"       		|# list of server for this domain							|
	| # port: 389                           		|# optional, default 389 for unencrypted session or 636 for encrypted sessions		|
	|# ssl\_enabled: false                  		|# optional, default true       							|
	|# ssl\_trust\_all\_certs: true         		|# optional, default false      							|
	|# ssl.keystore.file: \"path\"          		|# path to the truststore store 							|
	|# ssl.keystore.password: \"path\"      		|# password to the trusted certificate store  						|
	|# bind\_dn: [[admin\@example.com]      		|# account name administrator   							|
	|# bind\_password: \"password\"         		|# password for the administrator account 						|
	|# search\_user\_base\_DN: \"OU=lab,DC=example,DC=com\" |# search for the DN user tree database 						|
	|# user\_id\_attribute: \"uid           		|# search for a user attribute optional, by default \"uid\"            			|
	|# search\_groups\_base\_DN:\"OU=lab,DC=example,DC=com\"|# group database search. This is a catalog main, after which the groups will be sought.|
	|# unique\_member\_attribute: \"uniqueMember\" 		|# optional, default\"uniqueMember\"							|
	|# connection\_pool\_size: 10                  		|# optional, default 30									|
	|# connection\_timeout\_in\_sec: 10                  	|# optional, default 1									|
	|# request\_timeout\_in\_sec: 10                     	|# optional, default 1									|
	|# cache\_ttl\_in\_sec: 60                           	|# optional, default 0 - cache disabled							|

If we want to configure multiple domains, then in this configuration
file we copy the \# LDAP section below and configure it for the next
domain. 

Below is an example of how an entry for 2 domains should look
like. (It is important to take the interpreter to read these values
​​correctly).

![](/media/media/image77.png)

After completing the LDAP section entry in the `elasticsearch.yml` file,
save the changes and restart the service with the command:

	# systemctl restart elasticsearch
