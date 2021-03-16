Title: Configuration
Description: Specifics for configuring TypeRocket and its features.

---

## Configuration file

You can use customize TypeRocket by editing the configuration files in the `config` folder.

## Disable and Enable Plugins

To enable and disable the plugins edit the `plugins` setting.

```php
'plugins' => [
  'seo',
  'dev',
  'theme-options',
  'builder'
]
```

## Debug mode

In the `app.php` file set `debug` to `true`. Debug mode adds hints to fields and other areas to aid in development.

```php
'debug' => true
```

## Icons

You can override the Icons that come out of the box by setting `icons` to the full class name of the `Icons` class you want to use.

```php
'icons' => \TypeRocket\Elements\Icons::class
```

## TypeRocket Assets

In the `paths.php` file you can set your assets URL. The URLs paths should be the only paths you edit.

```php
'urls' => [
  'assets' => get_template_directory_uri() . '/typerocket/wordpress/assets',
  'components' => get_template_directory_uri() . '/typerocket/wordpress/assets/components',
],
```

### Setting to Root

For example, when TypeRocket is the root of your project instead of WordPress you can set the URLs to another location.

```php
'urls' => [
  'assets' => home_url() . '/assets',
  'components' => home_url() . '/assets/components',
]
```

*Note: If you change this setting you will also want to make sure your `gulpfile.js` is updated to the new location as well.*

## MySQL 5.7 Notes

In some cases with the most recent version of MySQL 5.7 WordPress will have issues with the `sql_mode`. Depending on your server you will need to disable `STRICT_TRANS_TABLES`. Locate your `my.cnf` file used by MySQL and use the following to disable the newer, better, strict mode:

```
[mysqld]
sql_mode=NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
```

After restarting the MySQL server check your new mode settings by running the SQL query.

```sql
SHOW VARIABLES LIKE 'sql_mode'
```

If you are using Laravel Forge there is a [good article on the topic of the MySQL mode setting](https://mattstauffer.co/blog/how-to-disable-mysql-strict-mode-on-laravel-forge-ubuntu).