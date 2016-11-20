# Search optimisation module

This module executes directly from the POST request at search_form_block without the need of an additional GET request. It assumes there is just node search on the site.

## Prerequisites

You will need `composer`, `drush` and `git` installed to the system.

## Set up

Checkout repository:

```
git clone https://github.com/tassoskoutlas/search_optimisation.git && cd search_optimisation
```

Create `settings.local.php`:

```
touch sites/default/settings.local.php
```

Add the following snippet:

```
<?php

/**
* Local settings file.
*/
$databases['default']['default'] = array(
'driver' => 'mysql',
'database' => 'DB_NAME',
'username' => 'DB_USER',
'password' => 'DB_PASS',
'host' => 'localhost',
'prefix' => 'main_',
'collation' => 'utf8_general_ci',
);
```
where DB_NAME, DB_USER and DB_PASS are the name, user and password of the
database server.

Execute the make file:

```
drush make drupal.make -y
```

Install Drupal standard profile:

```
drush si standard -y
```

Enable all required modules:

```
drush en master -y && drush master-execute --scope=base -y
```

## Usage

You can login to Drupal normally and use the search block on the sidebar. 

With the search_optimisation module enabled when you search through the block just one request is produced, similar to the following:

```
"POST /search_optimisation/node HTTP/1.1" 200 4919
```

This is in contrast of the default behaviour where a POST request is followed by a GET, similar to the following:

```
"POST /search_optimisation/ HTTP/1.1" 302 454
"GET /search_optimisation/search/node/cats%20OR%20dogs HTTP/1.1" 200 5047
```

## Coding standards

The code follows Drupal coding standards. Withing the module there is a composer.json file that contains PHP_Codesniffer and Drupal coder definitions.

Install extensions:

```
cd sites/all/modules/custom/search_optimisation/ && composer install
```

Install Drupal coding standards

```
./vendor/bin/phpcs --config-set installed_paths $(pwd)/vendor/drupal/coder/coder_sniffer/
```

Check the code:
```
./vendor/bin/phpcs --standard=Drupal --extensions=php,info,module search_optimisation.*
```

## Credits

This code is developed and maintained by
[Tassos Koutlas](https://github.com/tassoskoutlas).

The code is distributed under the
[EUPL v1.1](http://ec.europa.eu/idabc/eupl.html) open source software license.

