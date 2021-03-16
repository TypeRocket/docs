Title: Hooks
Description: Extend and hook into TypeRocket with actions and filters.

---

TypeRocket comes with a number of hooks to help you extend its core functionality.

## Core Loaded

The `typerocket_loaded` hook runs during `after_setup_theme`, after TypeRocket core has been loaded, and before the TypeRocket registry is run.

This hook is designed to allow WordPress plugins to interact with TypeRocket. 

```php
add_action( 'typerocket_loaded', function() {
    // do stuff
});
```

## Models

The `tr_model` action hook runs after a model is constructed

```php
add_action( 'tr_model', function($model) {
    // do stuff
});
```

### Fillable

The `tr_model_fillable_{model}` filter sets the fillable fields of a model. For example, the filter applied the `PagesModel` would have the name `tr_model_fillable_pages`.

```php
add_filter('tr_model_fillable_pages', function($fillable, $model){
    return $fillable;
}, 10, 2);
```

### Guard

`tr_model_guard_{model}` works the same as the fillable hook.

```php
add_filter('tr_model_guard_pages', function($guard, $model){
    return $guard;
}, 10, 2);
```

### Secure fields

Filter the fields when a model tries to secure the user input. This filter run after fillable and guard security has been run.

```php
add_filter( 'tr_model_secure_fields', function($fields, $model) {
    return $fields;
}, 10, 2);
```

## REST API

Return `true` if the REST API should be run.

```php
add_filter( 'tr_rest_api_load', function($load, $resource, $itemId) {
    return $load;
}, 10, 3);
```

Return `true` if the REST API template should be loaded.

```php
add_filter('tr_rest_api_template', function($load) {
    return $load;
});
```

## Middleware

Return the middleware stack for the current hook call or REST API request.

```php
add_filter('tr_kernel_middleware', function($middleware, $request, $type) {
    return $middleware;
}, 10, 3);
```

## Plugins Collection

Modify the plugins that should be loaded.

```php
add_filter('tr_plugins_collection', function($collection){
    return $collection;
});
```

## Matrix

Return `true` to run the matrix API.

```php
add_filter('tr_matrix_api_load', function($load, $group, $type, $formGroup) {
    return $load;
}, 10, 4);
```

Return `true` if the matrix API template should be loaded.

```php
add_filter('tr_matrix_api_template', function($load) {
   return $load;
});
```

