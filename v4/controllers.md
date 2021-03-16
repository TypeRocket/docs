Title: Controllers
Description: Controllers define how a resource is created, updated, read, deleted or take any action.

---

## About Controllers

Controllers define what happens when a resource is created, updated, read, deleted or any action is taken. There are three main controller classes in TypeRocket: `Controller`, `WPPostController`, and `WPTermController`. `WPTermController` and `WPPostController` both extend the base `Controller` class but have a few extra functions.

## Galaxy CLI

There are four Galaxy CLI commands "directives" for making a controller: `base`, `thin`, `post`, and `term`.

1. `base` - Make a base controller with the methods `index`, `add`, `create`, `edit`, `update`, `show`, `delete`, and `destroy`.
2. `thin` - Make a base controller with no methods.
3. `post` - Make a controller with no methods extending the `WPPostController`.
4. `term` - Make a controller with no methods extending the `WPTermController`.

*Note: The Galaxy command-line tool is not available if you are using the official TypeRocket Framework 4 WordPRess plugin.*

## Base Controller

Take a look at making a `base` controller with a model.

```bash
php galaxy make:controller -m base Person
```

Will product a basic thin controller class, see below, but with these predefined methods: `index`, `add`, `create`, `edit`, `update`, `show`, `delete`, and `destroy`. If you use the `thin` directive these methods will not be created.

```bash
php galaxy make:controller -m thin Person
```

Will produce `app/Controllers/PersonController.php`,

```php
<?php
namespace App\Controllers;

use \TypeRocket\Controllers\Controller;

class PersonController extends Controller
{
}
```

If you want to manually create a controller you can use the example above as your template example.

## Post Controller

The `WPPostController` is used for working with post types. The great part of this controller class is that it will be automatically setup to handling updating and creating post records.

Because the `WPPostController` automatically has `update()` and `create()` methods you do not need to worry about custom post types if you make a controller extending this class.

### Hooks

Because WordPress does not use an MVC architecture TypeRocket spoofs this design pattern for you when a post is updated on the edit and add post pages. In fact, TypeRocket spoofs this anytime the `save_post` action is called to ensure consistency.

### Making

To make a post controller use the Galaxy CLI.

```bash
php galaxy make:controller -m post Person
```

Will produce `app/Controllers/PersonController.php`,

```php
<?php
namespace App\Controllers;

use \App\Models\Person;
use \TypeRocket\Controllers\WPPostController;

class PersonController extends WPPostController
{
    protected $modelClass = Person::class;
}
```

## Term Controller

The `WPTermController` is used for working with taxonomy terms. The great part of this controller class is that it will be automatically setup to handling updating and creating post records.

Because the `WPTermController` automatically has `update()` and `create()` methods you do not need to worry about taxonomy terms if you make a controller extending this class.

### Hooks

Because WordPress does not use an MVC architecture TypeRocket spoofs this design pattern for you when a term is updated on the edit and add taxonomy term pages. In fact, TypeRocket spoofs this anytime the `create_term` or `edit_term ` action is called to ensure consistency.

### Making

To make a term controller use the Galaxy CLI.

```bash
php galaxy make:controller -m term Person
```

Will produce `app/Controllers/PersonController.php`,

```php
<?php
namespace App\Controllers;

use \App\Models\Person;
use \TypeRocket\Controllers\WPTermController;

class PersonController extends WPTermController
{
     protected $modelClass = Person::class;
}
```

## Dynamically Set Middleware

There are times when you do not want the global resource [middleware](/docs/v4/middleware/) stack to be called for specific methods of a controller by the [kernel](/docs/v4/kernel/). To dynamically set the middleware stack use the `routing()` method on a controller and the `setMiddleware()` method.

For example, take a member controller where you want to have different middleware for the login methods.

```php
<?php
namespace App\Controllers;

class MemberController extends Controller
{    
    public function routing()
    {
        $this->setMiddleware('not_member', ['only' => ['login', 'login_verify']]);
    }

    /*
     * When this method is called the "not_member"
     * middleware stack will be called by the 
     * kernel.
     */
    public function login()
    {
        return tr_view('members.login');
    }

    public function login_verify()
    {
        $redirect = tr_redirect();
        
        if( $id = auth_verify() ) {
            $redirect->toUrl( home_url('/members/' . $id ));
        } else {
            $redirect->back();
        }
         
        return $redirect;
    }

    // ...
}
```

In the `Kernel` the middleware might have these settings. It is important to notice that the `member` middleware group is an empty array. All methods of a controller must call a middleware group of the name of the resource it is connected to. 

```php
class Kernel extends \TypeRocket\Http\Kernel
{
    protected $middleware = [
        'hookGlobal' => [ AuthRead::class ],
        'resourceGlobal' => [ Middleware\VerifyNonce::class ],
        'noResource' => [ AuthAdmin::class ],
        // ...
        'member' => [], // empty
        'not_member' => [
            Middleware\Nonmember::class
        ],
    ];
}
```

### Set Middleware

The `setMiddleware()` method must be called from the controllers `routing()` method and takes two arguments:

1. `$group` - The middleware group to call from the kernel.
2. `$settings` - An array of settings defining the `only` or `except` methods to call in the middleware group.

#### Only

Only call the `not_member` middleware group on the `login` and `login_verify` controller methods.

```php
$this->setMiddleware('not_member', ['only' => ['login', 'login_verify']]);
```

#### Except

Call the `is_member` middleware group on all methods but the `login` and `login_verify` controller methods.

```php
$this->setMiddleware('is_member', ['except' => ['login', 'login_verify']]);
```

## Working with Controllers

To start working with controllers. You need to understand the [MVC design pattern](https://en.wikipedia.org/wiki/Model-view-controller). To help you get started let's take a look at a simple public JSON API built using MVC and TypeRocket.

### Controller JSON API Example

For this example, we will create an endpoint that lists the latest published posts in WordPress. In the TypeRocket `routes.php` file [add a route](https://typerocket.com/docs/v4/routes/#section-route-types) to the URL `/posts/published/latest/` for any GET requests. Then, point that route to a method on the `ApiController`.

```php
tr_route()->get()->match('/posts/latest/')->do('latest@Api');
```

When a user visits the URL `http://example.com/posts/published/latest/` it will call the `latest()` method on the `PostController` in the `app/Controllers/` folder. Let's create the `ApiController` and implement `latest()` method now.

```bash
php galaxy make:controller -m thin Api
```

In the newly created controller add the method `latest()`.

```php
<?php
namespace App\Controllers;

use App\Models\Post;
use TypeRocket\Controllers\Controller;

class ApiController extends Controller
{
    public function latest() {
         $latest = (new \App\Models\Post())->published()->take(10)->get();    
         return $latest;
    }
}
```

Because we are returning an `array` TypeRocket will format the view as JSON. Now you just need to make the API public by setting the `api` resource middleware stack to an empty array. We also need to remove the `AuthRead::class` from the `resourceGlobal` array.

```php
class Kernel extends \TypeRocket\Http\Kernel
{
    protected $middleware = [
        'hookGlobal' => [ AuthRead::class ],
        'resourceGlobal' => [ Middleware\VerifyNonce::class ],
        'noResource' => [ AuthAdmin::class ],
        // ...
        'api' => [],
    ];
}
```

You now have a public JSON API.

*Note: Be sure to take the security of your site into account when making public APIs.*

### Simplest JSON API Example

Also, you can create a much simpler JSON API using just the route. This required no controller, middleware, or kernel setup.

This example will have the same net result as the controller example.

```php
tr_route()->get()->match('/posts/latest/')->do(function() {
    return (new \App\Models\Post())->published()->take(10)->get();
});
```