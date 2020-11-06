Title: Upgrade Guide v1 to v5
Description: Upgrading TypeRocket v1 to v5

---

## Upgrading From v1

**TypeRocket Pro v5** is not fully backward compatible with **TypeRocket v4**. You will need to make a number of updates to get TypeRocket v4 to work. Because Typerocket v5 is such a big update, it is best to create a new TypeRocket v5 project and then migrate your existing v4 sites into TypeRocket v5. If you are coming from TypeRocket Pro v1 upgrading should take no more than an hour depending on your project size. 

**IMPORTANT NOTICE**: This is not a comprehensive upgrade guide and is still being updated as we look to officially release Typerocket v5.

## Key Changes

There several critical updates that you need to be aware of when making the update from v1 to v5:

- New component system for the builder and matrix fields.
- New Config and Container System
- New Folder Structure
- The `tr` helper functions have been moved to the project root.
- All `tr_` hooks are now prefixed with `typerocket_`.
- Some classes are now exclusive to Pro: Http and Table.
- Some fields are now exclusive to Pro: Gallery, Location, and Editor.
- Some extensions are now exclusive to Pro: SEO Meta, Hide Post Menu, Gutenberg Remover, Disable Comments, and Theme Options.
- New Pro classes Log, Mail, and Storage.
- New Design.
- New Tabs and Tables APIs.
- Removal of all plugins in favor of extensions system.
- New templating system.
- New CLI system.
- New Core system.
- Upgrade to Webpack.
- And more...

## Removed Helper Functions

All non-prefixed helper functions have been removed like `dd`, `dots_walk`, `class_names`, and `str_contains`. Also, number of helper functions have been removed:

```php
tr_table(); // This is now Pro version only
```

### tr_form()

The `tr_form()` function now load the `App/Elements/Form` class instead of `Typerocket/Elements/Form`.

## Pro Extension Publishing

Previously in Typerocket Pro v1 all features were included. With TypeRocket v5 you will need to also require `typerocket/professional` and then publish the professional package after installing it with `composer`.

## Builder & Matrix Component

The three directories pattern has been removed in favor of using classes and views. The directories were: `resources/visuals`, `resources/components`, and `wordpress/assets/components`. All of these have changed.

We removed: `resources/visuals` and `resources/components`. Also, all thumbnails should now live under `wordpress/assets/components`.

Components now live under the `app/Components` folder and are registered to the `config/components.php` file. To migrate your components use the galaxy command to create your new component classes and register them.

```
php galaxy make:component
``` 

Dynamic component thumbnails have been updated to no longer required the hook `tr_builder_component_thumbnails`. You can now control the thumbnail through the component class.

## Icons Removed

The TypeRocket icons `\TypeRocket\Elements\Icons` have been removed in favor of [the WordPress built-in options](https://developer.wordpress.org/resource/dashicons/). To upgrade your icons replace your icon names with the WordPress Dashicons.