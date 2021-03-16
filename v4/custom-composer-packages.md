Title: Custom Composer Packages
Description: Make your own TypeRocket composer packages. 

---

## TypeRocket Compatable Composer Packages

You can create your own custom composer pages that are compatible with TypeRocket. When you do TypeRocket comes with a Galaxy CLI command to help you publish your plugin (this is like activating a plugin in WordPress).

To make your package publishable add a `publish.php` file to the root of your composer package. When you publish your package the `publish.php` will get executed.

## Publishing

To publish your composer package run the following command but replace `my-vendor/my-package` with your composer vendor and package names.

```php
php galaxy plugin:publish my-vendor/my-package
```

### Working with Files

When publishing your package you may want to work with files. You can use the `tr_file()` helper to copy and update files.

