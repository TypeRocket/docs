Title: Configuration
Description: Specifics for configuring TypeRocket and its features.

---

## Configuration

The configuration files for TypeRocket are stored in the `config` folder.


## Accessing Configuration Values

You can access the values stored in a config file using dot notation within the `tr_config()` function. For example, `app.debug` will locate the key `debug` inside the `app.php` file.

```php
$value = tr_config('app.debug');
```

You can also set a default value if no value can be found.

```php
$value = tr_config('app.debug', false);
```

## Defining Configuration Values

Because WordPress uses constants for user-defined variables over env variables, TypeRocket provides the `typerocket_env()` function for accessing a constant but also provides a default value if it is not found.

```php
typerocket_env('WP_DEBUG', true);
```

You can use this function when creating your configuration options.

## Extensions

In your `config/app.php` file, you can add and remove extensions loaded by TypeRocket. The `extensions` settings take an array of extension classes. 

```php
/*
|--------------------------------------------------------------------------  
| Extensions  
|--------------------------------------------------------------------------  
|  
| The class names of the TypeRocket extensions you wish to enable.  
|  
*/  
'extensions' => [  
  '\TypeRocket\Extensions\PostMessages',
  '\TypeRocket\Extensions\PageBuilder',
  '\TypeRocket\Extensions\TypeRocketUI',
],
```

### Pro

TypeRocket Pro also comes with these additional extensions:

```php
'extensions' => [
   // ... 
  '\TypeRocketPro\Extensions\ThemeOptions',
  '\TypeRocketPro\Extensions\DevTools',
  '\TypeRocketPro\Extensions\Seo',
  '\TypeRocketPro\Extensions\RapidPages',
  '\TypeRocketPro\Extensions\HidePostMenu',
  '\TypeRocketPro\Extensions\DisableComments',
  '\TypeRocketPro\Extensions\GutenbergRemover',
]
```