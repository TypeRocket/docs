Title: Configuration
Description: Specifics for configuring TypeRocket and its features.

---

TypeRocket is highly configurable. No matter what your project requires TypeRocket can be configured to work exactly as needed in your project.

<iframe width="560" height="315" src="https://www.youtube.com/embed/SKSLoFeoy1U?rel=0" frameborder="0" allowfullscreen></iframe>

## WP Plugin Configuration

If you are using the official WordPress plugin installation of TypeRocket you need to configure your settings using available hooks and overrides. 

TypeRocket allows for a single `config` folder per active site. In your `wp-config.php` file you can define the location of a custom config folder but setting the `TR_CORE_CONFIG_PATH` constant. If you use a custom location you can edit the config files as desired.

```php
// Example only
define('TR_CORE_CONFIG_PATH', __DIR__ . '/your/config');
```

You can download the needed `config` [folder from GitHub](https://github.com/TypeRocket/typerocket).

*Note: This option is available to all regardless of how TypeRocket has been installed.*

## Configuration file

You can use customize TypeRocket by editing the configuration files in the `config` folder.

## Disable and Enable Plugins

To enable and disable the plugins edit the `plugins` setting. Plugins are loaded by class name. The core TypeRocket plugins are loaded through composer.

```php
'plugins' => [
    '\TypeRocketSEO\Plugin',
    '\TypeRocketPageBuilder\Plugin',
    '\TypeRocketThemeOptions\Plugin',
    '\TypeRocketDev\Plugin',
    '\TypeRocketDashboard\Plugin',
]
```

## Debug mode

In the `app.php` file set `debug` to `true`. Debug mode adds hints to fields and other areas to aid in development.

```php
'debug' => true
```

## Class Overrides

You can override the default icons, user model, and form model that is used by TypeRocket.

```php
/*
|--------------------------------------------------------------------------
| Class Overrides
|--------------------------------------------------------------------------
|
| Set the classes to use as the default for helper functions.
|
*/
'class' => [
    'icons' => \TypeRocket\Elements\Icons::class,
    'user' => \App\Models\User::class,
    'form' => \TypeRocket\Elements\Form::class
],
```

## Features

You can now control if specific features used by WordPress should be used by your site. For example, you can disable Gutenberg without the classic editor plugin or maybe you want to disable comments from your site entirely.

```php
/*
|--------------------------------------------------------------------------
| Enabled Features
|--------------------------------------------------------------------------
|
| Options to control what features you can use on the site.
|
*/
'features' => [
    'gutenberg' => true,
    'posts_menu' => true,
    'comments' => true,
],
```

## TypeRocket Assets

In the `paths.php` file you can set your assets URL. The URLs paths should be the only paths you edit.

```php
'urls' => [
  'assets' => get_template_directory_uri() . '/typerocket/wordpress/assets',
  'components' => get_template_directory_uri() . '/typerocket/wordpress/assets/components',
],
```

### Setting to Root

For example, when TypeRocket is the root of your project instead of WordPress you can set the URLs to another location.

```php
'urls' => [
  'assets' => home_url() . '/assets',
  'components' => home_url() . '/assets/components',
]
```

*Note: If you change this setting you will also want to make sure your `webpack.mix.js` is updated to the new location as well.*

## Move App Folder

To move the `app` folder to another location you will need to update your setup for either composer or the plugin depending on your installation type.

### Composer

Update your `composer.json` file with the new `App` namespace location. 

### Plugin

If you are using the plugin version of TypeRocket you can set the `TR_WP_PLUGIN_APP_MAP` constant in your `wp-config.php` file:

```php
// wp-config.php
define('TR_WP_PLUGIN_APP_MAP', [ 'prefix' => 'App\\', 'folder' => 'your app folder path here' ]);
```

## MySQL 5.7 Notes

In some cases with the most recent version of MySQL 5.7 WordPress will have issues with the `sql_mode`. Depending on your server you will need to disable `STRICT_TRANS_TABLES`. Locate your `my.cnf` file used by MySQL and disable the newer, better, strict mode. If you need more information on this there is [an article here on the topic](https://kevdees.com/disable-mysql-strict-mode-and-no_zero_date-errors-in-homestead/).

## Functional Features Configeration 

Some features require being called via a function call. you can call these features in the `functions.php` file of your theme.

### On the Fly Image Sizing

By calling the `tr_image_sizing()` function thumbnail images will only be generated when they are needed instead of all at once when the image is uploaded.

### SSL Enforcement

By calling `tr_ssl()` TypeRocket will do its best to enforce SSL on all content on the front-end of your website.