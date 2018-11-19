# The Grafana instalation #
	
1. To install the Grafana application you should:
	- add necessary repository to operating system:

			[root@logserver-6 ~]# cat /etc/yum.repos.d/grafan.repo
			[grafana]
			name=grafana
			baseurl=https://packagecloud.io/grafana/stable/el/7/$basearch
			repo_gpgcheck=1
			enabled=1
			gpgcheck=1
			gpgkey=https://packagecloud.io/gpg.key https://grafanarel.s3.amazonaws.com/RPM-GPG-KEY-grafana
			sslverify=1
			sslcacert=/etc/pki/tls/certs/ca-bundle.crt
			[root@logserver-6 ~]#


	- install the Grafana with following commands:

			[root@logserver-6 ~]# yum search grafana
			Loaded plugins: fastestmirror
			Loading mirror speeds from cached hostfile
			 * base: ftp.man.szczecin.pl
			 * extras: centos.slaskdatacenter.com
			 * updates: centos.slaskdatacenter.com
			=========================================================================================================== N/S matched: grafana ===========================================================================================================
			grafana.x86_64 : Grafana
			pcp-webapp-grafana.noarch : Grafana web application for Performance Co-Pilot (PCP)
			
			  Name and summary matches only, use "search all" for everything.
			
			[root@logserver-6 ~]# yum install grafana

	- to run application use following commands:

			[root@logserver-6 ~]# systemctl enable grafana-server
			Created symlink from /etc/systemd/system/multi-user.target.wants/grafana-server.service to /usr/lib/systemd/system/grafana-server.service.
			[root@logserver-6 ~]#
			[root@logserver-6 ~]# systemctl start grafana-server
			[root@logserver-6 ~]# systemctl status grafana-server
			● grafana-server.service - Grafana instance
			   Loaded: loaded (/usr/lib/systemd/system/grafana-server.service; enabled; vendor preset: disabled)
			   Active: active (running) since Thu 2018-10-18 10:41:48 CEST; 5s ago
			     Docs: http://docs.grafana.org
			 Main PID: 1757 (grafana-server)
			   CGroup: /system.slice/grafana-server.service
			           └─1757 /usr/sbin/grafana-server --config=/etc/grafana/grafana.ini --pidfile=/var/run/grafana/grafana-server.pid cfg:default.paths.logs=/var/log/grafana cfg:default.paths.data=/var/lib/grafana cfg:default.paths.plugins=/var...
			
			 [root@logserver-6 ~]#



1. To connect the Grafana application you should:

	- define the default login/password (line 151;154 in config file)

			[root@logserver-6 ~]# cat /etc/grafana/grafana.ini
			.....
			148 #################################### Security ####################################
			149 [security]
			150 # default admin user, created on startup
			151 admin_user = admin
			152
			153 # default admin password, can be changed before first start of grafana,  or in profile settings
			154 admin_password = admin
			155
.....
	- restart *grafana-server* service:

			[root@logserver-6 ~]# systemctl restart grafana-server

	- Login to Grafana user interface using web browser: *http://ip:3000*

![](/media/media/image112.png)
 
	- use login and password that you set in the config file.


1. Use below example to set conection to Elasticsearch server:

	![](/media/media/image113.png)
