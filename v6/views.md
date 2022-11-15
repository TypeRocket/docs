Title: Views
Description: Views are returned by a controllers for the page display. 

---

## Making a View

To create a `View` use the `tr_view()` helper function. This function takes two arguments:

1. `$view` - The view to be returned. This uses dot notation.
2. `$data` - An array of values to be [extracted](http://php.net/manual/en/function.extract.php) in the view. 
3. `$extension` - The default is `.php`.
4. `$folder` - The default is the views folder set in the config file.

```php
tr_view('books.index', ['data' => 'my data']);
```

Or, the same using OOP.

```php
\TypeRocket\Template\View::new('books.index', ['data' => 'my data']);
```

Views are located under `resources/views` by default (this location is set in your config under `paths.views`). When a controller returns a view it will return the view from the specified folder. Take this example view, it will return the `resources/views/books/index.php` template file.

```php
tr_view('books.index', ['data' => 'my data']);
```

If you pass an extension it will replace `.php` in the view lookup.

```php
// resources/views/books/index.blade.php
tr_view('books.index', [], '.blade.php');
```

Next, you can set a specific folder where your views are located if you do not want them loaded from the default views location set in the config `paths.views`.

```php
// resources/views/books/index.blade.php
tr_view('books.index', [], '.blade.php', __DIR__ . '/blade/views');
```

Instead of using dot notation, you can pass a file path too (this will ignore the `$extension` and `$folder` settings).

```php
tr_view(__DIR__ . '/my-view-file.php', ['data' => 'my data']);
```

## Rendering Views

You can use views to render their template files in a few ways. First, you can render views by returning them from a controller action. When a view is returned by a controller, TypeRocket will render the view at the right time for you.

```php
class BookController extends \TypeRocket\Controllers\Controller
{
    /**
     * Controller action
     */
    public function index()
    {
        return tr_view('books.index', ['data' => 'my data']);
    }
}
```

Or, you can render a view manually.

```php
echo tr_view('books.index', ['data' => 'my data']);
```

## Passing Data & Templating

When a view is loaded like `resources/views/books/index.php`, the passed array will be unpacked using the php `extract()` function. So, a view with `['data' => 'my data']` passed to it will have a `$data` variable with the string `'my data'` in its template file.

```php
// index.php
echo '<p>' . $data . '</p>';
```

Will output,

```html
<p>my data</p>
```

## View Page Title

To set a views page `<head>` `<title>` tag use the `setTitle()` method. This works when the view is rendered for the front-end.

```php
tr_view('books.add')->setTitle('Add Book');
```

## SEO Meta - !!

If you are using views within a post type's template file, you do not need to set SEO meta of the view. However, if you view is for anything that is not a post type you will need to set the SEO meta using the `setSeoMeta()` view method.

If the view has access to the TypeRocket SEO meta from the `\TypeRocketPro\Extensions\SEO` extension, you can set the SEO for the page.

```php
tr_view('books.show')->setSeoMeta($meta, $url);
```

You can add the SEO fields to a custom backend using the `tr_seo_meta_fields()` function. The function requires a `Form` instance.

```php
tr_seo_meta_fields($form);
```

## Caching

You can cache the rendered view by using the `cache()` method. This will cache the rendered view result until it is cleared from the cache. When caching provide as the arguments: 

1. A unique name for the cache.
2. (optional A cache time in seconds, `9999999999` is the default.
3. (optional) A folder for the cache, `views` is the default (was `app` in the past).

In this example, we cache the page for 2 minutes and cache the result to `storage/cache/my-views`

```php
echo tr_view('books.index')->cache('my-namespace.books.index', MINUTE_IN_SECONDS * 2, 'my-views');
```

To clear the cache use the Galaxy CLI and pass the cache folder name `my-views` as the argument.

```bash
php galaxy cache:clear my-views
```

If the folder was the default of `views` then run.

```bash
php galaxy cache:clear views
```

## Twig Engine - !!

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
    'views' => '\TypeRocketPro\Template\TwigTemplateEngine',
],
```

Instead of naming your template files in the `.php` extension, use the `.twig` extension.

### Clear Twig Cache

To clear the twig cache using `galaxy`.

```bash
php galaxy cache:clear twig
```

! **Note**: If `WP_DEBUG` is enabled caching will be disabled, but the cache still needs to be cleared.

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

## Tachyon Template Engine - !!

TypeRocket Pro also has a template engine called Tachyon. This template engine is a blazing fast and pure PHP templating engine equipped with layouts and more. You can read more about Tachyon in the [theme templating docs](/docs/v6/theme-templating/#section-advanced-views).

## Choose Templating Engine

If you want to use an engine that is not the default, you can use the `setEngine()` method.

```php
$engine = '\TypeRocketPro\Template\TwigTemplateEngine';
tr_view('books.show')->setEngine($engine);
```

## Views In Templates - !!

You can also use views within your standard WordPress theme's templates. To use views for your WordPress templates use the `tr_template_router` function.

```php
<?php  
/**
 * Example WordPress Template MVC
 *
 * my-theme/index.php
 * 
 * @var WP_Post[] $posts  
 */  
tr_template_router(function() use ($posts) {  
    $title = 'Pro';  
  
    return tr_view('master', compact('posts', 'title'));  
});
```

When using the `tr_template_router` function, you are able to exit the global scope and avoid conflicting with global variables. To access any global variables, you must pass them using the `use` statement.

## Custom View Folders

If you need to house your view files in a separate location from you main `resources/views` you can create your own view class.

```php
<?php
namespace MyNamespace;

class View extends \TypeRocket\Template\View
{
    public function init()
    {
        $this->setFolder(__DIR__ . '/your-folder-for/views');
    }
}
```

Then in your controller,

```php
$title = 'Pro';  

return \MyNamespace\View::new('master', compact('title'));  
```

Or, you can set the folder on the fly.

```php
$title = 'Pro';  
  
return tr_view('master', compact('title'))->setFolder(__DIR__ . '/your-folder-for/views');  
```

## View Contexts

View contexts allow you to define both an engine and views folder path at the same time using the TypeRocket config system. The default context is `views`.

A context looks up two config settings: 

1. Template engine under `app.templates.{context}`.
2. Views folder under `paths.{context}`.

You can add more context to your config settings to better group specific view systems. For example, maybe you have separate view locations and engines for email or admin templates.

```php
return tr_view('master')->setContext('admin');
```

The above will load its settings from the config, but they must be added by you first:

1. Template engine under `app.templates.admin`.
2. Views folder under `paths.admin`.

! **Note**: If you set the folder or engine for the view on the fly it will override the context settings.