Title: Plugin Install
Description: How to install TypeRocket Pro.

---

## Requirements

Before getting started, be sure your setup supports [the TypeRocket system and browser requirements](/docs/v5/requirements).

*Note: Do not modify the TypeRocket plugin files. You would only extend TypeRocket using available hooks and `wp-config.php` constants. When the documentation tells you to edit TypeRocket files, those edits should be made to your version of those files.*

## Plugin Installation

TypeRocket v5 Open is free to use forever. To install TypeRocket as a plugin you can [checkout this video](https://www.youtube.com/watch?v=JNbSneZXBm4) or you can follow these steps:

1. [Download TypeRocket v5 here](https://typerocket.com/downloads/v5.zip).
2. Install the plugin into your WordPress site.
3. Turn on pretty URLs.
4. Optionally [add override folders to customize your installation](/docs/v5/install-via-plugin/#section-create-override-folders).

## Pro Plugin Installation

To install the TypeRocket Pro plugin [go to your account](/account/) and download the plugin zip file. Find the download link at the bottom of your "Purchase Receipt" page.

<iframe width="560" height="315" src="https://www.youtube.com/embed/QZZkVCtUCbo" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

1. Download the latest version of TypeRocket plugin.
2. Upload the plugin in the WordPress admin or unzip the files into your WordPress sites `plugins` directory.
3. Activate the TypeRocket plugin in WordPress.
4. Activate your TypeRocket site license.
5. Turn on pretty URLs.

From the TypeRocket settings page located under "Settings > TypeRocket" add your site license. Activating your license gives you access to automatic updates.

Your license restricts you to a specific number of activations. You can remove old or current sites form your license in your [typerocket.com account](https://typerocket.com/account/). If your authentication is not working:

1. You may need to log in to your account and free a site from licenses under "Account > View Licenses > Manage Sites".
2. Your license might be expired.
3. You may need to log in to your account and free a site from licenses.

## Create Override Folders

<iframe width="560" height="315" src="https://www.youtube.com/embed/tXPn7wUfBdo" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Optionally, once you have installed TypeRocket, you may want to override the base configuration. You can do this in two ways; by placing the override folders, with files, in the active theme or the root of your site.

First, [download the TypeRocket project override folders from GitHub if you are using the free plugin](https://github.com/TypeRocket/typerocket). If you are using the Pro plugin you can access these files from the plugin itself under the `typrocket` folder. The override folders include: `app`, `config`, `resources`, `routes`, and `storage`.

Next, place those folders in either:

- The root of your active theme. 
- The root of your WordPress installation where your `wp-settings.php` is located.

If you place your files in the root of WordPress (not your active theme) also add the following to your `wp-config.php` file:

```php
// Root WordPress customizations
define('TYPEROCKET_CORE_CONFIG_PATH', __DIR__ . '/config');
define('TYPEROCKET_ALT_PATH', __DIR__);
define('TYPEROCKET_APP_NAMESPACE', 'App');
define('TYPEROCKET_AUTOLOAD_APP', [
    'prefix' => TYPEROCKET_APP_NAMESPACE . '\\',
    'folder' => __DIR__. '/app/',
]);
```

### Pro Upgrade Overrides

If you are upgrading from the free plugin to the TypeRocket Pro plugin you will also need to copy some configuration files from the plugin itself into your `config` overrides folder. The config files can be found under `typerocket-pro-v5/typrocket/config`. The files include:

- `editor.php`.
- `logging.php`.
- `mail.php`.
- `rapid.php`.
- `storage.php`.

Next, you will need to locate your custom `app/Elements/Fom.php` file and add `use \TypeRocketPro\Elements\Traits\AdvancedFields;`:

```php
<?php
namespace App\Elements;

use TypeRocket\Elements\BaseForm;

class Form extends BaseForm
{ 
    use \TypeRocketPro\Elements\Traits\AdvancedFields;
}
```

### Galaxy CLI Setup

Navigate to the public root of your WordPress project. This is the location where the `wp-settings.php` file is located. In your main WordPress directory, copy the `galaxy` file from the TypeRocket plugin to this location and add a file named `galaxy-config.php` .

Add the following code to your `galaxy-config.php` and point the `$typerocket` variable to the `typerocket` folder within the plugin version you are using.

You can also [watch the tutorial on YouTube](https://youtu.be/tXPn7wUfBdo?t=165).

```php
// Your paths might be different.
// This example works with the Pro plugin.
$typerocket = __DIR__ . '/wp-content/plugins/typerocket-pro-v5/typerocket';
$overrides = __DIR__ . '/wp-content/themes/my-theme';

define('TYPEROCKET_GALAXY_PATH', $typerocket);
define('TYPEROCKET_CORE_CONFIG_PATH', $overrides . '/config' );
define('TYPEROCKET_ROOT_WP', __DIR__);

define('TYPEROCKET_APP_ROOT_PATH', $overrides);
define('TYPEROCKET_ALT_PATH', $overrides);

// Here we include the app folder
define('TYPEROCKET_AUTOLOAD_APP', [
    'prefix' => 'App',
    'folder' => $overrides . '/app',
]);
```