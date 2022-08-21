Title: Errors
Description: Whoops for WordPress in TypeRocket.

---

## Whoops

!!

For more expressive error handling in WordPress development TypeRocket uses [Whoops](https://github.com/filp/whoops). Whoops is enabled when your `app.debug` is set to `true` - by default `WP_DEBUG` is connected to this config setting.

You can disable Whoops in your config by setting `app.errors.whoops` to `false`. Or, you can add `wps_disable` to a request URL to disable `Whoops` for this single request.

```
https://tut.flights.x/wp-admin/?wps_disable=1
https://tut.flights.x/blog/?wps_disable=1
https://tut.flights.x/?wps_disable=1
```

## PHP Errors

If WordPress is giving you error issues you may need to modify the [error reporting levels](https://www.php.net/manual/en/function.error-reporting.php) set by WordPress or TypeRocket.

### WordPress & PHP

You can set the error reporting level in WordPress using the init hook.
```php
add_action('init', function() {
    error_reporting( E_ERROR | E_PARSE );
});
```

### TypeRocket

You can set the error level TypeRocket uses in your config `app.errors.level` by default it is the value `E_ERROR | E_PARSE`.