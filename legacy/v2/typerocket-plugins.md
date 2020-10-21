Title: TypeRocket Plugins
Description: Use TypeRocket plugins for "Theme Options" and SEO or make your own.

---

TypeRocket comes with its own plugin system so you can modularize your theme development within TypeRocket itself. TypeRocket comes with three plugins: `seo`, `dev`and `theme-options`.

## SEO

The SEO plugin added a matebox to every public post type with fields to add some basic SEO.

## DEV

The DEV plugin added a custom page to the WordPress admin to aid in development.

## Theme Options

The "Theme Options" plugin adds custom theme options to your WordPress theme.

### Hooks

"Theme Options" has a hook to let you edit the template file it uses; `tr_theme_options_page`.

For example, you can create a template file named `theme-options.php` in the current themes folder and use it instead.

```php
add_filter('tr_theme_options_page', function() {
    return get_template_directory() . '/theme-options.php';
});
```

*Note: The default template is the admin.php file that comes with the plugin.*

You can also use the action hook `tr_theme_options_page`. This hook lets you run code before the theme options page outputs any HTML.

```php
add_action('tr_theme_options_page', function($options) {
    // your code here
});
```

You can also change the group that the theme options are saved to with the filter `tr_theme_options_name`. You should always change the name to reduce the chances of theme conflicts.

```php
add_filter('tr_theme_options_name', function($group) {
    return 'my_theme_options';
});
```

## Making Custom Plugins

1. First, locate the plugins directory in TypeRocket. If you moved the location for plugins use that directory instead.
2. Second, create a new folder inside the plugins folder. A good plugin directory name is `your-custom-plugin` not `your_plugin_name`.
3. Third, add a file named `init.php` inside your new plugin directory. This is the file that will contain your PHP code.
4. In the config.php file add the directory name to `TR_PLUGINS` separating it with pipe `|`.

```php
define('TR_PLUGINS', 'your-custom-plugin|seo|dev|theme-options');
```

Just replace `your-plugin-name` with the name of the directory you created.

*Note: "your-custom-plugin"  is just an example of what you might name the directory.*