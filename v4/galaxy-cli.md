Title: Galaxy CLI
Description: The Galaxy CLI help developers run built-in commands and custom ones. 

---

## About Galaxy CLI

The Galaxy CLI comes with TypeRocket out of the box and has a number of commands to aid in the development process. You can access the CLI and the list of commands by entering the following in the command line:

```bash
php galaxy list
```

*Note: The Galaxy command-line tool is not available if you are using the official TypeRocket Framework 4 WordPRess plugin.*


## Accessing DB From Galaxy CLI on Host Machine

When you are using a host machine for your development like [Laravel Homestead](https://github.com/laravel/homestead) you will want to be able to run DB enabled commands from the command line. To do this, you need to make sure your `wp-config.php` file can access the correct DB from your CLI.

Here is an example of a `wp-config.php` file designed to be run on the host and guest machine.

```php
define('DB_NAME', 'wordpress');
define('DB_USER', 'homestead');
define('DB_PASSWORD', 'secret');

/** MySQL hostname with CLI access */
if( php_sapi_name() !== 'cli' || gethostname() == 'homestead' ) {
    define('DB_HOST', 'localhost');
} else {
    define('DB_HOST', '127.0.0.1:33060');
}
```

*Note: You can always run your commands from the guest machine without extra configuration.*

## TypeRocket Commands

The make commands generate template code for you. These commands do not change configuration information or modify files. They simply create new files so you do not need to manually.

### Make

```
make:command     Make new command
make:controller  Make new controller
make:middleware  Make new middleware
make:model       Make new model
```

### Use

```
use:root         Use TypeRocket as root
use:templates    Use TypeRocket for templates
```

## WP Commands

To access WordPress command you need to first configure the Galaxy CLI.

### Enabling WP Commands

To enable WP commands locate your `config/galaxy.php` file and find the `wordpress` setting (it should be set to `false` by default). Change the `wordpress` setting to the directory that your `wp-load.php` file is located in. The `wp-load.php` file should be located at the root of your WordPress installation where the `wp-config-sample.php` is located as well.

*Note: The path that you choose might be different than what is located below.*

```php
/*
|--------------------------------------------------------------------------
| WordPress
|--------------------------------------------------------------------------
|
| Set to the WordPress root directory. This will enable new WP specific
| Galaxy commands like: SQL, Migrations, and Flushing Permalinks
|
| Example of root installation: TR_PATH . '/wordpress'
|
*/
'wordpress' => TR_PATH . '/wordpress',
```

### Commands

```
wp:flush    Hard flush the WordPress rewrites
wp:sql      WordPress database SQL script
```

## Galaxy For the TypeRocket WordPress Plugin

The official TypeRocket WordPress plugin does not come with the galaxy CLI for security reasons. However, You can create or download the Galaxy CLI for development.

Once you have downloaded or created the `galaxy` CLI you can edit the file to work with your configuration as needed.

### Download

Navigate to the public root of your WordPress project. This is the location where the `wp-settings.php` file is located. Then run the following command to download the `galaxy` CLI.

```bash
php -r "copy('https://raw.githubusercontent.com/TypeRocket/galaxy-plugin/master/galaxy', 'galaxy');"
```

You can now run `php galaxy` from that location.

### Create

In your main WordPress directory add a file named `galaxy` without `.php` extension and add the following code.

```php
#!/usr/bin/env php
<?php
$typerocket = __DIR__ . '/wp-content/plugins/typerocket-framework/typerocket';

define('TR_CORE_CONFIG_PATH', $typerocket . '/config' );
define('TR_WP_ROOT', __DIR__ );
define('WPMU_PLUGIN_URL', '/mu-plugins');

require $typerocket . '/vendor/autoload.php';
require $typerocket . '/init.php';
new \TypeRocket\Console\Launcher();
```

### Trouble Shooting

If you have trouble connecting to MySQL from the Galaxy CLI you may need to change your `wp-config.php` file. You can [learn more about VM CLI access above](https://typerocket.com/docs/v4/galaxy-cli/#section-accessing-db-from-galaxy-cli-on-host-machine).

Also, you may need to modify this copy of the `galaxy` file to fit your installation if you have TypeRocket installed in a location other than `wp-content/plugins/typerocket-framework`.