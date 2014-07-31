## Composer dependency management

The Directus API is written in PHP and employs a number of third-party, open-source packages to manage routing, rendering, database access, and so on. See the core Composer documentation [here](https://www.google.com/url?q=https%3A%2F%2Fgetcomposer.org%2F&sa=D&sntz=1&usg=AFQjCNEhxf8df2A4nLZIZrCVBRdpgDtF9Q).

Best practice [says](https://www.google.com/url?q=https%3A%2F%2Fgetcomposer.org%2Fdoc%2F01-basic-usage.md%23composer-lock-the-lock-file&sa=D&sntz=1&usg=AFQjCNFiTope9TfHBzLrje3eM8CW_efAAg) that the composer.lock file should be tracked in the application repository. This helps to ensure stability by guaranteeing that over time, and across the development team, the versions of application dependencies will not change with each installation, but will remain exactly the same.

This is especially critical from the perspective of Directus code integration and inter-dependency integration. Case in point: the Slim package introduces in version 2.4 breaking changes in its integration with Twig. Therefore changes to the composer.lock file are extremely sensitive. Here are some guidelines on how to modify Directus’ composer dependencies.

### Never run composer.phar update
* This “updates” the version of each dependency to the latest version.
* If for whatever reason you need to re-install your vendors, always instead remove your vendor directory and re-run `composer.phar install`

### Avoid modifying the composer.json file directly
* Always modify dependencies using composer.phar because it will modify the lock file directly
* For example, to add a new dependency, run this command:
  * `composer.phar [require](https://getcomposer.org/doc/03-cli.md#require) vendor/package:version`
  * This command updates your JSON file and your lockfile without changing the versions of your other dependencies.

## Requirements

* Apache HTTP Server
* MySQL Server
* PHP >5.2
* Git
* Composer

## Instructions

### Step 1: Setup a development environment
You will need to setup a local *AMP environment (Apache, MySQL and PHP) in order to test Directus. *AMP packages can be found for different platforms:

* [MAMP](http://www.mamp.info/en/index.html) for Mac OS
* [WAMP](http://www.wampserver.com/en/) for Windows
* [LAMP](https://help.ubuntu.com/community/ApacheMySQLPHP) for Linux

### Step 2: Install git
You will need Git to keep your test version of Directus up to date. Unless you already have Git installed, you can download it from [here](http://git-scm.com/).

### Step 3: Pull down Directus from github
Create a directory named "directus" in the www root. Next, open the directory in the terminal and type:

```
$ git init
$ git clone https://github.com/RNGR/directus6
```
### Step 4: Install dependencies
Directus uses composer to handle its php dependencies. Go to the `directus/api/` folder and install composer:

```
$ curl -s https://getcomposer.org/installer | php
```
Then install the dependencies by executing:

```
$ php composer.phar install
```

### Step 5: Setup the database
The AMP-packages listed above all include Phpmyadmin. The following three steps need to be completed in order to setup the database

1. Create the database
   1. Databases -> Create Database
   2. Type *directus* as name and select *utf8_general_ci* as Collation.
2. Create a user
   1. Once you've created the database it will be visible in the left column – open it by clicking it.
   2. Privileges -> Add user
   3. Fill in *username* and *password*. Leave the other fields as defaults.
3. Import the database schema
   1. Import->File to Import->Choose file
   2. Open api/schema.sql
   3. Press Go

### Step 6: Setup Directus
Open `directus/api/config_sample.php` Add the database username and password from Step 5 to *DB_USER* and *DB_PASSWORD*. Save the file as ```directus/api/config.php```

```
define('DB_USER', 		'myusername');
define('DB_PASSWORD',	'mypassword');
```
### Step 7: Setup Files Uploads
This requires configuring your Storage Adapters. Currently the best way to do this is to manually edit the `directus_storage_adapters` table using a SQL client.

By default, `directus.sql` should contain two storage adapters out of the box, with the `DEFAULT` and `THUMBNAIL` roles, both using the `FileSystemAdapter` (meaning these will map to your local hard drive).

Simply define the `destination` (absolute path) and `url` (equivalent URL to the same directory) parameters of each of these records. For example, using a MAMP setup, you might define them this way:

```
UPDATE  `directus_storage_adapters` SET  `destination` =  '/Applications/MAMP/htdocs/directus-media/', `url` = 'http://localhost:8888/directus-media/' WHERE  `directus_storage_adapters`.`id` = 1;
UPDATE  `directus_storage_adapters` SET  `destination` =  '/Applications/MAMP/htdocs/directus-media-thumbnails/', `url` = 'http://localhost:8888/directus-media-thumbnails/' WHERE  `directus_storage_adapters`.`id` = 2;
```

### Step 8: Done!
Open directus by navigating to the path *directus* in your local host. Log in using the default user *admin@getdirectus.com* and password *admin*