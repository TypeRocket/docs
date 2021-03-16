Title: Helper Functions
Description: All of the TypeRocket Helper functions

---

## Check Debug

Check if debug mode is enabled.

```php
$bool = tr_debug()
```

## App Classes

```php
echo tr_app_class('Models\\Post');
// Output: App\Models\Post
```

## Site State Changed

Used to facilitate tasks like flushing rewrite rules for the registration
and de-registration of post types and taxonomies.

- `@param string|array $arg` - Single function name or array of function names

```php
$arg = 'flush_rewrite_rules';
tr_update_site_state($arg)
```

## Container

Gets an instance of the class instance from the DI container if there is one.

```php
$config = tr_container('TypeRocket\\Core\\Config');
```

