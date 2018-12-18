# Role mapping #

In the `/etc/elasticsearch/elasticsearch.yml` configuration file you can find
a section for configuring role mapping:

	# LDAP ROLE MAPPING FILE`
	# rolemapping.file.path: /etc/elasticsearch/role-mappings.yml

This variable points to the file `/etc/elasticsearch/role-mappings.yml`
Below is the sample content for this file:

admin:	

	"CN=Admins,OU=lab,DC=dev,DC=it,DC=example,DC=com"

bank:

	"CN=security,OU=lab,DC=dev,DC=it,DC=example,DC=com"


How do the mapping mechanism work?
An AD user log in to Energy Log Server. In the application there is a
admin role, which through the file role-mapping.yml binds to the name
of the admin role to which the Admins container from AD is assigned.
It is enough for the user from the AD account to log in to the
application with the privileges that are assigned to admin role in 
the Energy Log Server. At the same time, if it is the first login in 
the Energy Log Server, an account is created with an entry that informs the
application administrator that is was created by logging in with AD.

Similar, the mechanism will work if we have a role with an arbitrary
name created in Energy Log Server Logistics and connected to the name of the
role-mappings.yml and existing in AD any container.

Below is a screenshot of the console on which are marked accounts that
were created by uesrs logging in from AD

![](/media/media/image85_js.png)

If you map roles with from several domains, for example
dev.examloe1.com, dev.example2.com then in User List we will see which
user from which domain with which role logged in Energy Log Server.
