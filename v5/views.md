Title: Views
Description: Views are returned by a controllers for the page display. 

---

## Making a View

To create a `View` use the `tr_view()` helper function. This function takes two arguments:

1. `$view` - The view to be returned. This uses dot notation.
2. `$data` - An array of values to be [extracted](http://php.net/manual/en/function.extract.php) in the view. 
3. `$extension` - The default is `.php`.

```php
tr_view('books.index', ['data' => 'my data']);
```

Because a view uses dot notation, it can return different templates based on context. These contexts are: "Admin" and "Front-end".

## Admin Views & Front-end Views

Views are located under `resources/views` by default. When a controller returns a view it will return the view from the specified folder.

Take this example view, it will return the `resources/views/books/index.php` template file.

```php
tr_view('books.index', ['data' => 'my data']);
```

### View Page Title

To set a views page `<head>` `<title>` tag use the `setTitle()` method.

```php
tr_view('books.add')->setTitle('Add Book');
```

### SEO Meta

*Pro Only: This is a Pro only extension feature.*

If you are using views within a post type's template file, you do not need to set SEO meta of the view. However, if you view is for anything that is not a post type you will need to set the SEO meta using the `setSeoMeta()` view method.

If the view has access to the TypeRocket SEO meta from the `\TypeRocket\Extensions\SEO` extension, you can set the SEO for the page.

```php
tr_view('books.show')->setSeoMeta($meta, $url);
```

You can add the SEO fields to a custom backend using the `tr_seo_meta_fields()` function. The function requires a `Form` instance.

```php
tr_seo_meta_fields($form);
```

## Templating

When a view is loaded like `resources/views/books/index.php` and `resources/pages/books/index.php`, the passes array will be unpacked using the php `extract()` function.

So, a view with `['data' => 'my data']` passed to it has a `$data` variable with the string `'my data'` in its template file.

```php
// index.php
echo '<p>' . $data . '</p>';
```

Will output,

```html
<p>my data</p>
```

## Twig Engine

*Pro Only: This is a Pro only extension feature.*

If you want to use [Twig](https://twig.symfony.com/) for your views, require Twig as apart of your project using `composer`.

```bash
composer require "twig/twig:^2.0"
```

Next, update your `template_engine` settings in your `config/app.php` file and switch your engine class to `\TypeRocket\Template\TwigTemplateEngine::class`. 

```php
/*
|--------------------------------------------------------------------------
| Template Engine
|--------------------------------------------------------------------------
|
| The template engine used to build views for the front-end and admin.
|
*/
'templates' => [
    'views' => '\TypeRocket\Template\TwigTemplateEngine',
],
```

Instead of naming your template files in the `.php` extension, use the `.twig` extension.

### Clear Twig Cache

To clear the twig cache using `galaxy`.

```php
galaxy cache:clear twig
```

*Note: If `WP_DEBUG` is enabled caching will be disabled, but the cache still needs to be cleared.*

### Customizing Twig Env

To customize the Twig config add a new config file named `config/twig.php` with the following code:

```php
return [
    'env' => [
         'debug' => typerocket_env('WP_DEBUG', true),
         'cache' => tr_config('paths.cache') . '/twig',
    ]
];
```

## Multiple Templating Engines

If you want to use an engine that is not the default, you can use the `setEngine()` method.

```php
$engine = '\TypeRocket\Template\TwigTemplateEngine';
tr_view('books.show')->setEngine($engine);
```

## Views In Templates

*Pro Only: This is a Pro only extension feature.*

You can also use views within your standard WordPress templates. To use views for your WordPress templates use the `\TypeRocket\Http\Template::respond()` static method.

```php
<?php  
/**
 * Example WordPress Template MVC
 *
 * my-theme/index.php
 * 
 * @var WP_Post[] $posts  
 */  
\TypeRocketPro\Http\Template::respond(function() use ($posts) {  
    $title = 'Pro';  
  
    return tr_view('master', compact('posts', 'title'));  
});
```

When using the `\TypeRocketPro\Http\Template::respond()` static method, you are able to exit the global scope and avoid conflicting with global variables. To access any global variables, you must pass them using the `use` statement.