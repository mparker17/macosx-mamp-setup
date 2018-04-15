# SSL on MAMP (optional)

(find-replace `5.6.10` with your desired version of PHP in this file)

You will need your local set up so that all Virtualhosts share the same top-level domain; `*.localhost` is recommended.

1. Generate a key with no password for the server:

        cd /Applications/MAMP/conf/apache
        openssl req -x509 -newkey rsa:2048 -keyout server.key -out server.crt -days 3650 -nodes

    ... make sure you set the FQDN as follows:

        Common Name (e.g. server FQDN or YOUR name) []:*.localhost

2. Edit `/Applications/MAMP/conf/apache/httpd.conf`, and ensure it is listening on port 80:

        Listen 80

    ...also uncomment the line that says...

        Include /Applications/MAMP/conf/apache/extra/httpd-ssl.conf

3. Edit `/Applications/MAMP/conf/apache/extra/httpd-ssl.conf`, and ensure it is listening on port 443:

        Listen 443

4. Edit `/Applications/MAMP/conf/apache/extra/httpd-vhosts.conf`, and ensure VirtualHosts are enabled on both ports:

        NameVirtualHost *:80
        NameVirtualHost *:443

    ...now, for each VirtualHost, redirect HTTP connections on port 80 to HTTPS, and ensure each HTTPS VirtualHost has the correct SSL settings and X-Forwarded-Proto header...

        <VirtualHost *:80>
          ServerName example.localhost
          Redirect / https://example.localhost/
        </VirtualHost>
        <VirtualHost *:443>
          DocumentRoot "/Users/mparker17/Projects/Sites/example"
          ServerName example.localhost
          ErrorLog "logs/example.error.log"
          CustomLog "logs/example.access.log" common

          SSLEngine on
          SSLCipherSuite ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP
          SSLCertificateFile "/Applications/MAMP/conf/apache/server.crt"
          SSLCertificateKeyFile "/Applications/MAMP/conf/apache/server.key"
          RequestHeader set X-Forwarded-Proto "https"

          <Directory /Users/mparker17/Projects/Sites/example>
            AllowOverride All
            Options All
            php_admin_value sendmail_path " tee -a /Applications/MAMP/Library/logs/example.mail.log > /dev/null # "
          </Directory>
        </VirtualHost>

5. Restart Apache.
6. Ensure you can connect to a site over HTTPS.

## Setting up Drupal 7

1. Install [the Drupal securepages module](https://www.drupal.org/project/securepages).
2. See https://github.com/mparker17/drupal-settings_local_snippets/blob/master/7/securepages.php for information about how to set up `settings.local.php`:
