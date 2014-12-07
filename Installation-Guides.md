## How To Enable Mod_Rewrite
#### Unix:

In order to use mod_rewrite you can type the following command in the terminal:

`a2enmod rewrite`

Restart apache2 after

`/etc/init.d/apache2 restart`

or

`service apache2 restart`


#### Windows (WAMP):

wamp tray icon > apache > apache module > rewrite_module


## How To Install Composer Dependencies

Navigate to directus/api/ and install composer:

`curl -s https://getcomposer.org/installer | php`

Install via composer:

`php composer.phar install`