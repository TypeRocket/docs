Title: Plugin Install
Description: How to install TypeRocket Pro.

---

## Requirements

Before getting started, be sure your setup supports [the TypeRocket system and browser requirements](/docs/v5/requirements).

## Plugin Installation

Anyone can download and enjoy TypeRocket from [here](#link-comming-soon).

If you have the pro version... install TypeRocket plugin for WordPress [go to your account](/account/) and download the plugin zip file. Find the download link at the bottom of your "Purchase Receipt" page.

*Note: Do not modify the TypeRocket plugin files. You would only extend TypeRocket using available hooks and `wp-config.php` constants. When the documentation tells you to edit TypeRocket files, those edits should be made to your version of those files.*

<iframe width="560" height="315" src="https://www.youtube.com/embed/-VkAydury5c" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### 1. Download & Activate 

1. Download the latest version of TypeRocket plugin.
2. Upload the plugin in the WordPress admin or unzip the files into your WordPress sites `plugins` directory.
3. Activate the TypeRocket plugin in WordPress.
4. Activate your TypeRocket site license if you are using Pro.

From the TypeRocket settings page located under "Settings > TypeRocket" add your site license. Activating your license gives you access to automatic updates.

Your license restricts you to a specific number of activations. You can remove old or current sites form your license in your [typerocket.com account](https://typerocket.com/account/).

### 2. Create Override Folders

<iframe width="560" height="315" src="https://www.youtube.com/embed/OC97D_kJRaY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Optionally, once you have installed TypeRocket, you may want to override the base configuration. You can do this in two ways; by placing the override folders, with files, in the active theme or the root of your site.

First, access and [download the TypeRocket project override folders from GitHub](https://github.com/TypeRocket/typerocket). The override folders include: `app`, `config`, `resources`, `routes`, and `storage`.

Next, place those folders in either:

- The root of your active theme. 
- The root of your WordPress installation where your `wp-settings.php` is located.

If you place your files in the root of WordPress (not your active theme) also add the following to your `wp-config.php` file:

```php
// Root WordPress customizations
define('TR_CORE_CONFIG_PATH', __DIR__ . '/config');
define('TR_ALT_PATH', __DIR__);
define('TR_APP_NAMESPACE', 'App');
define('TR_AUTOLOAD_APP', [
    'prefix' => TR_APP_NAMESPACE . '\\',
    'folder' => __DIR__. '/app/',
]);
```