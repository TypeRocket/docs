Title: Extension: Dev Tools
Description: Access powerful dev tools for WordPress.

---

## Getting Started

! **Pro Only**: This is a Pro only extension feature.

The `TypeRocketPro\Extensions\DevTools` extension adds an admin page to WordPress named "Dev". From the "Dev" page you can:

1. Search and view the available TypeRocket icons that can be used for post types and pages.
2. See all the registered image sizes added by WordPress of through [add_image_size()](https://developer.wordpress.org/reference/functions/add_image_size/).
3. View all TypeRocket routes and registered WordPress rewrite rules.
4. View the status of all TypeRocket migrations.
5. List of roles registered to WordPress.
6. Capabilities applied to WordPress roles.

## Toolbar

The `DevTools` extension also adds a toolbar to the bottom of every page is `WP_DEBUG` is set to `true`.

From the toolbar, you can view all of your database queries is you have `define('SAVEQUERIES', true);` in your `wp-config.php`.

Also, you can quickly flush the WordPress rewrite rules from the toolbar by clicking the "Flush Rules" tab.

You can disable the toolbar by adding `define('TYPEROCKET_DEV_TOOLBAR', false);` to your `wp-config.php`.