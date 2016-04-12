# SSL on MAMP (optional)

(find-replace `5.6.10` with your desired version of PHP in this file)

1. Generate a key with no password for the server:

	    cd /Applications/MAMP/conf/apache
	    openssl req -x509 -newkey rsa:2048 -keyout server.key -out server.crt -days 3650 -nodes

2. Edit `/Applications/MAMP/conf/apache/httpd.conf`, and uncomment the line that says:

	    Include /Applications/MAMP/conf/apache/extra/httpd-ssl.conf

3. Ensure that `httpd-ssl.conf` is loading the right files:

	    SSLCertificateFile "/Applications/MAMP/conf/apache/server.crt"
	    SSLCertificateKeyFile "/Applications/MAMP/conf/apache/server.key"

4. Add SSL configuration to the VirtualHost definition in `httpd-vhosts.conf`:

		<VirtualHost *:80 *:443>
		  DocumentRoot "/Users/mparker17/Projects/Sites/example"
		  ServerName example.dev
		  ErrorLog "logs/example.error.log"
		  CustomLog "logs/example.access.log" common

	      SSLEngine on
	      SSLCertificateFile "/Applications/MAMP/conf/apache/server.crt"
	      SSLCertificateKeyFile "/Applications/MAMP/conf/apache/server.key"]
	      RequestHeader set X-Forwarded-Proto "https"

		  <Directory /Users/mparker17/Projects/Sites/example>
		    AllowOverride All
		    Options All
		    php_admin_value sendmail_path " tee -a /Applications/MAMP/Library/logs/example.mail.log > /dev/null # "
		  </Directory>
		</VirtualHost>

5. Restart Apache.
6. Ensure you can connect to a site over HTTPS.

## Setting up Drupal

1. Install [the Drupal securepages module](https://www.drupal.org/project/securepages).
2. See https://github.com/mparker17/drupal-settings_local_snippets/blob/master/7/securepages.php for information about how to set up `settings.local.php`:
