Title: Redirects
Description: Redirect 

---

The `Redirect` object has several methods for making a redirect. For example, you can redirect to a URL on the home page right way using the following code.

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

To redirect to a URL use the `toURL()` method.

```php
$redirect->toUrl( home_url('/blog') );
```

## With Fields

To redirect with fields use the `withFields()` method. This is a great for when [validation](https://v3.l.rb.typerocket.test/docs/v3/validator/) fails and you need to [send the field data back to a form](https://v3.l.rb.typerocket.test/docs/v3/forms/#section-use-old).  

```php
$redirect->withFields( ['field_name' => 'The field value'] );
```

To block fields from being returned use the second argument.

```php
$redirect->withFields( ['field_name' => 'The field value'], ['field_name'] );
```

## To Page

To redirect to an admin page created by TypeRocket as a `Page` use the `toPage()` method.

```php
$resource = 'Seat';
$action = 'edit';
$item_id = 1;

$redirect->toPage($resource, $action, $item_id);
```

## Redirect Now

To redirect right away use the `now()` method.

```php
$redirect->now();
```