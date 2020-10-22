Title: Custom Composer Packages
Description: Make your own TypeRocket composer packages. 

---

## Composer Packages

You can create your custom composer packages that are compatible with TypeRocket. When you do, TypeRocket comes with a Galaxy CLI command to help you publish your extension (this is like activating a plugin in WordPress).

If you are using the TypeRocket Pro WordPress plugin, you should make your packages WordPress plugins. This feature is designed for composer installations.

To make your package publishable, add a `publish.php` file to the root of your composer package. When you publish your package, the `publish.php` will get executed.

## Publishing

To publish your composer package, run the following command but replace `my-vendor/my-package` with your composer vendor and package names.

```php
php galaxy extension:publish my-vendor/my-package
```

### Working with Files

When publishing your package, you may want to work with files. You can use the `tr_file()` helper to copy and update files.

