# Integration with AD #


You can configure the Energy Logserver to communicate with Active Directory to authenticate users. 
To integrate with Active Directory, you configure an Active Directory realm and assign Active Directory 
users and groups to the Energy Logserver roles in the role mapping file.

To protect passwords, communications between the Energy Logserver and the LDAP server should be encrypted 
using SSL/TLS. Clients and nodes that connect via SSL/TLS to the LDAP server need to have the LDAP 
server’s certificate or the server’s root CA certificate installed in their keystore or truststore.
## AD configuration ##

The AD configuration should be done in the `/etc/elasticsearch/properties.yml` 
file.

Below is a list of settings to be made in the `properties.yml` file
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

	ldaps:
	        - name: "example1.com"
	          host: "127.0.0.1,127.0.0.2"
	          port: 389 # optional, default 389
	          ssl_enabled: false # optional, default true
	          ssl_trust_all_certs: true # optional, default false
	          bind_dn: "admin@example1.com"
	          bind_password: "password" # generate encrypted password with /usr/share/elasticsearch/pass-encrypter/pass-encrypter.sh
	          search_user_base_DN: "OU=lab,DC=example1,DC=com"
	          user_id_attribute: "uid" # optional, default "uid"
	          search_groups_base_DN: "OU=lab,DC=example1,DC=com"
	          unique_member_attribute: "uniqueMember" # optional, default "uniqueMember"
	          connection_pool_size: 10 # optional, default 30
	          connection_timeout_in_sec: 10 # optional, default 1
	          request_timeout_in_sec: 10 # optional, default 1
	          cache_ttl_in_sec: 60 # optional, default 0 - cache disabled
	          service_principal_name: "esauth@example1.com" # optional, for sso
	          service_principal_name_password : "password" # optional, for sso
	        - name: "example2.com" #DOMAIN 2
	          host: "127.0.0.1,127.0.0.2"
	          port: 389 # optional, default 389
	          ssl_enabled: false # optional, default true
	          ssl_trust_all_certs: true # optional, default false
	          bind_dn: "admin@example2.com"
	          bind_password: "password" # generate encrypted password with /usr/share/elasticsearch/pass-encrypter/pass-encrypter.sh
	          search_user_base_DN: "OU=lab,DC=example2,DC=com"
	          user_id_attribute: "uid" # optional, default "uid"
	          search_groups_base_DN: "OU=lab,DC=example2,DC=com"
	          unique_member_attribute: "uniqueMember" # optional, default "uniqueMember"
	          connection_pool_size: 10 # optional, default 30
	          connection_timeout_in_sec: 10 # optional, default 1
	          request_timeout_in_sec: 10 # optional, default 1
	          cache_ttl_in_sec: 60 # optional, default 0 - cache disabled
	          service_principal_name: "esauth@example2.com" # optional, for sso
	          service_principal_name_password : "password" # optional, for ssl

After completing the LDAP section entry in the `properties.yml` file,
save the changes and restart the service with the command:

	# systemctl restart elasticsearch

## Configure SSL suport for AD authentication ##

Open the certificate manager on the AD server.

![](/media/media/image78_js.png)
 Select the certificate and open it

![](/media/media/image79_js.png)

Select the option of copying to a file in the Details tab

![](/media/media/image80_js.png)

Click the Next button

![](/media/media/image81.png)

Keep the setting as shown below and click Next

![](/media/media/image82.png)

Keep the setting as shown below and click Next.

![](/media/media/image83.png)

Give the name a certificate

![](/media/media/image84.png)

After the certificate is exported, this certificate should be imported
into a trusted certificate file that will be used by the Elasticsearch
plugin.

To import a certificate into a trusted certificate file, a tool called
„keytool.exe" is located in the JDK installation directory.

Use the following command to import a certificate file:

	keytool -import -alias adding_certificate_keystore -file certificate.cer -keystore certificatestore
The values for RED should be changed accordingly.

By doing this, he will ask you to set a password for the trusted
certificate store. Remember this password, because it must be set in
the configuration of the Elasticsearch plugin. The following settings
must be set in the `properties.yml` configuration for
SSL:

	ssl.keystore.file: "<path to the trust certificate store>"
	ssl.keystore.password: "< password to the trust certificate store>"
## Role mapping ##

In the `/etc/elasticsearch/properties.yml` configuration file you can find
a section for configuring role mapping:

	# LDAP ROLE MAPPING FILE`
	# rolemapping.file.path: /etc/elasticsearch/role-mappings.yml

This variable points to the file `/etc/elasticsearch/role-mappings.yml`
Below is the sample content for this file:

```
admin:	
"CN=Admins,OU=lab,DC=dev,DC=it,DC=example,DC=com"
bank:
"CN=security,OU=lab,DC=dev,DC=it,DC=example,DC=com"
```

**Attention. The role you define in the `role.mapping` file must be created in the Energy Logserver.**

How to the mapping mechanism works ?
An AD user log in to Energy Logserver. In the application there is a
admin role, which through the file role-mapping .yml binds to the name
of the admin role to which the Admins container from AD is assigned.
It is enough for the user from the AD account to log in to the
application with the privileges that are assigned to admin role in 
the Energy Logserver. At the same time, if it is the first login in 
the Energy Logserver, an account is created with an entry that informs the
application administrator that is was created by logging in with AD.

Similar, the mechanism will work if we have a role with an arbitrary
name created in Energy Logserver Logistics and connected to the name of the
role-mappings.yml and existing in AD any container.

Below a screenshot of the console on which are marked accounts that
were created by uesrs logging in from AD

![](/media/media/image85_js.png)

If you map roles with from several domains, for example
dev.examloe1.com, dev.example2.com then in User List we will see which
user from which domain with which role logged in Energy Logserver.

## Password encryption ##

For security reason you can provide the encrypted password for Active Directory integration.
To do this use *pass-encrypter.sh* script that is located in the *Utils* directory in installation folder.

1. Installation of *pass-encrypter*

		cp -pr /instalation_folder/elasticsearch/pass-encrypter /usr/share/elasticsearch/

1. Use *pass-encrypter*

		# /usr/share/elasticsearch/pass-encrypter/pass-encrypter.sh
		Enter the string for encryption :
		new_password
		Encrypted string : MTU1MTEwMDcxMzQzMg==1GEG8KUOgyJko0PuT2C4uw==

# Integration with Radius #

To use the Radius protocol, install the latest available version of Energy Logserver.

## Configuration ##

The default configuration file is located at /etc/elasticsearch/properties.yml:

		# Radius opts
		#radius.host: "10.4.3.184"
		#radius.secret: "querty1q2ww2q1"
		#radius.port: 1812

Use appropriate secret based on config file in Radius server. The secret is configured on `clients.conf` in Radius server. 

In this case, since the plugin will try to do Radius auth then client IP address should be the IP address where the Elasticsearch is deployed.

Every user by default at present get the admin role.