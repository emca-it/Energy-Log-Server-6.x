Example of postfix configuration with SSL encryption enabled
------------------------------------------------------------

To configure email delivery with SSL encryption you need to make 
the following changes in the *postfix* configuration files:

- **`/etc/postfix/main.cf`** - file should contain the following 
entries in addition to standard (unchecked entries):
	
		mydestination = $myhostname, localhost.$mydomain, localhost
		myhostname = example.com
		relayhost = [smtp.example.com]:587
		smtp_sasl_auth_enable = yes
		smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
		smtp_sasl_security_options = noanonymous
		smtp_tls_CAfile = /root/certs/cacert.cer
		smtp_use_tls = yes
		smtp_sasl_mechanism_filter = plain, login
		smtp_sasl_tls_security_options = noanonymous
		canonical_maps = hash:/etc/postfix/canonical
		smtp_generic_maps = hash:/etc/postfix/generic
		smtpd_recipient_restrictions = permit_sasl_authenticated

 - **`/etc/postfix/sasl/passwd`** - file should define the data for authorized

			[smtp.example.com\]:587 [[USER@example.com:PASS]](mailto:USER@example.com:PASS)
	
You need to give appropriate permissions:

  		chmod 400 /etc/postfix/sasl_passwd

and map configuration to database: 
  
		postmap /etc/postfix/sasl_passwd

next you need to generate a ca cert file:

  		cat /etc/ssl/certs/Example\_Server\_CA.pem | tee -a etc/postfix/cacert.pem

And finally, you need to restart postfix

  		/etc/init.d/postfix restart
