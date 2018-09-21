Configure SSL suport for AD authentication.
-------------------------------------------



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
must be set in the `elasticsearch.yml` configuration for
SSL:

	ssl.keystore.file: "<path to the trust certificate store>"
	ssl.keystore.password: "< password to the trust certificate store>"
