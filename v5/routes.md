Title: Routes
Description: Bypass the WordPress rewrite rules and use routes.

---

The WordPress rewrite rules are mighty. You can use them to create custom endpoints and paths. TypeRocket routes do not explicitly register WordPress rewrite rules. Instead, TypeRocket routes bypass the rewrite `wp_options` cache. However, TypeRocket routes do pass through the rewrite engine. This allows you to quickly route requests to TypeRocket controllers where you can manage the business logic of your application.

Because of TypeRocket's rewrite cache bypass, you do not need to flush the permalinks when you add a new route. However, TypeRocket routes do pass through the WordPress rewrite engine when a match is found.

When a TypeRocket route is matched, TypeRocket dynamically generates a WordPress rewrite rule and adds it to the top of the WordPress rewrite array to ensure it will be the route used. TypeRocket routes will be favored over WordPress routes, to prevent plugins and WordPress content from breaking your custom application code.

## Routes File

In the TypeRocket root directory, locate the `routes/public.php` file. This file is where you will add all of your custom routes. Routes are mapped from the root path of your domain and are not used for admin pages.

## Quick Start

Let's create a quick route to see things in action. Add the following in your `routes/public.php` file to create a custom route.

```php
tr_route()->get()->on('hi/*', function($arg) {
    // Will return... Hi, {url_segment}
    return 'Hi, ' . esc_html($arg);  
});
```

Then visit the URL `hi/typerocket`. You should see:

```
Hi, typerocket
```

Then visit the URL `hi/kevin`. You should see:

```
Hi, kevin
```

Now, let's get more interesting.

```php
tr_route()->get()->on('hi/*/my/*', function($arg, $arg2) {  
    return 'Hi, ' . esc_html($arg) . ' ' . esc_html($arg2);  
});
```

Then visit the URL `hi/typerocket/my/pro`. You should see:

```
Hi, typerocket pro
```

Well done! You created a custom route without needing any WordPress rewrite rule madness.

Now, let dive deep and get a little more involved by using class-based controllers and powerful RegEx path matched routes.

## Getting Started

To get started, you will need a controller and a model. To create a controller and model, use the TypeRocket Galaxy CLI. The galaxy `make:controller` command helps create controller and model files for you.

To create a blank controller file named `MemberController` and base model file call `Member` in the correct `app` folders run:

```shell
php galaxy make:controller -m thin Member
```

Or, create the files manually in your `app/Controllers` and `app/Models` folders.

- [Controllers](https://l.rb.typerocket.test/docs/v5/controllers/)
- [Models](https://l.rb.typerocket.test/docs/v5/models/)

## Registering a Route

Now, in the `routes/public.php` file, add a new GET and POST route for a login page. For example, `https://example.com/login/`. The `match()` method is for the route path, and the `do()` method is for the route handler. 

Note, in this example we are using the TypeRocket shorthand in the `do()` method. Shorthand automatically assigns a controller for us.

```php
tr_route()->get()->match('login')->do('login@Member');
tr_route()->post()->match('login')->do('auth@Member');
```

The `get` route will map to the `MemberController` and call the public `login` method when an HTTP GET request is sent to the `/login/` URL.

The `post` route will map to the `MemberController` and call the public `auth` method when an HTTP POST request is sent to the `/login/` URL.

All that is left is to create the defined methods in the `MemberController` to handle the request.

*Note: "Controller" is omitted at the end of the string "Member". You should not append "Controller". TypeRocket handles adding it for you.*

## Route Types

There are five route types: `get`, `post`, `put`, `delete`, `any`.

### Get

The `get()` method will register a route for a GET request at a given URL path.

```php
tr_route()->get()->match('my/custom/path')->do('method@Controller');
```

### Post

The `post()` method will register a route for a POST request at a given URL path. POST requests should be used when creating a new controller.

```php
tr_route()->post()->match('my/custom/path')->do('method@Controller');
```

### Put

The `put()` method will register a route for a PUT request at a given URL path. PUT requests should be used when updating a controller.

```php
tr_route()->put()->match('my/custom/path')->do('method@Controller');
```

### Delete

The `delete()` method will register a route for a DELETE request at a given URL path. DELETE requests should be used when deleting a controller.

```php
tr_route()->delete()->match('my/custom/path')->do('method@Controller');
```


### Any

The `any()` method will register a route for a GET, POST, PUT, and DELETE request at the same time for a given URL path.

```php
tr_route()->any()->match('my/custom/path')->do('method@Controller');
```

Can also be done this way,

```php
tr_route()->post()->get()->put()->delete()->match('my/custom/path')->do('method@Controller');
```

## Route Controllers

You can set the controller of a route using the `do()` method. As notes, before you can use the shorthand for the `do()` method or pass in a callable. 

### Classes

You can also set a class callable as the route handler.

```php
tr_route()->get()->match('profile/1')->do(['\App\Controllers\MemberController', 'show']);
```

### Anonymous Function

You can also set an anonymous function as the route handler.

```php
tr_route()->get()->match('profile/1')->do(function() {
    return 'Hi Member 1!';
});
```

### Shorthand

TypeRocket route shorthand consist of two components: action and controller. Take a look at this example,

```php
tr_route()->get()->match('login')->do('login@Member');
```

The action component, `login`, of this shorthand calls the `login()` method of the controller; and the `Member` string selects `\App\Controllers\MemberController` as the controller.

#### Contoller Override

You can override what controller is used by using the full class name of the controller.  

```php
tr_route()->get()->match('login')->do('login@\MyPlugin\Controllers\MemeberController' );
```

## Route Matching

The `match()` method will match a route based on regular expressions. You can use [https://regex101.com/](https://regex101.com/) to help you create a URL route match.

### Variable Paths with RegEx Capture Groups

In some cases, you will want to have variable paths. For example, when you view a member profile, you may have a URL like `https://example.com/profile/1` where `1` is the user's ID. In this case, the user ID needs to a variable.

```php
tr_route()->get()->match('profile/([^\/]+)', ['id'])->do(function($id) {
    return "Hi Member {$id}!";
});
```

Let's examine this example route further.

### Single Variable

To make a variable path, use regular expression capture groups in the `match()` method. RegEx capture groups will become variables and get passed to your controller. 

Capture groups can be named (in this case is named `id`). To name the captured group, pass the second argument of the `match()` method an array with values for each group you are trying to capture. Doing this will use the value of the array as the variable name for the controller. The variable's value will be set the value captured by the RegEx from the URL.

For example, visiting the URL `example.com/profile/1` using the following route will output `Hi Member 1!`.

```php
tr_route()->get()->match('profile/([^\/]+)', ['id'])->do(function($id) {
    return "Hi Member {$id}!";
});
```

If capture groups are not named, then the variable's name will not determine the placement of the value. The order of the arguments will determine the placement of the values.  This means the following is the same as the above.

```php
tr_route()->get()->match('profile/([^\/]+)')->do(function($arg) {
    return "Hi Member {$arg}!";
});
```

### Multiple Variables

You can use as many variables as you like, but they must all have a unique name.

```php
tr_route()
    ->get()
    ->match('profile/([^\/]+)/address/([^\/]+)', ['profile_id', 'address_id'])
    ->do(function($profile_id, $address_id) {
        return "Hi Member {$profile_id} with address {$address_id}!";
    });
```

### Sending Vars to Controllers

Any time a route variable is set, its value can be sent to the controller by setting a controller argument to the same name as the path variable.

For the GET request to `https://example.com/profile/1` you might have the route:

```php
tr_route()->get()->match('profile/([^\/]+)', ['id'])->do('profile@Member');
```

TypeRocket makes mapping the variables easy by injecting them right into the controller when requested. Again, the argument must have the same name as the path variable. 

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

## Quick Routes

You can quickly add routes using the `on()` method. This method takes two arguments: match and controller.

- **Match**: A string value but all `*` chars will be replaced with the capture group `([^\/]+)`.
- **Controller**: the same value you would use for the `do()` method.

```php
tr_route()->get()->on('hi/*/my/*', function($arg, $arg2) {  
    return 'Hi, ' . $arg . ' ' . $arg2;  
});
``` 

## Securing Routes

By default, TypeRocket opens all custom routes to the public. To disallow access to public routes, you need to add the HTTP kernel middleware `AuthRead::class` in your `App\Http\Kernel` class to the `http` group. The Kernel class is located under `app/Http/Kernel.php`.

```php
// ...

class Kernel extends \TypeRocket\Http\Kernel
{
    protected $middleware = [
        'hooks' => [],
        'http' =>
            [
                AuthRead::class, // added
                VerifyNonce::class
            ],
        'post' => [ CanEditPosts::class ],
        // ...
    ];
}
```

Once you have added the `AuthRead` access will be blocked unless a user has `read` capability.

## Middleware

You can set the middleware for a route using the middleware group name. For example, this a route with the group of `post` will run all the middleware in that group. The `post` group contains `CanEditPosts`.

```php
tr_route()
    ->put()
    ->middleware('post')
    ->match('my-api/post/([^\/]+)')
    ->do('update@Post');
```

You can always create your middleware groups. Also, you can pass an array of middleware instead of the group name. You can not pass an array of groups.

## Route File Extension Spoofing

You may want to create a sitemap for your site where the URL path is `/seo/sitemap.xml`. To achieve this, you need to use `noTrailingSlash()` the method o your route definition.

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

        tr_response()->send('xml');

        return tr_view('sitemap.index', compact('pages'));
    }
}
```

## Named Routes

At times you will want to access your routes to generate URLs or for other purposes. Naming a route makes it easy to lookup after it has been registered.

To name a route, use the `name()` method.

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
$route = tr_route_url('users.jobs');
```

Or, you can generate a URL using the `match` keys.

```php
// Generate URL from route
$get_with_site_url = false;

$url = tr_route_url('users.jobs', [
    'user_id' => 1,
    'job_id' => 24
], $get_with_site_url);
```

### Forms

You can also access named routes on a `Form`. A `Form` will use `Model` data to map values into the `<form action="url_here">` element. Or, you can provide your values to override the model's data.

```php
// Using model data
tr_form('\App\Models\UserJob', 1)
    ->useRoute('users.jobs');
```

```php
// Using your own values
$values = [
    'user_id' => 1,
    'job_id' => 24
];

tr_form('\App\Models\UserJob', 1)
    ->useRoute('users.jobs', $values);
```

## Routes For Plugins & Themes

If you need to set routes from within a TypeRocket plugin dynamically, you can use the `tr_routes` action hook. For example, if you wanted to have a custom plugin create a sitemap for your site.

```php
add_action('tr_routes', function() {
    tr_route()
        ->get()
        ->noTrailingSlash()
        ->match('seo/sitemap.xml')
        ->do([\MyPlugin\Controllers\SitemapController::class, 'index']);
});
```