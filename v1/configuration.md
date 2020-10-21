Title: Configuration
Description: Specifics for configuring TypeRocket and its features.

---

## Configuration

All of the configuration files for the TypeRocket are stored in the `config` directory.


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

Because WordPress uses constants for user-defined variables over env variables, TypeRocket provides the `immutable()` function for accessing a constant but also provides a default value if it is not found.

```php
immutable('WP_DEBUG', true);
```

You can use this function when creating your configuration options.

## Extensions

In your `config/app.php` file, you can add and remove extensions loaded by TypeRocket.

The `extensions` settings take a key-value array of with key as the class name and the value as an array of arguments that will be used by the extension class' constructor. 

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
  '\TypeRocket\Extensions\Seo' => [ null ],  
  '\TypeRocket\Extensions\PageBuilder' => null,  
  '\TypeRocket\Extensions\PostTypesUI' => null,  
  '\TypeRocket\Extensions\ThemeOptions' => null,  
  '\TypeRocket\Extensions\DevTools' => [ true ],  
  '\TypeRocket\Extensions\PostMessages' => null,  
  '\TypeRocket\Extensions\Gutenberg' => [ true ],  
],
```