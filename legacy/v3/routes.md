Title: Routes
Description: Bypass the WordPress rewrite rules and use routes.

---

The WordPress rewrite rules are extremely powerful. You can use them to create custom endpoints and paths. TypeRocket routes do not explicitly register WordPress rewrite rules. Instead, TypeRocket routes bypass the rewrite `wp_options` cache. However, TypeRocket routes do pass through the rewrite engine. This allows you to quickly route requests to TypeRocket controllers where you can manage the business logic of your application.

Because of TypeRocket's rewrite cache bypass, you do not need to flush the permalinks when you add a new route. However, TypeRocket routes do pass through the WordPress rewrite engine when a match is found.

When a TypeRocket route is matched TypeRocket dynamically generates a WordPress rewrite rule and adds it to the top of the WordPress rewrite array to ensure it will be the route used. TypeRocket routes will be favored over WordPress routes, to prevent plugins and WordPress content from breakig your custom application code.

## Routes File

In the TypeRocket root directory locate the `routes.php` file. This file is where you will add all of your custom routes. Routes are mapped form from root path of your domain and are for none admin pages.

## Setup

To get started, you will need a controller and model. To create a controller and model use the TypeRocket Galaxy CLI. The galaxy `make:controller` command helps create controller and model files for you.

To create a blank controller file named `MemberController` and base model file call `Member` in the correct `app` folders run:

```shell
php galaxy make:controller -m thin Member
```

## Registering a Route

Now, in the `routes.php` file add a new GET and POST route for a login page. For example, `https://example.com/login/`. this first argument is for the route path and the second is for the route handler.

```php
tr_route()->get('/login/', 'login@Member');
tr_route()->post('/login/', 'auth@Member');
```

The `get` route will map to the `MemberController` and call the public `login` method when an HTTP GET request is sent to the `/login/` URL.

The `post` route will map to the `MemberController` and call the public `auth` method when an HTTP POST request is sent to the `/login/` URL.

All that is left is to create the defined methods in the `MemberController` to handle the request.

## Route Types

There are five route types: `get`, `post`, `put`, `delete`, `any`.

### Get

The `get()` method will register a route for a GET request at a given URL path.

```php
tr_route()->get('/my/custom/path/', 'method@Resource');
```

### Post

The `post()` method will register a route for a POST request at a given URL path. POST requests should be used when creating a new resource.

```php
tr_route()->post('/my/custom/path/', 'method@Resource');
```

### Put

The `put()` method will register a route for a PUT request at a given URL path. PUT requests should be used when updating a resource.

```php
tr_route()->put('/my/custom/path/', 'method@Resource');
```

### Delete

The `delete()` method will register a route for a DELETE request at a given URL path. DELETE requests should be used when deleting a resource.

```php
tr_route()->put('/my/custom/path/', 'method@Resource');
```


### Any

The `any()` method will register a route for a GET, POST, PUT, and DELETE request at the same time for a given URL path.

```php
tr_route()->any('/my/custom/path/', 'method@Resource');
```

## Route Callbacks

You can also set an anonymous function as the route handler. However, anonymous functions do not work with variable paths the same way controllers do.

```php
tr_route()->get('/profile/1/', function() {
    return 'Hi Member 1!';
});
```

Or, with a variable path:

```php
tr_route()->get('/profile/{id}', function( $path, \TypeRocket\Http\Request $request, $wilds ) {
   echo $path;
   var_dump($wilds); // will output the value of {id} in an array.
});
```

## Variable Paths

In some cases, you will want to have variable paths. For example, when you view a member profile you may have a URL like `https://example.com/profile/1` where `1` is the user's ID. In this case, the user ID needs to a variable.

### Single Variable

To make a variable path use curly braces in the route path.

```php
tr_route()->get('/profile/{id}', 'profile@Member');
```

### Multiple Variables

You can use as many variables as you like but they must all have a unique name.

```php
tr_route()->get('/profile/{user}/company/{company}', 'company@Member');
```

## Send Variables to Controllers

Any time a route variable is set its value can be sent to the controller by setting a controller argument to the same name as the path variable.

For the GET request to `https://example.com/profile/1` you might have the route:

```php
tr_route()->get('/profile/{id}', 'profile@Member');
```

TypeRocket makes mapping the variables easy injecting them right into the controller when requested. Again, the argument must have the same name as the path variable. 

```php
namespace App\Controllers;

use TypeRocket\Controllers\Controller;

class MemberController extends Controller
{
    public function profile( $id ) {
        return return "Hi Member {$id}!";
    }
}
```

## Public Routes

By default, TypeRocket blocks all public access to custom routes as a security feature. To allow access to public routes you need to disable the HTTP kernel middleware `AuthRead::class` in your `App\Http\Kernel` class from the `resourceGlobal` group. The Kernel class is located under `app/Http/Kernel.php`. In this file comment out the `AuthRead::class`.

```php
class Kernel extends \TypeRocket\Http\Kernel
{
    protected $middleware = [
        'hookGlobal' => [],
        'resourceGlobal' =>
            [
                // AuthRead::class,
                Middleware\VerifyNonce::class
            ],
        'noResource' =>
            [ AuthAdmin::class ],
        'user' =>
            [ IsUserOrCanEditUsers::class ],
        'post' =>
            [ OwnsPostOrCanEditPosts::class ],
        ....
    ];
}
```

Once you have commented out the `AuthRead` class you can then take the final step.

### Public Controllers

When using a controller with a custom public route you need to create a middleware group for that controller. The middleware group must match the resource name of the controller. For example, in the controller member example above the resource is `Member`. Therefore, in the `Kernel` class you must create a net group of `member`; then set that group to an empty array.

```php
class Kernel extends \TypeRocket\Http\Kernel
{
    protected $middleware = [
        'hookGlobal' => [],
        'resourceGlobal' =>
            [
                // AuthRead::class,
                Middleware\VerifyNonce::class
            ],
        'member' => [], // New member group
        'noResource' =>
            [ AuthAdmin::class ],
        'user' =>
            [ IsUserOrCanEditUsers::class ],
        'post' =>
            [ OwnsPostOrCanEditPosts::class ],
        ....
        
    ];
}
```

If you chose not to create a new group the default group will be the `noResource` group.

