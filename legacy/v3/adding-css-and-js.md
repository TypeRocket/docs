Title: Adding CSS and JS
Description: The wp_enqueue_scripts hook lets you add css and javascript to a theme.

---

When you're adding styles and scripts to your WordPress theme you should always use the [wp_enqueue_scripts WordPress hook](https://codex.wordpress.org/Function_Reference/wp_enqueue_script). However, your theme must be using the required theme functions `wp_head()` and `wp_footer()` for `wp_enqueue_scripts` to fire.

Take a look at adding the main theme `style.css` file and a `script.js` from a theme's `js` folder.  

```php
<?php // functions.php
add_action( 'wp_enqueue_scripts', function() {
    $themeUrl = get_template_directory_uri();

    // add the style.css file to the html head
    wp_enqueue_style( 'main-css', get_stylesheet_uri() );

    // add javascript to the bottom of the theme
    wp_enqueue_script( 'main-js', $themeUrl . '/js/script.js', array(), '1.0', true );
} );
```

If you want to add more CSS and JavaScript files you need to use a different file handle for each file. The file handle is the first argument in `wp_enqueue_style()` and `wp_enqueue_script()`.

Take a look at the modified `functions.php` file.

```php
<?php // functions.php
add_action( 'wp_enqueue_scripts', function() {
    $themeUrl = get_template_directory_uri();

    // css files
    wp_enqueue_style( 'another-css', $themeUrl . '/css/theme.css' );

    // js files
    wp_enqueue_script( 'plugin-js', $themeUrl . '/js/plugin.js', array(), '1.0', true );
    wp_enqueue_script( 'main-js', $themeUrl . '/js/script.js', array(), '1.0', true );
} );
```

Here we have four handles: `main-css`, `another-css`, `plugin-js` and `main-js`.