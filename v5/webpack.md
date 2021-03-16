Title: Webpack with Laravel Mix
Description: Building a theme with Laravel Mix makes managing assets easy.

---

## About Laravel Mix

The [webpack](https://webpack.js.org/) NPM package is a build tool for the front-end assets of your theme. It is also an automation tool to help you manage your development workflow.

Laravel Mix takes advantage of webpack and makes the development process that much better. You can learn more about everything you can do with webpack on [Mix's documentation page](https://laravel-mix.com/).

### Installing Mix

To get started, you need to first download the NPM packages required to use mix in the root of your TypeRocket folder.

```bash
npm install
```

## Using

The best place to get started is on the [Mix's documentation page](https://laravel-mix.com/). In TypeRocket, your main webpack file `webpack.mix.js` is in the root TypeRocket directory. 


## Development

You are in the development process run `npm run watch` to watch for file changes and update your assets on the fly.

```shell
npm run watch
```

## Production

When your code is ready for production, run `npm run prod` to minify and compress your assets.

```bash
npm run prod
```

## Versioning

TypeRocket Pro supports with versioning for Webpack files that use the `verson()` method. You can use the `tr_manifest_cache()` method to locate your `mix-manifest.json` file.

```php
// Define Theme Directory  
define('THEME_DIR', get_template_directory_uri() );  
  
$manifest = tr_manifest_cache( tr_config('paths.assets') . '/templates/mix-manifest.json', 'templates');  
  
// Theme Assets  
add_action('wp_enqueue_scripts', function() use ($manifest) {  
    wp_enqueue_style( 'main-style', THEME_DIR . $manifest['/theme/css/theme.css'] );  
    wp_enqueue_script( 'main-script', THEME_DIR . $manifest['/theme/js/theme.js'], [], false, true );  
});  
  
// Admin Assets  
add_action('admin_enqueue_scripts', function() use ($manifest) {  
    wp_enqueue_style( 'admin-style', THEME_DIR . $manifest['/admin/css/admin.css'] );  
    wp_enqueue_script( 'admin-script', THEME_DIR . $manifest['/admin/js/admin.js'], [], false, true );  
});
```