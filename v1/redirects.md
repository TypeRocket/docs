Title: Redirects
Description: Redirect

---

The `Redirect` object has several methods for making a redirect. For example, you can redirect to a URL on the home page right away using the following code.

```php
tr_redirect()->toURL(home_url('/sample-page/'))->now();
```

## Make Redirect

```php
$redirect = new \TypeRocket\Http\Redirect();
```

Or,

```php
$redirect = tr_redirect();
```

## To URL

To redirect to a URL, use the `toURL()` method.

```php
$redirect->toUrl( home_url('/blog') );
```

## To Home

To redirect to a URL using the home URL, the `toHome()` method.

```php
$redirect->toHome( '/blog' );
```

## To Site

To redirect to a URL using the site URL, the `toSite()` method.

```php
$redirect->toSite( 'blog' );
```

## To Admin

To redirect to a URL using the admin URL, the `toAdmin()` method.

```php
$redirect->toAdmin('themes.php', ['page' => 'theme_options']);
```

## To URL

To redirect to a URL the `toUrl()` method.

```php
$redirect->toUrl( 'https://example.com' );
```

## To Route

To redirect to a named `Route` with the `toRoute()` method.

```php
$redirect->toRoute('users.show', ['id', '1']);
```

## To Page

To redirect to an admin page created by TypeRocket as a `Page` use the `toPage()` method.

```php
$resource = 'Seat';
$action = 'edit';
$item_id = 1;

$redirect->toPage($resource, $action, $item_id);
```

## Back

```php
$redirect->back();
```

## With Fields

To redirect with fields, use the `withOldFields()` method. This is great for when [validation](https://typerocket.com/docs/v1/validator/) fails, and you need to [send the field data back to a form](https://typerocket.com/docs/v1/forms/#section-use-old).  

```php
$redirect->withOldFields( ['field_name' => 'The field value'] );
```

To block fields from being returned, use the second argument.

```php
$redirect->withOldFields( ['field_name' => 'The field value'], ['field_name'] );
```

Get the fields.

```php
// Using this might keep you Form class
// from accessing old field data. 
tr_old_fields();
```

## With Message

```php
$redirect->withMessage('You did it!', 'success');
```

Get the message.

```php
tr_redirect_message();
```

## With Data

```php
$redirect->withData(['my_data' => 'value']);
```

Get the data.

```php
tr_redirect_data();
```

## With Errors

```php
$redirect->withErrors(['my_error' => 'value']);
```

Get the data.

```php
tr_redirect_errors();
```

## With Flash

Redirect with WordPresss admin flash message. Works with the TypeRocket REST API.  Types include: `success`, `error`, and `warning`.

```php
$type = 'success';
$redirect->withFlash('Message', $type);
```

## Redirect Now

To redirect right away, use the `now()` method.

```php
$redirect->now();
```