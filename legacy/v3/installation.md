Title: Installation
Description: How to install and configure TypeRocket. PHP 5.5+ and WordPress 4.5+.

---

## Requirements

The TypeRocket framework has a few system requirements.

- PHP >= 5.5.9
- WordPress >= 4.5
- PDO PHP Extension
- Mbstring PHP Extension
- Friendly URLs
- [Composer](https://getcomposer.org/)

## Theme Installation

To install TypeRocket within an existing WordPress theme follow these instructions.

<iframe width="560" height="315" src="https://www.youtube.com/embed/PWSKaiZNoKk?rel=0" frameborder="0" allowfullscreen></iframe> 

### 1. Download

Go to your custom theme folder, not the WordPress themes folder. Then in the command line and run the composer command:

```bash
composer create-project --prefer-dist typerocket/typerocket typerocket v3.0.9
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

### 1. Download TypeRocket with Composer

To install TypeRocket as your main application create the TypeRocket project before installing WordPress.

In a fresh location run:

```bash
composer create-project --prefer-dist typerocket/typerocket typerocket v3.0.9
```

### 2. Root Typerocket

 [Rooting TypeRocket](/docs/v3/rooting-typerocket/) is using TypeRocket as the "root" of a project and setting the `wordpress` directory as public. To "root" Typerocket run the galaxy command `use:root`.

```bash
php galaxy use:root {database} {username} {password}
```

![tr-root](https://typerocket.com/wp-content/uploads/2016/09/tr-root.gif)

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

## Plugin Installation

You can install TypeRocket into a plugin in the same way you install it into a theme with one exception: You need to tell TypeRocket where its core JavaScript, CSS, fonts, images, components, and other assets are located.

You can do this under `config/paths.php`. For example, I'm using TypeRocket in the root of a plugin call "My Plugin". Inside the plugin folder the `paths.php` configuration file is located under `wp-content/plugins/my-plugin/typerocket/config/paths.php`. In the `paths.php` I need to update the `urls` setting to get TypeRocket working:

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
        'assets' => plugin_dir_url(__DIR__ . '..') . '/wordpress/assets',
        'components' => plugin_dir_url(__DIR__ . '..') . '/wordpress/assets/components',
    ],

    // ....

];
```

### Using TypeRocket Within a Plugin.

To use TypeRocket within a plugin be sure to use wrap any registerables, like post types, within the hook `typerocket_loaded`.

```php
add_action( 'typerocket_loaded', function() {
    // register post types, taxonomies, and pages
});
```

