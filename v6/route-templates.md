Title: Route Templates
Description: Use the WordPress rewrite rules with MVC and bypass a theme's templates.

---

!! 

## Getting Started

Typerocket [routes are powerful](/docs/v6/routes/). Also, you might want to use MVC with the built-in WordPress rewrite rules too. But, maybe you don't want to use the WordPress file based templating system. 

With route-templates you can completely replace for your theme's templates system in favor of TypeRocket style routes. This means you will no longer need to make WP templates. You can replace them with MVC completely.

The secret to this is the `tr_route_template()` function.

## About Route Templates

To add a route-template go to your routes file located at `routes/public.php` and create them there. Route-templates work like normal routes in every way with only a few key differences.

- Route-templates will respond no matter what request method is used with a default handler. Normal routes only respond to their defined request methods.
- Route-templates have global middleware only. Normal routes only use the middleware of their definition.
- Route-templates cannot be named.
- Route-templates do not match a given URL path. They match a [WordPress template name](https://developer.wordpress.org/themes/basics/template-hierarchy/).

! **Note**: Route-templates do not replace your theme's need for an `index.php` file.

## Enabling

To enable route-templates you need to register the `\TypeRocketPro\Services\TemplateRouter` service in your `app.services` configuration file.

```php
'services' => [
    /*
     * TypeRocket Service Providers...
     */
    '\TypeRocket\Services\ErrorService',
    '\TypeRocket\Services\MailerService',
    '\TypeRocketPro\Services\TemplateRouter', // add the template router
    
    ...
],
```

Once you have registered the service, you need to set a static page for your "Homepage" and "Posts page" under "Admin > Settings > Reading Settings > 
Your homepage displays". If these settings are not used you will be unable to use the `index` slug later.

## Basic Example

To get started, register your first route-template to replace the default `index.php` template. Note, your theme still needs an `index.php` file to be valid; but it will not get used by WordPress.

To replace the `index.php` template with a route-template add the following to your `routes/public.php` file.

```php
<?php
/*
|--------------------------------------------------------------------------
| Routes
|--------------------------------------------------------------------------
*/
tr_route_template()->on('index', function() {
    return 'Your new site index template';
});
```

`tr_route_template()->on()` takes two arguments:

1. `$slug` - The template name to replace without the `.php` extension.
2. `$handler` - The controller or callback you want to handle the request.

! **Note**: The slug `index` only works for a plugin install with one theme.

### Variations

Also, like normal routes you can use a controller.

```php
tr_route_template()->on('index', 'index@SiteController');
```

Or, send back a view.

```php
tr_route_template()->on('index', function() {
    return tr_view('site.index');
});
```

## Request Methods

Route-templates require you to use the `on()` method. The `on()` sets your default handler. However, you can also define alternative handlers for other request methods.

```php
tr_route_template()->on('single', 'post@SiteController')
  ->post('postCreate@SiteController')
  ->put('postUpdate@SiteController')
  ->delete('postDelete@SiteController');
```

! **Note**: Many nginx servers do but support `PUT` and `DELETE` requests for the `/` path. So, you may not be able to use `put` and `delete` on the `index` template route-template.

## Middleware

To define a global middleware list for all request methods on a route-template use `middleware()`.

```php
tr_route_template()
    ->on('single-post', 'post@SiteController')
    ->post('postCreate@PostTypeController')
    ->middleware([\TypeRocket\Http\Middleware\CheckSpamHoneypot::class]);
```