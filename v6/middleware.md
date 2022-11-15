Title: Middleware
Description: Middleware components allow  you to filter HTTP requests.

---

## Http Middleware

Middleware allows you to take action before or after the `Response` is sent. TypeRocket middleware works like [Laravel middleware](http://laravel.com/docs/master/middleware).

## Locating middleware

You should locate your middleware classes in `typerocket/app/Http/Middleware` folder and use the namespace `App\Http\Middleware`.

## Creating Middleware

When you create you own middleware any code you add before `$this->next->handle()` take place before the controller and any code after gets called after the controller.

Run the galaxy command `make:middleware` to create your first one.

```shell
php galaxy make:middleware Example
```

Will create,

```php
<?php
namespace App\Http\Middleware;

use \TypeRocket\Http\Middleware\Middleware;

class Example extends Middleware
{

    public function handle()
    {
        $request = $this->request;
        $response = $this->response;

        // Do stuff before the controller is called

        $this->next->handle();

        // Do stuff after the controller is called
    }
}
```

Once you have created your middleware class, you must register it to use it.

## Register Middleware

To register your middleware, you should add it to an existing middleware group or create a new middleware group in your `app/Http/Kernal.php` file.

Here, we can add an example middleware group called `my_group` and add the `Example` middleware to it.

```php
// ...

class Kernel extends \TypeRocket\Http\Kernel  
{  
    protected $middleware = [  
		// ...
		'search' => [ AuthAdmin::class ],
		'my_group' => [ App\Http\Middleware\Example::class ],
  ];  
}
```

## TypeRocket REST API Middleware

The TypeRocket REST API automatically detects middleware groups based on the resource type.

For example, post types look for a middleware group in the kernel that matches their registered ID. That group is not found the middleware group defaults to the `post` group. Taxonomies work in the same way but default to the `term` group.

It is **IMPORTANT** to note, [custom resources](/docs/v6/custom-resources/) will look for their group by resource ID; but, custom resources have the fallback group `rest`. Be sure you create a middleware group for any custom resources unless you want them to be use the `rest` middleware fallback group.