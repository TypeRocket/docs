Title: Middleware
Description: Middleware components allow  you to filter HTTP requests.

---

Middleware allows you to take an action before or after the `Response` and `Request` are send to the resources `Controller`.

TypeRocket middleware works in a similar way to [Laravel middleware](http://laravel.com/docs/master/middleware).

You can define what middleware stack is used by resource using the [Kernel](/docs/v4/kernel/).

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

        // Do stuff before controller is called

        $this->next->handle();

        // Do stuff after controller is called
    }
}
```

*Note: The Galaxy command-line tool is not available if you are using the official TypeRocket Framework 4 WordPRess plugin.*