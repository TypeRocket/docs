Title: Upgrade Guide v4 to v1 Pro
Description: Upgrading TypeRocket v4 to v1 Pro

---

**TypeRocket Pro v1** is not fully backward compatible with **TypeRocket v4**. You will need to make a number of updates to get TypeRocket v4 to work. Because Typerocket Pro v1 is such a big update, it is best to create a new TypeRocket Pro v1 project and then migrate your existing v4 sites into TypeRocket Pro v1.

**IMPORTANT NOTICE**: This is not a comprehensive upgrade guide and is still being updated.

## Key Changes

There several critical updates that you need to be aware of when making the update:

- New Routing, Middleware, and MVC
- New Tabs, Table, and Pages API
- New Config and Container System
- New Folder Structure
- New `tr` helper functions
- New JSON APIs
- New Fields and Forms system
- New Design
- New Tabs and Tables APIs
- Removal of all plugins in favor of extensions system
- New templating system
- New CLI system
- New Core system
- Upgrade to Webpack
- And more...

## New Helper Function Names

A number of helper function names have been updated:

```php
// Was ...
tr_posts_field('my_field');
tr_users_field('my_field');
tr_comments_field('my_field');
tr_options_field('my_field');
tr_taxonomies_field('my_field');

// Now ...
tr_post_field('my_field');
tr_user_field('my_field');
tr_comment_field('my_field');
tr_option_field('my_field');
tr_term_field('my_field');
```

```php
// Was ...
tr_tables();

// Now ...
tr_table();
```

### tr_form()

The `tr_form()` function signature and methods have changed significantly.

Method updates:

- `useJson()` is now use `useRest()`.

#### Form Rows & Columns

Form rows and columns now have a new syntax. You can compare the versions using these links:

- Pro [https://l.rb.typerocket.test/docs/v1/forms/#section-rows-columns](https://l.rb.typerocket.test/docs/v1/forms/#section-rows-columns)
- v4 [https://l.rb.typerocket.test/docs/v4/forms/#section-field-rows](https://l.rb.typerocket.test/docs/v4/forms/#section-field-rows)

## New Routing

There is now an all-new routing system that is much more powerful. The old system only allowed for anonymous functions and quick declarations; further, any capture groups were required to be named. For example, when a GET HTTP request is sent to `/post/123/edit` it sends a response of `123`:

```php
tr_route()->get()->match('post/(\d+)/edit', ['id'])->do(function($id) {
    return $id;
});
``` 

In v1 Pro you can now use the following:

```php
// Routes ...
tr_route()->get()->match('post/(\d+)/edit')->do(['MyPlugin\\Controllers\\MyCustomController', 'edit']);

// Controller class ...  
namespace MyPlugin\Controllers;

class MyCustomController {
    public function edit($id) {
        return $id
    }
}
```

## New Config System

The config system has been updated:

```php
// Was ...
TypeRocket\Core\Config::locate('app.seed')

// Now ...
tr_config('app.seed');
```

## Custom Resources

Custom resources are also vastly different.

- Pro [https://l.rb.typerocket.test/docs/v1/custom-resources/](https://l.rb.typerocket.test/docs/v1/custom-resources/)
- v4 [https://l.rb.typerocket.test/docs/v4/custom-resources/](https://l.rb.typerocket.test/docs/v4/custom-resources/)

## CSS

All the CSS of the framework has been updated to use modern tools like flexbox and CSS variables.

## Webpack

We have moved to a newer version of webpack for building assets for root installed sites.