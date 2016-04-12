# Memcached on MAMP (optional)

(find-replace `5.6.10` with your desired version of PHP in this file)

1. Ensure you have the PHP source code for your version(s) of PHP (this should be done if you set up MAMP using the instructions in `1 Setting up MAMP.md`)

        ls /Applications/MAMP/bin/php/php5.6.10/include/php

    ... if you see files there, then you're good. Otherwise, see `1 Setting up MAMP.md` step 4.2.

2. The PHP memcached extension uses the libmemcached library to provide an API for communicating with memcached servers. Install it, and find the path to it's folder:

        brew install libmemcached
        brew info libmemcached

3. Install the memcached extension:

        /Applications/MAMP/bin/php/php5.6.10/bin/pecl install memcached

    It will ask you for the path to the libmemcached directory. Use the path you got in step 2.

4. Edit `php.ini` file (e.g.: `/Applications/MAMP/bin/php/php5.6.10/conf/php.ini`), and set:

        [PHP]
        ; Extensions
        extension=memcached.so

5. Restart Apache.
6. Check `phpinfo()` for a `memcached` section.

## Setting up Drupal

1. Install [the Drupal memcache module](http://drupal.org/project/memcache).
2. See https://github.com/mparker17/drupal-settings_local_snippets/blob/master/7/memcache.php for information about how to set up `settings.local.php`:
