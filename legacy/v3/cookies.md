Title: Cookies
Description: HTTP Cookies can be used to save information locally to a client.

---

## Set a Cookie

To set a cookie use the `tr_cookie()` helper method.

```php
tr_cookie()->set('cookie_name', 'my cookie data');
```

By default, a cookie will be stored for one minute. To change the cookie expiration, add a third argument with the time in seconds.

```php
$hour = HOUR_IN_SECONDS;
tr_cookie()->set('cookie_name', 'my cookie data', $hour);
```

WordPress comes with these options:

- `MINUTE_IN_SECONDS`
- `HOUR_IN_SECONDS`
- `DAY_IN_SECONDS`
- `WEEK_IN_SECONDS`
- `MONTH_IN_SECONDS`
- `YEAR_IN_SECONDS`

*Note: Cookies must be set before any headers are sent. This is before anything is printed to the screen.*

## Get a Cookie

To get a cookie use the `tr_cookie()` helper method.

```php
echo tr_cookie()->get('cookie_name');
```

Will output,

```
my cookie data
```

## Delete a Cookie

```php
tr_cookie()->delete('cookie_name');
```

*Note: Cookies must be deleted before any headers are sent. This is before anything is printed to the screen.*

## Transient Cookies

Private data that is not safe in a cookie can be hard to move around if a page redirects. For example, maybe you want to pass a username or SSN after creating a database record but the page redirects. [WordPress transients](https://codex.wordpress.org/Transients_API) are the answer to this problem and store data in the database instead of the browser. However, a WordPress transient has no relationship to a to a client or browser.

TypeRocket transient cookies allow you to create a WordPress transient in combination with a cookie to pass private, and non-private, information on a per-client basis.

### Set a Transient Cookie

```php
$data = ['key' => 'value'];
tr_cookie()->setTransient('redirect_data', $data);
```

By default, a cookie will be stored for one minute. In most cases, you will not want to store a transient cookie for more than one minute. However, you can change the expiration. To change the cookie expiration, add a third argument with the time in seconds.

```php
$hour = HOUR_IN_SECONDS;
tr_cookie()->set('cookie_name', 'my cookie data', $hour);
```

*Note: Transient cookies must be set before any headers are sent. This is before anything is printed to the screen.*

### Get and Delete a Transient Cookie

When you get the cookie transient it will be deleted if the HTTP headers have not been sent yet. You can also completely disable deleting the transient by passing `false` as the second argument. If you pass `true` the transient will be deleted.

```php
$delete = true;
tr_cookie()->setTransient('redirect_data', $delete);
```

*Note: Transient cookies must be deleted before any headers are sent. This is before anything is printed to the screen.*