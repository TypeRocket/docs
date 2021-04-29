Title: Custom Composer Packages
Description: Make your own TypeRocket composer packages. 

---

## About TypeRocket Extension

Extensions, differ from WordPress plugins in that they are installed via composer only and do not have an interface on the plugins admin page in WordPress. Extensions are designed for composer root installations of TypeRocket only.

If you are using the TypeRocket Pro as a WordPress plugin you should [make WordPress plugins](/docs/v5/plugins-making) instead of extensions.

## Composer Packages

At their heart Typerocket extensions are [composer packages](https://packagist.org/). Any composer package can be used in a Typerocket project, but a TypeRocket enabled package can include publish/unpublish script. TypeRocket comes with a Galaxy CLI command to help you publish your extension (this is like activating a plugin in WordPress). 

To make your package publishable, add a `ext/publish.php` file to the root of your composer package. When you publish your package, the `ext/publish.php` will get executed.

## Publishing

To publish your composer package, run the following command but replace `my-vendor/my-package` with your composer vendor and package names.

```bash
php galaxy extension:publish my-vendor/my-package
```

## Unpublish

To unpublish a package to use the `--mode=unpublish` option.

```bash
php galaxy extension:publish my-vendor/my-package --mode=unpublish
```

## Multiple Publishers

You can have multiple package publishers if you like. Each publisher should be located under your packages `ext` folder with the main `ext/publish.php` script. For example, you can have a `ext/feature-2.php` script. To run that script use the command:

```bash
php galaxy extension:publish my-vendor/my-package feature-2
```

## Publish Scripts

You can anything you want into a publisher script. Here is an example:

```php
<?php
/** @var $this \TypeRocket\Console\Command */
$content = WP_CONTENT_DIR;

if($this->getOption('mode', 'publish') == 'publish') {
    $this->success('Publishing Package');
    $this->into($content);
} else {
    $this->warning('Package Unpublished.');
}
```

