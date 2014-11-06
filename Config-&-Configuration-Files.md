#Config & Configuration Files
These files are automatically generated during the installation process, alternatively you can utilize the sample files to setup manually.

## api/config.php
This file contains project-specific constants for the Directus framework. These values are typically not changed after initial setup.

* **Default timezone**
* **API version utilized** (API_VERSION)
* **Environment** (DIRECTUS_ENV)
    * production => error suppression, nonce protection
    * development => no error suppression, no nonce protection (allows manual viewing of API output)
    * staging => no error suppression, no nonce protection (allows manual viewing of API output)
    * development_enforce_nonce => no error suppression, nonce protection
* **Database Connection Settings**
    * DB_HOST => MySQL hostname, often "localhost"
    * DB_NAME => MySQL database name
    * DB_USER => MySQL database user with appropriate permissions
    * DB_PASSWORD => The password for the above MySQL user
    * DB_PREFIX => An optional table prefix. If set, only tables with prefix will be managed
* **Slave Database Connection Settings**
You have the option to enter credentials for a slave database server. If set, Directus will fall back to a slave database if the master fails.
    * DB_HOST_SLAVE => MySQL slave hostname, often "localhost"
    * DB_USER_SLAVE => MySQL slave database user with appropriate permissions
    * DB_PASSWORD_SLAVE => The password for the above MySQL slave user
* **Paths and URLs**
    * DIRECTUS_PATH => Directus Application Path, i.e. "/"
    * $host => Server Name i.e. "www.example.com"
    * ROOT_URL => Based off the $host, this is scheme-less (agnostic to http/https)
    * ROOT_URL_WITH_SCHEME => Same as ROOT_URL but explicitly defines the scheme (https://). Use this for emailing URLs(links, images etc) as some clients will trip on the scheme agnostic ROOT_URL
    * APPLICATION_PATH => Absolute path to application
* **Memcached**
    * MEMCACHED_SERVER => Memcached Server, operates on default 11211 port. i.e. "127.0.0.1"
    * MEMCACHED_ENV_NAMESPACE => Namespaced memcached keys so branches/databases to not collide. Options are:
        * prod
        * staging
        * testing
        * development
* **Status Options**
    * STATUS_DELETED_NUM => When utilizing the status column for soft-delete policy, this is the integer used for deleted items. This is important because Directus ignores deleted items system-wide as opposed to inactive or other status states.
    * STATUS_ACTIVE_NUM => Similarly, the active column has special meaning within the soft-delete policy as it is considered truly "active"
    * STATUS_COLUMN_NAME => (Default: "active") This is the adjustable column name for the status system. Adding a column of this name (datatype: INT/TINYINT) will enable a soft-delete policy for that table. The settings for the above active/deleted values (and any additional workflow stages) can be set within api/configuration.php.

## api/configuration.php
This file contains the configuration arrays for the Directus framework.

* **Session Prefix** This value allows you to run multiple versions of Directus on the same server without the sessions conflicting.
* **HTTP/HTTPS** Here you can set the _forceHttps_ variable (defaults to: false)
* **SMTP** Allows you to configure the host, port, username, and password for your SMTP mail server. (Utilized by _messages_ and _forgot password_)
* **Database Hooks** (dbHooks)
    * postInsert => A global function called after each database record insert
    * postUpdate => A global function called after each database record update
* **Database Table Blacklist** (tableBlacklist) Is an array of table names that will be ignored by the Directus framework
* **Status Mapping** (statusMapping) Is a multi-dimensional array of options for the status system (soft-delete policy) allowing for a customizable workflow to be defined. The following options exist as illustrated by the framework defaults:
    * key => (0, 1, and 2) represents the value to be saved in the status field (defined above) if that option is selected
    * name => (Deleted, Live, Draft) is the visible terminology displayed to the user in the UI
    * color => (#c1272d, #5b5b5b, #bbbbbb) is a hex value that the option name will be displayed in, for instance deleted is red, Live is dark gray, and Draft is a light gray)
    * sort => (3, 1, 2) is the order the options will be shown in as determined by your specific content workflow

_Default Status Mapping:_
```
'0' => array(
  'name' => 'Deleted',
  'color' => '#c1272d',
  'sort' => 3
),
'1' => array(
  'name' => 'Live',
  'color' => '#5b5b5b',
  'sort' => 1
),
'2' => array(
  'name' => 'Draft',
  'color' => '#bbbbbb',
  'sort' => 2
)
```