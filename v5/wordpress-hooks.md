Title: WordPress Hooks
Description: Extend and hook into TypeRocket with actions and filters.

---

## About Hooks

TypeRocket comes with several hooks to help you extend its core functionality. Not all the hooks are listed here.

## Actions

### typerocket_loaded

The `typerocket_loaded` hook runs during `after_setup_theme`, after TypeRocket core has loaded, and before the TypeRocket registry runs. This hook allows WordPress plugins to interact with TypeRocket. 

```php
add_action('typerocket_loaded', function() {
    // do stuff
});
```

### typerocket_before_load

You can also use `typerocket_before_load` if you want to run code before the TypeRocket system is loaded.

```php
add_action('typerocket_before_load', function() {
    // do stuff
});
```

### typerocket_model

The `typerocket_model` action hook runs after a model is constructed.

```php
add_action('typerocket_model', function($model) {
    // do stuff
});
```

### typerocket_after_routes

The `typerocket_after_routes` action hook runs after the TypeRocket `public.php` route file is loaded.

```php
add_action('typerocket_after_routes', function() {
    // do stuff
});
```

### typerocket_kernel

The `typerocket_kernel` action hook runs when `TypeRocket\Http\HttpKernel` is instantiated.

```php
add_action('typerocket_kernel', function($kernel) {
    // do stuff
});
```

## Filters

### typerocket_middleware

Return the middleware stack for the current hook call or REST API request.

```php
add_filter('typerocket_middleware', function($middleware, $globalGroup) {
    return $middleware;
}, 10, 2);
```

### typerocket_extensions

Modify the plugins that should be loaded.

```php
add_filter('typerocket_extensions', function($extensions){
    return $extensions;
});
```