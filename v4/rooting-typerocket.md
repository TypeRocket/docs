Title: Rooting TypeRocket
Description: What does `use:root` do behind the scenes and how can you manually?

---

To "root" TypeRocket run the galaxy command `use:root` right after you have created your composer project.

```sh
php galaxy use:root {database} {username} {password}
```

Then all that is left is to complete the WordPress installation. But, what does `use:root` do behind the scenes and how can you manually root TypeRocket yourself?

## Installation WordPress

To manually "root" TypeRocket, install WordPress in the `wordpress` folder where the assets are located. Then, create the `wp-config.php` file in the main TypeRocket folder next to the `init.php` file.

## Connect TypeRocket 

In the `wp-config.php` file just after the `ABSPATH` is defined and before WordPress settings are loaded  include the TypeRocket `init.php` file.

```php
/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
	define('ABSPATH', dirname(__FILE__) . '/');

require __DIR__ . '/init.php'; // Include TypeRocket

/** Sets up WordPress vars and included files. */
require_once(ABSPATH . 'wp-settings.php');
```

## Enable Theme Override

TypeRocket lets you use its template overrides. This keeps you from creating a theme in the `wp-content/themes` directory and exposing your theme's code.

Enable TypeRocket templates by running the galaxy common `use:templates` and point it to your `wp-content` directory.

*Note: `pwd` is a great command for finding your current folder location.*

If your path is `/www/wordpress/wp-content` then run:

```sh
php galaxy use:templates /www/wordpress/wp-content
```

This will create the `mu-plugins` directory and `typerocket.php` plugin needed to map the default theme to `/resources/themes/templates`.

In the `app.php` configuration file set the `templates` option from `false` to `'templates'` if it has not been done by the `php galaxy templates` command.

```php
'templates' => 'templates'
```

## Set the asset URLs

In your `paths.php` configuration file set the `urls` to a public location.

```php
'urls' => [
  'assets' => home_url() . '/assets',
  'components' => home_url() . '/assets/components',
]
```

Or, for HTTPS,

```php
'urls' => [
  'assets' => home_url('/assets', 'https'),
  'components' => home_url('/assets/components', 'https'),
]
```

## Install WordPress 

Complete the WordPress installation by visiting your sites web address.

## Optional: Sync WordPress To Galaxy CLI

Finally, if you would like access to the `galaxy` WordPress commands go to the `galaxy.php` config file and set the "wordpress" setting to `TR_PATH . '/wordpress'`.

```php
'wordpress' => TR_PATH . '/wordpress',
```


### MySQL Port Forwarding

If your connection to the MySQL database is not to the host machine, like with port forwarding in Laravel Homestead, set the `wp-config.php` files `DB_HOST` to the correct location based on environment.

When using Homestead the following configuration will allow using the Galaxy CLI on the host and guest machines. 

```php
if( php_sapi_name() !== 'cli' || gethostname() == 'homestead' ) {
    define('DB_HOST', 'localhost');
} else {
    define('DB_HOST', '127.0.0.1:33060');
}
```

If you do not know your host name run the following command to find out what it is.

```shell
echo "<?php echo gethostname().PHP_EOL;" | php
```