Title: WordPress Hooks
Description: Extend and hook into TypeRocket with actions and filters.

---

TypeRocket comes with several hooks to help you extend its core functionality. Not all the hooks are listed here.

## Core Loaded

The `typerocket_loaded` hook runs during `after_setup_theme`, after TypeRocket core has loaded, and before the TypeRocket registry runs.

This hook allows WordPress plugins to interact with TypeRocket. 

```php
add_action('typerocket_loaded', function() {
    // do stuff
});
```

You can also use `typerocket_before_load` if you want to run code before the TypeRocket system is loaded.

*Note: If you are creating plugins that interact with TypeRocket you will want to install TypeRocket as an MU plugin.*

## Models

The `typerocket_model` action hook runs after a model is constructed.

```php
add_action('typerocket_model', function($model) {
    // do stuff
});
```

## Middleware

Return the middleware stack for the current hook call or REST API request.

```php
add_filter('typerocket_middleware', function($middleware, $globalGroup) {
    return $middleware;
}, 10, 2);
```

## Extensions List

Modify the plugins that should be loaded.

```php
add_filter('typerocket_extensions', function($extensions){
    return $extensions;
});
```