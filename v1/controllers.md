Title: Controllers
Description: Controllers define how a resource is created, updated, read, deleted or take any action.

---

## Make Controller
There are four Galaxy CLI commands "directives" for making a controller: `base`, `thin`, `post`, and `term`. You can also create a controller manually using the example code to follow.

1. `base` - Make a base controller with the methods `index`, `add`, `create`, `edit`, `update`, `show`, `delete`, and `destroy`.
2. `thin` - Make a base controller with no methods.
3. `post` - Make a controller with no methods extending the `WPPostController`.
4. `term` - Make a controller with no methods extending the `WPTermController`.

## Base Controller

Take a look at making a `base` controller **with a model**.

```bash
php galaxy make:controller -m base Person
```

Will product a basic thin controller class, see below, but with these predefined methods: `index`, `add`, `create`, `edit`, `update`, `show`, `delete`, and `destroy`. If you use the `thin` directive, these methods will not be created.

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

*Note: To manually create a controller, you can use the example above as your template example.*

## Post Controller

The `WPPostController` is used for working with post types. Because the `WPPostController` automatically has `create()`, `update()`, and `destroy()` methods, you do not need to worry about custom post types if you make a controller extending this class.

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

### Hooks

Because WordPress does not use an MVC architecture TypeRocket spoofs this design pattern for you when a post is updated on the edit and add post pages. In fact, TypeRocket spoofs this anytime the `save_post` action is called to ensure consistency.

## Term Controller

The `WPTermController` is for working with taxonomy terms. Because the `WPTermController` automatically has `create()`, `update()` and `destroy()` methods, you do not need to worry about taxonomy terms if you make a controller extending this class.
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

### Hooks

Because WordPress does not use an MVC architecture TypeRocket spoofs this design pattern for you when a term is updated on the edit and add taxonomy term pages. In fact, TypeRocket spoofs this anytime the `create_term` or `edit_term ` action is called to ensure consistency.

## Dynamically Set Middleware

There are times when you do not want the global resource [middleware](/docs/v1/middleware/) stack to be called for specific methods of a controller by the [kernel](/docs/v1/kernel/). To dynamically set the middleware stack use the `__construct()` method on a controller and the `addMiddleware()` method.

For example, take a member controller where you want to have different middleware for the login methods.

```php
<?php
namespace App\Controllers;

class MemberController extends Controller
{    
    public function __construct()
    {
        $this->addMiddleware('not_member', ['only' => ['login', 'login_verify']]);
    }

    /*
     * When this method is called the "not_member"
     * middleware group stack will be called by the 
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

In the `Kernel` the middleware might have these settings.

```php
class Kernel extends \TypeRocket\Http\Kernel
{
    protected $middleware = [
        'hooks' => [],  
        'http' => [ VerifyNonce::class ],
        // ...
        'not_member' => [
            Middleware\Nonmember::class
        ],
    ];
}
```

### Add Middleware

The `addMiddleware()` method must be called from the controllers `__constructor()` method and takes two arguments:

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

To start working with controllers. You need to understand the [MVC design pattern](https://en.wikipedia.org/wiki/Model-view-controller). To help you get started, let's take a look at a simple public JSON API built using MVC and TypeRocket.

### Controller JSON API Example

For this example, we will create an endpoint that lists the latest published posts in WordPress. In the TypeRocket `routes/public.php` file [add a route](https://typerocket.com/docs/v1/routes/#section-route-types) to the URL `/posts/published/latest/` for any GET requests. Then, point that route to a method on the `ApiController`.

```php
tr_route()->get()->match('/posts/latest/')->do('latest@Api');
```

When a user visits the URL `http://example.com/posts/published/latest/` it will call the `latest()` method on the `ApiController` in the `app/Controllers/` folder. Let's create the `ApiController` and implement `latest()` method now.

```bash
php galaxy make:controller -m thin Api
```

In the newly created controller, add the method `latest()`.

```php
<?php
namespace App\Controllers;

use App\Models\Post;
use TypeRocket\Controllers\Controller;

class ApiController extends Controller
{
    public function latest() {
         $latest = (new \App\Models\Post)->published()->take(10)->get();    
         return $latest;
    }
}
```

Because we are returning an `array` TypeRocket will format the view as JSON. 

You can also secure the method using model policies accessed by the `can()` method. 

```php
public function latest(\TypeRocket\Models\AuthUser $user) {
	$post = new \App\Models\Post;
	
	// only logged-in users who can create a post
	// can also view the JSON API
	if(!$post->can('write', $user)) {
		tr_abort(401);
	}
	
    return $post->published()->take(10)->get();
}
```

*Note: Be sure to take the security of your site into account when making public APIs.*

### Simplest JSON API Example

Also, you can create a much simpler JSON API using just the route. This required no controller, middleware, or kernel setup.

This example will have the same net result as the controller example.

```php
tr_route()->get()->on('/posts/latest/', function() {
    return (new \App\Models\Post)->published()->take(10)->get();
});
```