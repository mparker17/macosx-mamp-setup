# PHP tools on MAMP (recommended)

(find-replace `5.6.10` with your desired version of PHP in this file)

1. Install Composer using the instructions at https://getcomposer.org/download/
    You may wish to move the `composer.phar` file from your current directory to somewhere else (e.g.: `/usr/local/bin`):

        mv ./composer.phar /usr/local/bin/composer

2. Set up your shell:
    * If you don't know what shell you use, run `echo $SHELL`
    * If you use the Bash shell, add a line to `$HOME/.bashrc` that says:

            export PATH="$HOME/.composer/vendor/bin:$PATH"

    * If you use the Zsh shell, add a line to `$HOME/.zshrc` that says:

            export PATH="$HOME/.composer/vendor/bin:$PATH"

    * If you use the Fish shell, add a line to `$HOME/.config/fish/config.fish` that says:

            set PATH $HOME/.composer/vendor/bin $PATH

## Misc tools

1. Tell composer to install the tools:

        composer global require 'phpmd/phpmd'
        composer global require 'psy/psysh'

## Drush & Drupal Console

1. Tell composer to install Drush and Drupal Console:

        composer global require 'drush/drush'
        composer global require 'drupal/console'

2. Optional: set up your shell:
    * If you don't know what shell you use, run `echo $SHELL`
    * If you use the Bash shell, add a line to `$HOME/.bashrc` that says:

            export DRUSH_PHP="/Applications/MAMP/bin/php/php5.6.10/bin/php"

    * If you use the Zsh shell, add a line to `$HOME/.zshrc` that says:

            export DRUSH_PHP="/Applications/MAMP/bin/php/php5.6.10/bin/php"

    * If you use the Fish shell, add a line to `$HOME/.config/fish/config.fish` that says:

            set DRUSH_PHP /Applications/MAMP/bin/php/php5.6.10/bin/php

## PHP CodeSniffer and the Drupal sniffs

1. Tell composer to install PHPCodeSniffer and the Coder module:

        composer global require 'squizlabs/php_codesniffer'
        composer global require 'drupal/coder'

2. Tell PHPCodeSniffer where to find the Drupal sniffs:

        phpcs --config-set installed_paths $HOME/.composer/vendor/drupal/coder/coder_sniffer

## Pantheon CLI

1. Tell composer to install it:

        composer global require 'pantheon-systems/terminus'

2. Go to your Pantheon dashboard. Go to the *Account* tab, then the *Machine Tokens* sub-tab. Click `Create Token`. Give it a name and click `Generate Token`. Confirm your identity if asked. Copy the machine token and authenticate for the first time:

        terminus auth login --machine-token=$your_token
