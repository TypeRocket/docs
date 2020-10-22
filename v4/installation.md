Title: Download & Installation
Description: How to install and configure TypeRocket. PHP 7+ and WordPress 5+.

---

## Before Installing

Before getting started with TypeRocket v4 (the free version), be aware that [TypeRocket Pro](/pro) has a [large number of new features](/compare-versions/) and is a massive upgrade from v4.

If you plan to use TypeRocket v4, keep in mind that the upgrade process from v4 to Pro will take some time. 

## Requirements

The TypeRocket framework has a few system requirements.

- PHP >= 7
- WordPress >= 5.2
- PDO PHP Extension
- Mbstring PHP Extension
- Friendly URLs
- [Composer](https://getcomposer.org/) (Custom Installations Only)

## Using TypeRocket

Before installing TypeRocket using composer [be aware of the possible conflicts and issues you may encounter if you plan to mass distribute your plugin or theme](https://l.rb.typerocket.test/how-to-use-and-install-typerocket/).

## WP Plugin

**Composer not required**

This is the install guide for our official TypeRocket Framework 4 WordPress plugin. If you are making your own plugins for WordPress you should build those plugins against this plugin.

**WARNING**: Do not edit the files within the official plugin. When in the documentation you are told to edit TypeRocket files those edits are to made to your own versions of those files (If you have custom installation using composer this is not the case).    

### 1. Download & Install

Composer is not required to use the official TypeRocket Framework 4 WordPress plugin.

1. [Download the latest version of TypeRocket here](https://l.rb.typerocket.test/downloads/latest.zip)
2. Unzip the files into your WordPress sites `plugins` directory.
3. Active the TypeRocket Framework Plugin.

*Note: You should not modify the files and TypeRocket core system within the WordPress plugin. You would only extend TypeRocket using available hooks and configuration options.*

### 2. Configuring

If you plan to develop your site using TypeRocket you may need to create your own `config` and `app` folders.

It is not required for all features but it is recommended to override where your `config` and `app` folders are located. TypeRocket allows for a single `app` folder and `config` folder per active site.

You can download the needed `config` and `app` [folders from GitHub](https://github.com/TypeRocket/typerocket).

In your `wp-config.php` file you can define the location of a custom config folder but setting the `TR_CORE_CONFIG_PATH` constant.

```php
// Example only
define('TR_CORE_CONFIG_PATH', __DIR__ . '/your/config');
```

Likewise, you can define your application folder using the `TR_WP_PLUGIN_APP_MAP` constant in the `wp-config.php` file as well.

```php
// Example only
define('TR_WP_PLUGIN_APP_MAP', [
    'prefix' => 'App\\',
    'folder' => __DIR__ . '/your/app/',
])
```

If you do not define an `app` folder in your `wp-config.php` file TypeRocket will:

1. First, look for an `app` folder your active theme.
2. Lastly, if no custom `app` folder is found TypeRocket will use the `app` folder within its core system.

Also, you should not edit the `routes.php` file in TypeRocket if you are using the official plugin (composers installation can ignore this). To add routes when using the official plugin you will need to make your own custom WordPress plugin and add routes from that plugin.

For example, you might create a plugin named `my-typerocket-routes.php` with the following code:

```php
<?php
// Plugin Name: My TypeRocket Routes
add_action('tr_load_routes', function() {
  
    tr_route()->get()->match('hello')->do(function() {
        return 'Hello';
    });

    tr_route()->get()->match('hello-there')->do(function() {
        return 'Hello there!';
    });

});
```  

### 3. Using TypeRocket Within Your Custom Plugins

Once you have the official plugin installed you can begin using TypeRocket in your theme and other plugins.
 To use TypeRocket within your custom plugins be sure to use wrap any registerables, like post types, within the hook `typerocket_loaded`.

```php
add_action( 'typerocket_loaded', function() {
    // register post types, taxonomies, and pages
});
```

If you want to add a PSR4 namespace to load a TypeRocket plugin you can do so using the same hook.

```php
// Example only
add_action('plugins_loaded', function() {
    $map_site_options = [
        'prefix' => 'TypeRocketSiteOptions\\',
        'folder' => __DIR__ . '/typerocket/vendor/typerocket/plugin-site-options/src/',
    ];
    tr_autoload_psr4($map_site_options);
}, 0, 0);
```

### 4. Developer Notes

The official **TypeRocket Framework 4** plugin does not have the following features:

- The `galaxy` CLI tool but you can [install it separately](https://l.rb.typerocket.test/docs/v4/galaxy-cli/#section-typerocket-wordpress-plugin).
- Migrations (You will need to use the official WordPress method for migrations)

Not all of the normal default TypeRocket plugins are enabled by default. You will need to enable those plugins manually using a custom config as outlined below. These plugins are:

- Page Builder
- Sitemap (needs custom PSR4 autoloader)
- Site Options (needs custom PSR4 autoloader)

## Theme Installation

To install TypeRocket within an existing WordPress theme follow these instructions.

<iframe width="560" height="315" src="https://www.youtube.com/embed/G9UE8xV9ZOY?rel=0" frameborder="0" allowfullscreen></iframe> 

### 1. Download

Go to your custom theme folder, not the WordPress themes folder. Then in the command line and run the [composer](https://getcomposer.org/) command:

```bash
composer create-project --prefer-dist typerocket/typerocket:v4
```

*Note: The composer command will create a `typerocket` folder for you. You do not need to make a folder yourself.*

### 2. Import

At the top of your themes `functions.php` file require `typerocket/init.php`. This will initialize TypeRocket.

```php
<?php // functions.php

require ('typerocket/init.php');
```

### 3. Activate

To complete the installation, you need to [flush the WordPress permalinks and rewrite rules](/flushing-permalinks-in-wordpress/).

*Note: Any time you register a post type or change a post type ID you need to do the same.*

## Root Installation

If you are building a large site or application you will want to manage your entire development process within TypeRocket exposing only WordPress and plugins to visitors from the server.

To do this you need to do the following.

1. Download TypeRocket
2. Root TypeRocket
3. Finish

<iframe width="560" height="315" src="https://www.youtube.com/embed/iPKdJng8Bnc?rel=0" frameborder="0" allowfullscreen></iframe> 

### 1. Download TypeRocket with Composer

To install TypeRocket as your main application create the TypeRocket project before installing WordPress.

In a fresh location run:

```bash
composer create-project --prefer-dist typerocket/typerocket:v4
```

### 2. Root Typerocket

 [Rooting TypeRocket](/docs/v4/rooting-typerocket/) is using TypeRocket as the "root" of a project and setting the `wordpress` directory as public. To "root" Typerocket run the galaxy command `use:root`.

```bash
php galaxy use:root {database} {username} {password}
```

![tr-root](https://l.rb.typerocket.test/wp-content/uploads/2016/09/tr-root.gif)

Running `use:root` will do all the legwork for you.

- Downloads WordPress in the `wordpress` folder where the assets are located.
- Creates the `wp-config.php` file in the main TypeRocket folder next to the `init.php` file.
- Enables root templates.
- Installs TypeRocket MU plugin for theme override.
- Updates the TypeRocket assets paths.
- `require()` the `init.php` file in `wp-config.php`.

#### Production

When pushing your code to a production server ensure that you have the correct `wp-config.php` settings, the MU TypeRocket WordPress plugin, and the correct TypeRocket configuration. To do this simply ensure that you use the configurations set by the `use:root` command.

Note, when rooting TypeRocket `wp-config.php` will be modified and `init.php` will be required after the `ABSPATH` is defined. This is important to note because you will need to require the `init.php` in `wp-config.php` for production and not just development.

```php
/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
	define('ABSPATH', dirname(__FILE__) . '/');

require __DIR__ . '/init.php'; // Include TypeRocket

/** Sets up WordPress vars and included files. */
require_once(ABSPATH . 'wp-settings.php');
```

### 3. Finish

Finally, complete the WordPress installation by visiting your site's web address.

## MU Plugin

You can install TypeRocket into an MU plugin in the same way you install it into a theme with one exception: You need to tell TypeRocket where its core JavaScript, CSS, fonts, images, components, and other assets are located.

<iframe width="560" height="315" src="https://www.youtube.com/embed/jgHuwmg2CK4?rel=0" frameborder="0" allowfullscreen></iframe>


*Note: It is best to install TypeRocket as an MU plugin. This is best practice in the WordPress community for all frameworks.*

### 1. Download

Download TypeRocket into your `mu-plugins` folder; if you do not have the folder create it.

```php
composer create-project --prefer-dist typerocket/typerocket:v4
```

### 2. Configure

You can do this under `config/paths.php`. For example, I'm using TypeRocket in the root of the `mu-plugins` folder. The `paths.php` configuration file is located under `wp-content/mu-plugins/typerocket/config/paths.php`. In the `paths.php` I need to update the `urls` setting to get TypeRocket working:

```php
<?php // paths.php
return [
    /*
    |--------------------------------------------------------------------------
    | Assets URL
    |--------------------------------------------------------------------------
    |
    | The URL where TypeRocket assets can be found.
    |
    */
    'urls' => [
        'assets' => WPMU_PLUGIN_URL . '/typerocket/wordpress/assets',
        'components' => WPMU_PLUGIN_URL . '/typerocket/wordpress/assets/components',
    ],

    // ....

];
```

### 3. Create an MU Plugin

Next, all you need is an MU plugin and have it import TypeRocket. As an example, here is an MU plugin named `typerocket.php` added under the `mu-plugins` folder; the full path for the MU plugin is `wp-content/mu-plugins/typerocket.php`;

```php
<?php
// Plugin Name: TypeRocket
require_once 'typerocket/init.php';
```
