Title: Galaxy CLI
Description: The Galaxy CLI help developers run built-in commands and custom ones. 

---

## About Galaxy CLI

The Galaxy CLI comes with TypeRocket out of the box and has a number of commands to aid in the development process. You can access the CLI and the list of commands by entering the following in the command line:

```bash
php galaxy list
```

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

There are a number of Galaxy commands.

### Cache

Commands to interface with the TypeRocket cache.

```
cache:clear        Clear Cache
```

This command removes all files from a specified cache folder located under the configured path for storage `paths.storage`. The main cache folder is located under `storage/cache`. So, the command `php galaxy cache:clear app` will remove all files from `storage/cache/app`, `php galaxy cache:clear twig` will remove all files from `storage/cache/twig`, and so forth.

### Config

Commands to interface with the TypeRocket config.

```
config:seed        Generate seed for app.php
``` 

In the `config/app.php` file of a fresh project you will see the `'seed' => 'PUT_TYPEROCKET_SEED_HERE'`. Running `php galaxy config:seed` will replace the seed with a random string.

### Core

Commands used to interface with TypeRocket core.

```
core:update        Update Core
```

When running `composer update` on your project this command will run. It downloads all the latest js, css, and other assets into the public directory for your project. To manually download the assets for TypeRocket run `php galaxy core:update`.

### Extension

To publish custom TypeRocket enabled extensions.

```
extension:publish  Publish extension package
```

Extensions, differ from WordPress plugins in that they are installed via composer only and do not have an interface on the plugins admin page in WordPress. You can [read more about extensions here](/docs/v5/custom-composer-packages).

### Make

The make commands generate template code for you. These commands do not change configuration information or modify files. They create new files so you do not need to manually.

```
make:command       Make new command
make:component     Make new component
make:composer      Make new view composer
make:controller    Make new controller
make:fields        Make new HTTP fields container
make:middleware    Make new middleware
make:migration     Make new migration
make:model         Make new model
make:plugin        Make new WP plugin (pro only)
make:policy        Make new auth policy
make:rule          Make new validation rule
make:service       Make new service
```

### Root

If you are using TypeRocket as the root of your project.

```
root:install       Install TypeRocket as root
```

### Other

Other commands.

```
help               Displays help for a command
list               Lists commands
migrate            Run migrations
```

## WP Commands

Commands to interface with WordPress.

```
wp:download        Download WordPress
wp:flush           Hard flush the WordPress rewrites
wp:maintenance     WordPress maintenance mode
wp:sql             WordPress database SQL script
```

To access WordPress command, you might need to configure the Galaxy CLI. In most cases, the `tr_wp_root()` function will do the work for you automatically.

*Note: Be sure your CLI can access the database from its current host. For example, if you are using a VM, you might not be able to access the database of the guest machine.*

### Manullay Configuring WP 

To manually configure WP commands, locate your `config/galaxy.php` file, and find the `wordpress` setting. Change the `wordpress` setting to the directory that your `wp-load.php` file is located in. The `wp-load.php` file should be located at the root of your WordPress installation, where the `wp-config-sample.php` is located as well.

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
'wordpress' => TYPEROCKET_PATH . '/wordpress',
```

## Galaxy For the TypeRocket WordPress Plugin

Navigate to the public root of your WordPress project. This is the location where the `wp-settings.php` file is located. In your main WordPress directory, copy the `galaxy` file from the TypeRocket plugin to this location and add a file named `galaxy-config.php` .

Add the following code to your `galaxy-config.php` and point the `$typerocket` variable to the `typerocket` folder within the plugin version you are using.

You can also [watch the tutorial on YouTube](https://youtu.be/tXPn7wUfBdo?t=165).

```php
<?php
// galaxy-config.php

$typerocket = __DIR__ . '/wp-content/plugins/typerocket-pro-v5/typerocket';
define('TYPEROCKET_GALAXY_PATH', $typerocket);
define('TYPEROCKET_CORE_CONFIG_PATH', $typerocket . '/config' );
define('TYPEROCKET_ROOT_WP', __DIR__);
```

Also, you might want to update where the galaxy command creates new files like models and controllers.

```php
// The folder that contains your app folder
// not the app folder itself
define('TYPEROCKET_APP_ROOT_PATH', __DIR__ . '/wp-content/themes/my-theme');
define('TYPEROCKET_ALT_PATH', __DIR__ . '/wp-content/themes/my-theme');
```

### Trouble Shooting

If you have trouble connecting to MySQL from the Galaxy CLI, you may need to change your `wp-config.php` file. You can [learn more about VM CLI access above](https://typerocket.com/docs/v5/galaxy-cli/#section-accessing-db-from-galaxy-cli-on-host-machine).

## Making a Command

To make your own galaxy CLI command use the galaxy command `make:command`. Galaxy commands use the [Symfony Console Package](http://symfony.com/doc/current/console.html). Let's make a command named `app:test`.

```shell
php galaxy make:command Test app:test
```

This will output,

```shell
Command created: Test
Configure Command Test: Add your command to config/galaxy.php
```

You can now find your new command at `app/Commands/Test.php`. We will take a look at it next, but first, add the command to the `galaxy.php` configuration file.

```php
'commands' => [
    \App\Commands\Test::class
]
```

Now run `php galaxy list` and check for:

```shell
app
  app:test         Short description here
```

Now run the command:

```shell
 php galaxy app:test
```

This will output,

```shell
Executed!
```

You are now ready to start building your command.

### Coding Commands

Here is the `Test` command class we just created. If you want to make your commands without the galaxy CLI, this can serve as a working template.

```php
<?php
namespace App\Commands;

use \TypeRocket\Console\Command;

class Test extends Command
{

    protected $command = [
        'app:test',
        'Short description here',
        'Longer help text goes here.',
    ];

    protected function config()
    {
        // If you want to accept arguments
        // $this->addArgument('arg', self::REQUIRED, 'Description');
    }

    public function exec()
    {
        // When command executes
        $this->success('Execute!');
    }
}
```