

#  Agents module

The Agents module is used for the central management of agents used in Energy Logserver such as Filebeat, Winlogbeat, Packetbeat, Metricbeat.# Agent installation #
All necessary components can be found in the installation folder *${installation_folder}/utils/agents\_bin*.

## Component modules ##

The software consists of two modules:

- Plugin Agents - installation just like any standard Kibana plugin. Before you run the module for the first time, you must add the mapping for the .agents index with the `create_temlate.sh` script

- MasterAgent software - installed on host with agent (like beats);

## Table of configuration parameter for Agent software ##

		|Parameter            |Work type             |Required |Defult value           |Description                                 |
		|---------------------|----------------------|---------|-----------------------|--------------------------------------------|
		|port                 |Agent                 |No       |40000                  |The port on which |the agent is listening   |
		|host                 |Agent                 |No       |Read from system       |The address on which the agent is listening |
		|hostname             |Agent                 |No       |Read from system       |Host name (hostname)                        |
		|autoregister         |Agent                 |No       |24                     |How often the agent's self-registration should take place. Time in hours |
		|metricbeat_path      |Agent                 |No       |./                     |Catalog for meatricbeat                     |
		|filebeat_path        |Agent                 |No       |./                     |Directory for filebeat                      |
		|winlogbeat_path      |Agent                 |No       |./                     |Catalog for winlogbeat                      | 
		|packetbeat_path      |Agent                 |No       |./                     |Catalog for packetbeat                      |
		|custom_list          |Agent                 |No       |Not defiend            |List of files and directories to scan. If a directory is specified, files with the yml extension are registered with it. The file / directory separator is the character ";" |
		|createfile_folder    |Agent                 |No       |Not defiend            |List of directories where files can be created. The catalogs are separated by the symbol ";". These directories are not scanned for file registration.
		|logstash             |Agent                 |No       |https://localhost:8080 |Logstash address for agents                  |
		|https_keystore       |Agent and Masteragent |No       |./lig.keystore         |Path to the SSL certificate file.            |
		|https_keystore_pass  |Agent and Masteragent |No       |admin                  |The password for the certificate file        |
		|connection_timeout   |Agent and Masteragent |No       |5                      |Timeout for https calls given in seconds.    |
		|connection_reconnect |Agent and Masteragent |No       |5                      |Time in seconds that the agent should try to connect to the Logstash if error occur |

## Installing agent software ##

The Agent's software requires the correct installation of a Java Runtime Environment. The software has been tested on Oracle Java 8.
It is recommended to run the Agent as a service in a given operating system.

1. Generating the certificates - EDIT DOMAIN, DOMAIN_IP - use this scripts:

    - create CA certificate and key:

    ```bash
    #!/bin/bash
    DOMAIN="localhost"
    DOMAIN_IP="192.168.0.1"
    COUNTRYNAME="PL"
    STATE="Poland"
    COMPANY="ACME"
    
    openssl genrsa -out rootCA.key 4096
    
    echo -e "${COUNTRYNAME}\n${STATE}\n\n${COMPANY}\n\n\n\n" | openssl       req -x509 -new -nodes -key rootCA.key -sha256 -days 3650 -out rootCA.crt
    
    ```
   - create certificate and key for you domain:
    ```bash
    #!/bin/bash
    DOMAIN="localhost"
    DOMAIN_IP="192.168.0.1"
    COUNTRYNAME="PL"
    STATE="Poland"
    COMPANY="ACME"
    openssl genrsa -out ${DOMAIN}.pre 2048
    openssl pkcs8 -topk8 -inform pem -in ${DOMAIN}.pre -outform pem -out ${DOMAIN}.key -nocrypt
    openssl req -new -sha256 -key ${DOMAIN}.key -subj "/C=${COUNTRYNAME}/ST=${STATE}/O=${COMPANY}/CN=${DOMAIN}" -reqexts SAN -config <(cat /etc/pki/tls/openssl.cnf <(printf "[SAN]\nsubjectAltName=DNS:${DOMAIN},IP:${DOMAIN_IP}")) -out ${DOMAIN}.csr
    
    openssl x509 -req -in ${DOMAIN}.csr -CA rootCA.crt -CAkey rootCA.key -CAcreateserial -out ${DOMAIN}.crt -sha256 -extfile <(printf "[req]\ndefault_bits=2048\ndistinguished_name=req_distinguished_name\nreq_extensions=req_ext\n[req_distinguished_name]\ncountryName=${COUNTRYNAME}\nstateOrProvinceName=${STATE}\norganizationName=${COMPANY}\ncommonName=${DOMAIN}\n[req_ext]\nsubjectAltName=@alt_names\n[alt_names]\nDNS.1=${DOMAIN}\nIP=${DOMAIN_IP}\n") -days 3650 -extensions req_ext
    ```
    - to verify certificate use following command:
    ```bash
    openssl x509 -in ${DOMAIN}.crt -text -noout
    ```
    - creating Java keystore, you will be asked for the password for the certificate key and whether the certificate should be trusted - enter "yes"
    ```bash
    #!/bin/bash
    DOMAIN="localhost"
    DOMAIN_IP="192.168.0.1"
    COUNTRYNAME="PL"
    STATE="Poland"
    COMPANY="ACME"
     keytool -import -file rootCA.crt -alias root -keystore root.jks -storetype jks
    openssl pkcs12 -export -in ${DOMAIN}.crt -inkey ${DOMAIN}.pre -out     node_name.p12 -name "${DOMAIN}" -certfile rootCA.crt
    ```
   
1. Linux host configuration

  - To install the MasterAgent on Linux RH / Centos, the net-tools package must be installed:

    ```bash
    yum install net-tools
    ```

-  Add an exception to the firewall to listen on TCP 8080 and 8081:

    ````bash
    firewall-cmd --permanent --zone public --add-port 8080/tcp
    firewall-cmd --permanent --zone public --add-port 8081/tcp
    ````
    
- Logstash - Configuration

 ```bash
 /bin/cp -rf ./logstash/agents_template.json /etc/logstash/templates.d/
 mkdir /etc/logstash/conf.d/masteragent
 /bin/cp -rf ./logstash/*.conf /etc/logstash/conf.d/masteragent/
 
  /etc/logstash/pipelines.yml:
 - pipeline.id: masteragent
  path.config: "/etc/logstash/conf.d/masteragent/*.conf"

 mkdir /etc/logstash/conf.d/masteragent/ssl
 /bin/cp -rf ./certificates/localhost.key /etc/logstash/conf.d/masteragent/ssl/
 /bin/cp -rf ./certificates/localhost.crt /etc/logstash/conf.d/masteragent/ssl/
 /bin/cp -rf ./certificates/rootCA.crt /etc/logstash/conf.d/masteragent/ssl/
 chown -R logstash:logstash /etc/logstash
 ```

-  Masterbeat - Installation

```bash
 /bin/cp -rf ./agents/linux /opt/agents
 /bin/cp -rf ./agents/linux/agents/linux/MasterBeatAgent.conf /opt/agents/agent.conf
 /bin/cp -rf ./certificates/node_name.p12 /opt/agents/
 /bin/cp -rf ./certificates/root.jks /opt/agents/
 chown -R kibana:kibana /opt/agents
```

- Linux Agent - Installation

```bash
 /bin/cp -rf ./agents/linux/masteragent /opt/masteragent
 /bin/cp -rf ./certificates/node_name.p12 /opt/masteragent
 /bin/cp -rf ./certificates/root.jks /opt/masteragent
 /bin/cp -rf ./agents/linux/masteragent/masteragent.service
 /usr/lib/systemd/system/masteragent.service
 systemctl daemon-reload
 systemctl enable masteragent
 systemctl start masteragent
```

-  Download `MasterBeatAgent.jar` and `agent.conf` files to any desired location;

-  Upload a file with certificates generated by the `keytool` tool to any desired location;

-  Update entries in the `agent.conf` file (the path to the key file, paths to files and directories to be managed, the Logstash address, etc.);

  - 	The agent should always be run with an indication of the working directory in which the `agent.conf` file is located;

  - The Agent is started by the `java -jar MasterBeatAgent.jar` command.

  - Configuration of the `/etc/systemd/system/masteragent.service` file:

      	[Unit]
            	Description=Manage MasterAgent service
            	Wants=network-online.target
            	After=network-online.target
            	
            	[Service]
            	WorkingDirectory=/opt/agent
            	ExecStart=/bin/java -jar MasterBeatAgent.jar
            	User=root
            	Type=simple
            	Restart=on-failure
            	RestartSec=10
            	
            	[Install]
            	WantedBy=multi-user.target
      ```
      
      ```

  -	After creating the file, run the following commands:

  		systemctl daemon-reload
  		systemctl enable  masteragent
  		systemctl start masteragent

3. Windows host configuration

  - Download the latest version of MasterAgnet, which includes: 
    - Agents.jar;
    - agents.exe;
    - agent.conf;
    - agents.xml;
    - lig.keystore;

  - Add an exception to the firewall to listen on TCP port 8081;
  - Add an exception to the firewall to allow connection on TCP port 8080 with remote hosts;
  - Copy Master Agent files to installation directory: "C:\Program Files\MasterAgent"

  - To install the service, start the PowerShell console as an administrator and execute the following commands:

     ```powershell
     New-Service -name masteragent -displayName masteragent -binaryPathName "C:\Program Files\MasterAgent\agents.exe"
     ```
  - Check status of the services
     ```cmd
       cd C:\Program Files\MasterAgent
       agents.exe status
     ```

## The agent management ##

The GUI console is used to manage agents. In the **Agetns** tab, you can find a list of connected agents. There are typical information about agents such as:

- Host name;
- OS name;
- IP Address;
- TCP port;
- Last revision;

![](/media/media/image114.png)

Additionally, for each connected agent, you can find action buttons such as:

- Drop - to remove the agent configuration from the GUI;
- Create - to create new configuration files;
- Show - it is used to display the list of created configuration files;

### Creating a new configuration file ###

![](/media/media/image115.png)

To add a new configuration file press the **Create** button, add a new file **name**, add a new **path** where the file should be saved and the context of the new configuration file.
The new file will be saved with the extension * .yml.

### Editing configuration file ###

To display a list of configuration files available for a given host, press the Show button.

A list of configuration files will be displayed,  and the following options for each of them:

- Show - displays the contents of the file;
- Edit - edit the contents of the file;
- Delete - deletes the file.

To edit the file, select the Edit button, then enter the changes in the content window, after finishing select the Submit button.

