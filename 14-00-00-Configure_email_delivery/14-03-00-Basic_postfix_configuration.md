# Basic *postfix* configuration #

Base configuration of *postfix* application you can make in
`/etc/postfix/main.cfg` configuration file, which must complete 
with the following entry:

- section *# RECEIVING MAIL*

		inet_interfaces = all
		inet_protocols = ipv4

- section *# INTERNET OR INTRANET*

		relayhost = [IP mail server]:25 (port number)

I the netxt step you must complete the canonical file
of *postfix*

At the end you should restart the *postfix*:

	systemctl restart postfix