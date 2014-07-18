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
