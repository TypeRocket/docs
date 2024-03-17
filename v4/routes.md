Title: Routes
Description: Bypass the WordPress rewrite rules and use routes.

---

The WordPress rewrite rules are extremely powerful. You can use them to create custom endpoints and paths. TypeRocket routes do not explicitly register WordPress rewrite rules. Instead, TypeRocket routes bypass the rewrite `wp_options` cache. However, TypeRocket routes do pass through the rewrite engine. This allows you to quickly route requests to TypeRocket controllers where you can manage the business logic of your application.

Because of TypeRocket's rewrite cache bypass, you do not need to flush the permalinks when you add a new route. However, TypeRocket routes do pass through the WordPress rewrite engine when a match is found.

When a TypeRocket route is matched TypeRocket dynamically generates a WordPress rewrite rule and adds it to the top of the WordPress rewrite array to ensure it will be the route used. TypeRocket routes will be favored over WordPress routes, to prevent plugins and WordPress content from breaking your custom application code.

## Routes File

In the TypeRocket root directory locate the `routes.php` file. This file is where you will add all of your custom routes. Routes are mapped form from root path of your domain and are for none admin pages.

## Setup

To get started, you will need a controller and a model. To create a controller and model use the TypeRocket Galaxy CLI. The galaxy `make:controller` command helps create controller and model files for you.

To create a blank controller file named `MemberController` and base model file call `Member` in the correct `app` folders run:

```shell
php galaxy make:controller -m thin Member
```

Or, create the files manually in your `app/Controllers` and `app/Models` folders.

- [Controllers](https://typerocket.com/docs/v4/controllers/)
- [Models](https://typerocket.com/docs/v4/models/)

*Note: The Galaxy command-line tool is not available if you are using the official TypeRocket Framework 4 WordPRess plugin.*

## Registering a Route

Now, in the `routes.php` file add a new GET and POST route for a login page. For example, `https://example.com/login/`. The `match()` method is for the route path and the `do()` method is for the route handler. 

Note, in this example, we are using the TypeRocket quick route declaration in the `do()` method. Quick route declarations automatically assign a controller and middleware group for us.

```php
tr_route()->get()->match('login')->do('login@Member');
tr_route()->post()->match('login')->do('auth@Member');
```

The `get` route will map to the `MemberController` and call the public `login` method when an HTTP GET request is sent to the `/login/` URL.

The `post` route will map to the `MemberController` and call the public `auth` method when an HTTP POST request is sent to the `/login/` URL.

All that is left is to create the defined methods in the `MemberController` to handle the request.

## Route Matching

The `match()` method will match a route based on regular expressions. You can use [https://regex101.com/](https://regex101.com/) to help you create a URL route match.

## Route Types

There are five route types: `get`, `post`, `put`, `delete`, `any`.

### Get

The `get()` method will register a route for a GET request at a given URL path.

```php
tr_route()->get()->match('my/custom/path')->do('method@Resource');
```

### Post

The `post()` method will register a route for a POST request at a given URL path. POST requests should be used when creating a new resource.

```php
tr_route()->post()->match('my/custom/path')->do('method@Resource');
```

### Put

The `put()` method will register a route for a PUT request at a given URL path. PUT requests should be used when updating a resource.

```php
tr_route()->put()->match('my/custom/path')->do('method@Resource');
```

### Delete

The `delete()` method will register a route for a DELETE request at a given URL path. DELETE requests should be used when deleting a resource.

```php
tr_route()->delete()->match('my/custom/path')->do('method@Resource');
```


### Any

The `any()` method will register a route for a GET, POST, PUT, and DELETE request at the same time for a given URL path.

```php
tr_route()->any()->match('my/custom/path')->do('method@Resource');
```

Can also be done this way,

```php
tr_route()->post()->get()->put()->delete()->match('my/custom/path')->do('method@Resource');
```

## Route Callbacks

You can also set an anonymous function as the route handler.

```php
tr_route()->get()->match('profile/1')->do(function() {
    return 'Hi Member 1!';
});
```

Or, with a variable path:

```php
tr_route()->get()->match('profile/(.+)', ['id'])->do(function($id) {
    return "Hi Member {$id}!";
});
```


## Variable Paths with RegEx Capture Groups

In some cases, you will want to have variable paths. For example, when you view a member profile you may have a URL like `https://example.com/profile/1` where `1` is the user's ID. In this case, the user ID needs to a variable.

### Single Variable

To make a variable path use regular expression capture groups in the `match()` method. RegEx capture groups will become variables and get passed to your controller. 

Further, capture groups must be named. To name the captured group pass the second argument of the `match()` method an array with values for each group you are trying to capture. Doing this will use the value of the array as the variable name for the controller. The variable's value will be set the value captured by the ReGex from the URL.

For example, visiting the URL `example.com/profile/123` using the following route will output `Hi Member 123!`.

```php
tr_route()->get()->match('profile/(.+)', ['id'])->do(function($id) {
    return "Hi Member {$id}!";
});
```

### Multiple Variables

You can use as many variables as you like but they must all have a unique name.

```php
tr_route()
    ->get()
    ->match('profile/(.+)/address/(.+)', ['profile_id', 'address_id'])
    ->do(function($profile_id, $address_id) {
        return "Hi Member {$profile_id} with address {$address_id}!";
    });
```

### Stricter Matching Recommended

In many cases, the regex capture group `(.+)` will be aggressively greedy. It is better to use a stricter capture group pattern like `([^\/]+)`.

The capture group `([^\/]+)` is the recommended regex pattern.

*Note: The capture group `(.+)` is used in our examples for help readability of the documentation.*

### App Controllers

Any time a route variable is set its value can be sent to the controller by setting a controller argument to the same name as the path variable.

For the GET request to `https://example.com/profile/1` you might have the route:

```php
tr_route()->get()->match('profile/(.+)', ['id'])->do('profile@Member');
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

By default, TypeRocket opens all custom routes to the public; this is the opposite of version 3. To disallow access to public routes you need to add the HTTP kernel middleware `AuthRead::class` in your `App\Http\Kernel` class to the `resourceGlobal` group. The Kernel class is located under `app/Http/Kernel.php`.

```php
class Kernel extends \TypeRocket\Http\Kernel
{
    protected $middleware = [
        'hookGlobal' => [],
        'resourceGlobal' =>
            [
                AuthRead::class, // added
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

Once you have added the `AuthRead` access will be blocked.

### Public Controllers

When using a controller with a custom public route you need to create a middleware group for that controller. The middleware group must match the resource name of the controller. For example, in the controller member example above the resource is `Member`. Therefore, in the `Kernel` class you must create a new group of `member`; then set that group to an empty array.

```php
class Kernel extends \TypeRocket\Http\Kernel
{
    protected $middleware = [
        'hookGlobal' => [],
        'resourceGlobal' =>
            [
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

## Route File Extension Spoofing

In the new TypeRocket v4.0 routing system, you can now define yours that use file extensions. For example, you may want to create a sitemap for your site where the URL path is `/seo/sitemap.xml`. To achieve this you need to use `noTrailingSlash()` the method o your route definition.

### Sitemap Example

In your `routes.php` file:

```php
tr_route()
    ->get()
    ->noTrailingSlash()
    ->match('seo/sitemap.xml')
    ->do('index@Sitemap');
```

This will point all traffic from `/seo/sitemap.xml` to the custom controller `Sitemap`. Here is what the controller might look like:

```php
<?php

namespace App\Controllers;

use TypeRocket\Controllers\Controller;

class SitemapController extends Controller
{
    public function index()
    {
        $pages = (new \WP_Query([
            'post_type' => ['page', 'post'],
            'posts_per_page' => -1,
            'post_status' => 'publish'
        ]))->get_posts();

        header("Content-type: text/xml");

        return tr_view('sitemap.index', compact('pages'));
    }
}
````

## Quick Route Declarations

TypeRocket quick route decorations consist of three components: action, resource, and controller. While the controller component is not required the action and resources are required. A quick route declaration without a controller specified follows this syntax `action@Resource`. Take a look at this example,

```php
tr_route()->get()->match('login')->do('login@Member');
```

The action component, `login`, of this quick route declaration calls the `login()` method of the controller; and the resource component, `Member`, selects `\App\Controllers\MemberController` as the controller and set the middleware group to `member` in the `Kernel`.

### Controller Override

By default, TypeRocket uses the resource declaration of in the route `do()` method to automatically load the correct controller for that route. For example, `tr_route()->do('index@Post')` load the `\App\Controllers\PostController` controller. However, you can override what controller is used by including the controller component of the quick route declaration.  

```php
tr_route()->get()->match('about/(.+)', ['id'])->do('index@Post:\MyPlugin\Controllers\PostController' );
```

## Named Routes

At times you will want to access your routes to generate URLs or for other purposes. Naming a route makes it easy to lookup after it has been registered.

To name a route use the `name()` method.

```php
// Register named route
tr_route()
    ->get()
    ->match('/user/([^\/]+)/job/([^\/]+)', ['user_id', 'job_id'])
    ->name('users.jobs');
```

Once a route is named, you can look it up from the DI Container.

```php
// Lookup route
$route = tr_route_lookup('users.jobs');
```

Or, you can generate a URL using the `match` keys.

```php
// Generate URL from route
$get_with_site_url = false;

$url = tr_route_url_lookup('users.jobs', [
    'user_id' => 1,
    'job_id' => 24
], $get_with_site_url);
```

### Forms

You can also access named routes on a `Form`. A `Form` will use `Model` data to map values into the `<form action="url_here">` element. Or, you can provide your own values to override the model's data.

```php
// Using model data
tr_form('job', 'update', 1, new \App\Models\UserJob)
    ->useRoute('users.jobs');
```

```php
// Using your own values
$values = [
    'user_id' => 1,
    'job_id' => 24
];

tr_form('job', 'update', 1, new \App\Models\UserJob)
    ->useRoute('users.jobs', $values);
```

## Route Hooks

If you need to set routes from within a TypeRocket plugin dynamically you can use the `tr_load_routes` action hook. For example, if you wanted to have a custom plugin create a sitemap for your site.

```php
add_action('tr_load_routes', function() {
    tr_route()
        ->get()
        ->noTrailingSlash()
        ->match('seo/sitemap.xml')
        ->do([\MyPlugin\Controllers\SitemapController::class, 'index']);
});
```

*Note: The tr_load_routes hook fire when typerocket is loaded so if your plugin code is loaded after typerocket your routes will not work. For example, an MU plugin install of Typerocket will not let your WordPress plugins use the tr_load_routes hook.* 