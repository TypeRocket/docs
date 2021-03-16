Title: Install With Composer
Description: Installing TypeRocket Pro with Composer

---

## Requirements

Before getting started, be sure your setup supports [the TypeRocket system and browser requirements](/docs/v5/requirements).

**Important**: Pro users with a Plus or Max license will have access to composer via github for the pro extension. Single site licenses do not have composer access for the pro extension.

## Licensing & Use

Before installing TypeRocket using composer, [be aware of the licensing restrictions and our advice on how TypeRocket should be used](https://typerocket.com/how-to-use-and-install-typerocket/).

## Install TypeRocket with Composer

**Important**: Composer `2.0+` is required.

To install TypeRocket using composer, first [download composer](https://getcomposer.org/download/) and install it globally. You can [read the full instructions on the composer.org website](https://getcomposer.org/doc/00-intro.md).

### 2. Installation Types

There are two recommended ways to download and install TypeRocket using composer:

1. WordPress MU Plugin install.
2. Root install.

For beginners, the MU install is the fittest place to start.

*Note: Only one installation type is required for TypeRocket to work. Either the MU plugin install or the root install. Also, you should not install TypeRocket into an existing plugin or theme.*

### 2.1 MU Install

From the command line, go to your `wordpress/wp-content/mu-plugins` folder.

```
composer create-project --prefer-dist typerocket/typerocket typerocket
```

Finally, in the root of your [WordPress MU plugins folder](https://wordpress.org/support/article/must-use-plugins/) create a PHP file named `typerocket.php` and add the following.

```php
<?php
/*
Plugin Name: TypeRocket MU  
Description: MU plugin installation.  
Author: TypeRocket  
Version: 5  
Author URI: http://typerocket.com  
*/
define('TYPEROCKET_MU_INSTALL', '/typerocket/wordpress/');  
require ('typerocket/init.php');
```

### 2.2 Root Install

*Note: You do not need to install WordPress before doing a root install.*

To install TypeRocket as your root application, create the TypeRocket Pro project in a new location run from the command line.

```
composer create-project --prefer-dist typerocket/typerocket typerocket
cd typerocket
```

Next, use the TypeRocket Galaxy CLI `root:install` command to install WordPress and start your project:

```bash
php galaxy root:install {database} {db_username} {db_password}
```

TypeRocket root install is now complete. You can access the WordPress files from the `typerocket/wordpress` folder. Finally, Complete the WordPress installation by pointing your servers root web folder to the `typerocket/wordpress` and then visit your site's web address.

#### PHP Server

Optionally, if you do not have a web server configured, you can use the built-in TypeRocket PHP server on your local machine to view your WordPress site.

```
php -S localhost:8888 -t wordpress server.php
```

In your browser, visit `localhost:8888` to view the WordPress install page.

#### What Root Install Does

Running `root:install` will:

- Downloads WordPress in the `wordpress` folder where the assets are located.
- Creates the `wp-config.php` file in the main TypeRocket folder.
- Enables TypeRocket's root theme as the default.
- Add `require __DIR__ . '/init.php';` to your `wp-config.php`.

#### Deploying A Root Install

When pushing your code to a production server, ensure that you have the correct `wp-config.php` settings. Note, when rooting TypeRocket `wp-config.php` is modified and `init.php` is required after the `ABSPATH` is defined. This is important to note because you need to require the `init.php` in `wp-config.php` for production and not just development.

```php
/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__) . '/');

require __DIR__ . '/init.php'; // Include TypeRocket

/** Sets up WordPress vars and included files. */
require_once(ABSPATH . 'wp-settings.php');
```

## Updating

To update TypeRocket run `composer update`. This will update all the PHP code and JS/CSS assets.

## Pro

If you are using TypeRocket Pro and have composer access with your license you can follow these instructions to get `typerocket/professional` installed.

### 1. Authenticate

Enable access to the TypeRocket composer repository by authenticating your server or development computer with `typerocket.repo.packagist.com` using your "Composer Token". You can access your token from [your TypeRocket account](https://typerocket.com/account/).

Authenticate composer by replacing the text `YOUR_TOKEN_GOES_HERE` from the following command with your token. From the command line run:

```
composer config --global --auth http-basic.typerocket.repo.packagist.com token YOUR_TOKEN_GOES_HERE
```

*Note: Token authentication is required only once per machine or server.*

### 2. Install

After you have added composer auth access from [your TypeRocket account](https://typerocket.com/account/). 
Run these commands but replace `YOUR_URL_HERE` with the "Composer URL" from your account page:

```
composer create-project --prefer-dist typerocket/typerocket typerocket
cd typerocket
composer config repositories.pro '{"type": "composer","url": "YOUR_URL_HERE","only": ["typerocket/professional"]}'
composer require typerocket/professional
php galaxy extension:publish typerocket/professional
```