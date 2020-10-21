Title: Responses
Description: The response object has several methods for working with the HTTP response.

---

The `Response` object has several methods for working with the HTTP response.

## Make Response

```php
$response = new \TypeRocket\Http\Response();
```

## Set Message

To set the JSON API response message use the `setMessage()` method.

```php
$response->setMessage('Message');
```

## Flash Next

To flash a message on the next request in the admin use the `flashNext()` method. This is useful when a controller or page redirects.

```php
$response->flashNext('Message');
```

## Flash Now

To flash a message on the current request in the admin use the `flashNow()` method.

```php
$response->flashNow('Message');
```

## Exit Any

To exit right away on any request, JSON API or normal request, use the `exitAny()` method.

```php
$response->exitAny( 404 );
```

## Exit JSON

To exit right away during a JSON API request use the `exitAny()` method.

```php
$response->exitJson( 500 );
```

## Exit Message

To exit right away during a normal request use the `exitMessage()` method.

```php
$response->exitMessage( 500 );
```

## Exit Not Found

To exit with `404`.

```php
$response->exitNotFound();
```

## With Fields

To respond with fields use the `withFields()` method.

```php
$response->withFields( ['field_name' => 'The field value'] );
```