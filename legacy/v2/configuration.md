Title: Configuration
Description: Specifics for configuring TypeRocket and its features.

---

Before you can use TypeRocket you need to create a `config.php` file in TypeRocket folder you installed. You can duplicate the `config.sample.php` file and rename it to `config.php`.

## TypeRocket Folder

If you are including TypeRocket in a folder named something other than typerocket (in your themes directory) open the `config.php` file and change the `TR_FOLDER` to the name you have chosen.

For example if you move the typerocket folder into a folder named inc:

```php
define('TR_FOLDER', 'inc/typerocket');
```

Or, you rename the typerocket folder to inc:

```php
define('TR_FOLDER', 'inc');
```

## TypeRocket Plugins

By default TypeRocket loads three plugins from the plugins folder: `seo`, `dev` and `theme-options`.

```php
define('TR_PLUGINS', 'seo|dev|theme-options');
```

Each plugin name coincides with its folder name and is separated by a pipe `|`. Look under the plugins folder to see all the plugins available.

## Debug mode

In the `config.php` file set `TR_DEBUG` to `true`. Debug mode adds hints to fields and other areas to aid in development.

```php
define('TR_DEBUG', true);
```

## Matrix Folder

When you are using matrix fields `TR_MATRIX_FOLDER_PATH` specifies where the groups are located.

The default location is under the typerocket folder as `matrix`.

```php
define('TR_MATRIX_FOLDER_PATH', __DIR__ . '/matrix');
```

You can set this to your theme if you like.

```php
$theme_dir = get_template_directory();
define('TR_MATRIX_FOLDER_PATH', $theme_dir  . '/matrix');
```

## TypeRocket Plugins Folder

You can also change the plugins directory `TR_PLUGINS_FOLDER_PATH` and URL `TR_PLUGINS_URL`.

For example, you can set the plugins inside the theme as well.

```php
$theme_dir = get_template_directory();
define('TR_PLUGINS_FOLDER_PATH', $theme_dir . '/plugins');

$theme_url = get_stylesheet_directory_uri();
define('TR_PLUGINS_URL', $theme_url . '/plugins');
```

*Note: The plugins path and URL need to point to the same folder.*

## TypeRocket Seed

The seed needs to change to help increase the security of TypeRocket. This is not required but it is highly recommended.

```php
define('TR_SEED', 'asdfa65739as$%!fasdljo3m3!l0');
```

## TypeRocket App

TypeRocket lets you add your own files to the TypeRocket autoloader. Typically controllers and models are added to the `/app` folder.

```php
define('TR_APP_FOLDER_PATH', __DIR__ . '/app');
```