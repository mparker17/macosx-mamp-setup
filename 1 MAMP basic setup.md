# MAMP basic setup

(find-replace `5.6.10` with your desired version of PHP in this file)

1. Download a new version of MAMP from [official website](http://www.mamp.info/), and run the installer.
    * Remember to customize the installation and uncheck MAMP Pro.
    * The installer will create a /Applications/MAMP folder.
    * Do not start the servers yet: configure them first.
2. Fix MySQL:
    1. Set up the "huge" MySQL configuration template:

            cp /Applications/MAMP/Library/support-files/my-huge.cnf /Applications/MAMP/conf/my.cnf

    2. Edit `/Applications/MAMP/conf/my.cnf`, and change the `max_allowed_packet` line to `1024M`.
    3. If they exist, comment out lines that start with:
        * `log-bin` (most likely)
        * `expire_logs_days` (unlikely)
        * `max_binlog_size` (unlikely)
    4. Restart MySQL.
    5. Run `PURGE BINARY LOGS;`
3. Edit the Apache configuration:
    1. Edit `/Applications/MAMP/conf/apache/httpd.conf`, and uncomment the line that says:

            Include /Applications/MAMP/conf/apache/extra/httpd-vhosts.conf

    2. Edit `/Applications/MAMP/conf/apache/extra/httpd-vhosts.conf`. Delete the example code. For each site you want to configure, add a section that looks like:

            <VirtualHost *:80>
              DocumentRoot "/Users/mparker17/Projects/Sites/example"
              ServerName example.dev
              ErrorLog "logs/example.error.log"
              CustomLog "logs/example.access.log" common
              <Directory /Users/mparker17/Projects/Sites/example>
                AllowOverride All
                Options All
                php_admin_value sendmail_path " tee -a /Applications/MAMP/Library/logs/example.mail.log > /dev/null # "
              </Directory>
            </VirtualHost>

4. Set up PHP:
    1. Look in `/Applications/MAMP/bin/php`. Rename the folders containing PHP versions you *do not want to use* so they have `-old` at the end.
    2. For each version of PHP you *do want to use* (assuming `5.6.10`; find-replace in this document as required):
        1. Download the PHP source code for your version(s) of PHP.

                mkdir -p /Applications/MAMP/bin/php/php5.6.10/include/php
                cd /Applications/MAMP/bin/php/php5.6.10/include/
                curl -fSL http://php.net/get/php-5.6.10.tar.xz/from/this/mirror -o php-5.6.10.tar.xz
                tar -xf ./php-5.6.10.tar.xz -C php --strip-components=1
                rm ./php-5.6.10.tar.xz

        2. Configure the PHP sources:

                cd /Applications/MAMP/bin/php/php5.6.10/include/php
                ./configure

        4. Edit `php.ini` file (e.g.: `/Applications/MAMP/bin/php/php5.6.10/conf/php.ini`), and set:

                [PHP]
                max_execution_time = 0
                max_input_time = 0
                memory_limit = -1
                display_errors = On
                display_startup_errors = On

                [apc]
                apc.enabled=1
                apc.shm_segments=1
                apc.shm_size=512M

                [pdo_mysql]
                pdo_mysql.default_socket = /Applications/MAMP/tmp/mysql/mysql.sock

                [OPcache]
                zend_extension="/Applications/MAMP/bin/php/php5.6.10/lib/php/extensions/no-debug-non-zts-20131226/opcache.so"
                opcache.memory_consumption=128
                opcache.interned_strings_buffer=8
                opcache.max_accelerated_files=4000
                opcache.revalidate_freq=60
                opcache.fast_shutdown=1
                opcache.enable_cli=1

                [xdebug]
                ; Uncomment the line that specifies where xdebug is.
                xdebug.remote_enable = On
                xdebug.max_nesting_level = 512

5. Set up your shell:
    * If you don't know what shell you use, run `echo $SHELL`
    * If you use the Bash shell, add a line to `$HOME/.bashrc` that says:

            export PATH="/Applications/MAMP/bin/php/php5.6.10/bin:$PATH"
            export PATH="/Applications/MAMP/Library/bin:$PATH"

    * If you use the Zsh shell, add a line to `$HOME/.zshrc` that says:

            export PATH="/Applications/MAMP/bin/php/php5.6.10/bin:$PATH"
            export PATH="/Applications/MAMP/Library/bin:$PATH"

    * If you use the Fish shell, add a line to `$HOME/.config/fish/config.fish` that says:

            set PATH /Applications/MAMP/bin/php/php5.6.10/bin $PATH
            set PATH /Applications/MAMP/Library/bin $PATH
