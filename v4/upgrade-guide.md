Title: Upgrade Guide
Description: Upgrade from version 2 of TypeRocket.

---

Version 4 of TypeRocket is not fully backward compatible. You will need to make a number of updates to any version 3 TypeRocket site to get it to work. Because version 4 is such a big update it is best to create a new version 4 project and then migrate your existing 3 sites into version 4.

## Key Changes

There a number of key updates that you need to be aware of when making the update.

- New Routing
- New Config
- New Folder Structure
- Move to Webpack
- Removal of NPM for managing typerocket front-end code. This is now all updated via composer. 

## New Routing

There is now an all-new routing system that uses a less friendly syntax but is much more powerful. The new system uses regex.

```php
tr_route()->get()->match('post/(.*)/edit', ['id'])->do(function($id) {
    return $id;
});
``` 

When a GET HTTP request is sent to `/post/123/edit` it sends a response of `123`.

## Middleware

You can also add middleware at the route level but only works for routes using controllers.

```php
tr_route()->middleware([App\Http\Middleware\VerifyNonce::class])
        ->get()
        ->match('post/(.*)/edit', ['id'])
        ->do('index@Post');
```

**Note**: We removed the `AuthRead` requirement for custom routes because this was causing some confusion. Be sure you secure your custom routes as needed with middleware.

## New Config System

You can now store your own and access your configuration information in the config folder. This functions much like the config system in Laravel. You can access your configuration settings using the `Config::locate()` method.

```php
TypeRocket\Core\Config::locate('app.seed')
```

## Gutenberg

The design of fields and elements has been updated to fit the new design style of Gutenberg.

## CSS

All the CSS of the framework has been updated to use modern tools like flexbox.

## Webpack

We have moved to webpack for building assets when updating core and the core assets for Typerocket are in the same repo as the core PHP files instead of a separate NPM package.