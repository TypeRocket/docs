Title: Middleware
Description: Middleware components allow  you to filter HTTP requests.

---

## About Middleware

Middleware are the components that allow you to take an action before or after the `Response` and `Request` are send to the resources `Controller`.

TypeRocket middleware works in a similar way to [Laravel middleware](http://laravel.com/docs/master/middleware).

You can define what middleware stack is used by resource using the [XKernel](/docs/kernel-and-xkernel/).

## Locating Middleware

You should locate your middleware classes in `typerocket/app/Http/Middleware` folder and use the namespace `TypeRocket\Http\Middleware`.

### Example: Authorized Read

Take a look at the `AuthRead` middleware.

```php
<?php
namespace TypeRocket\Http\Middleware;

/**
 * Class AuthRead
 *
 * Authenticate user has read access and if the user does not
 * invalidate the response.
 *
 * @package TypeRocket\Http\Middleware
 */
class AuthRead extends Middleware
{
    public function handle()
    {
        if ( ! current_user_can('read')) {
            $this->response->setInvalid();
            $this->response->setError( 'auth', false );
            $this->response->setStatus(401);
            $this->response->setMessage( "Sorry, you don't have enough rights." );
        }
        $this->next->handle();
    }
}
```

## Creating Middleware

When you create you own middleware any code you add before `$this->next->handle()` take place before the controller and any code after gets called after the controller.

```php
<?php // typerocket/app/Http/Middleware/YourMiddleware.php
namespace TypeRocket\Http\Middleware;

class YourMiddleware extends Middleware
{
    public function handle()
    {
        /**
         * Before controller is called
         **/

        $this->response; // access response object
        $this->request; // access request object

        $this->next->handle(); // call next middleware

        /**
         * After controller is called
         **/
    }
}
```