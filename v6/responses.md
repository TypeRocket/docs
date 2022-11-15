Title: Responses
Description: The response object has several methods for working with the HTTP response.

---

## Getting Started

The TypeRocket response system can be used to manage the server's response to client-side requests. The response system should be used within the context of a controller or middleware for the best results.

If you use the response system outside of a controller, keep in mind some methods like `abort()` will need to be handled by your own try/catch statements.

Also, when dealing with responses, you may want to set a status code. [https://httpstatuses.com/](https://httpstatuses.com/) is an excellent resource for the different status code numbers.

## Response Instance

The `Response` object has several methods for working with the HTTP response.

You can use the `tr_response()` method to access the application's primary response instance.

```php
$response = tr_response();
```

### Make Response

In some rare cases, you may want to create your response instance.

```php
$response = new \TypeRocket\Http\Response();
```

## Set Message

To set the REST API response message use the `setMessage()` method. The message is also used if the response object is the return value of a controller.

```php
$response->setMessage('Hello world!');
```

The response will look like this is the REST API is used, or you return the response as the value from a controller.

```json
{
"message": "Hello world!",
"messageType": "success",
"redirect": false,
"status": null,
"flash": true,
"blockFlash": false,
"errors": [],
"data": []
}
```

You can also set the message type at the same time. There are three valid message types: `success`, `error`, and `warning`.

```php
$response->setMessage('Message', 'success');
$response->setMessage('Message', 'error');
$response->setMessage('Message', 'warning');
```

The request types will set the color of the alert in the WordPress admin is the REST API is used.

Also, there are three shorthand methods for setting the message type. These shortcuts also allow you to optionally set the status code.

### Success

```php
$response->success('Message', 201);
```

### Error

```php
$response->error('Message', 422);
```

### Warning

```php
$response->warning('Message', 200);
```

### Set Message Type

You can also set the message type manually. When setting the message type this way, the error code will be set to match message type if it has not been set already; thus, setting the type to `error` will default the status code to `422`. 

```php
$response->setMessageType('error');
```

### Getters

You can get the message type and message as follows.

```php
$message = $response->getMessage();
$type = $response->getMessageType();
```

## Flash Next

To flash a message on the next request in the admin, use the `flashNext()` method. This is useful when a controller or page redirects.

```php
$response->flashNext('Message');
```

## Flash Now

To flash a message on the current request in the admin, use the `flashNow()` method.

```php
$response->flashNow('Message');
```

## Set Flash

Set if the flash message should be shown on the front end.

```php
$response->setFlash(true);
```

## Lock Flash

Stop flash from being overridden by future sets.

```php
$response->lockFlash();
```

You can also unlock the flash if it is locked.

```php
$response->unlockFlash();
```

## Block Flash

Force the flash to not be shown.

```php
$response->blockFlash();
```

## Exit Any

To exit right away on any request, REST API or normal request, use the `exitAny()` method.

```php
$response->exitAny( 404 );
```

## Exit JSON

To exit right away during a JSON API request, use the `exitAny()` method.

```php
$response->exitJson( 500 );
```

## Exit Message

To exit right away during a normal request, use the `exitMessage()` method.

```php
$response->exitMessage( 500 );
```

## Exit Not Found

To exit with `404`.

```php
$response->exitNotFound();
```

## Exit Server Error

To exit with a server error.

```php
$response->exitServerError();
```
## Abort

Unlike the `exit` methods of the response class, the `abort()` method throws an exception. When the exception is caught by the kernel, a response is automatically generated.

The abort response can be either a WordPress template `404` or any other error code template. Or, the response will send a JSON string is the requester wants JSON.

```php
$response->abort(500);
```

If the error code sent is not a valid HTTP status code and error screen will be displayed instead.

Using abort allows you to provide WordPress templates beyond `404.php`. This means you can have a `500.php` error code in your theme as well if an abort is used.

## With Old Fields

To respond with fields, use the `withOldFields()` method.

```php
$response->withOldFields( ['field_name' => 'The field value'] );
```

## Errors

```php
$response->setErrors(['my_key' => 'My value.']);
$response->setError('my_key', 'My value.');
$response->hasErrors();
$response->getErrors();
```

## Get Return

This returns a reference to the return value from your controller.

```php
$response->getReturn();
```

## Set Status

To set the status code pass any value HTTP status code into the `setStatus()` method.

```php
$response->setStatus(404);
```

There are also quick status setters for specific statuses: `bad`, `unauthorized`, `forbidden`. Also, these methods set the response message making it easy to provide a JSON response. 

### Bad

Sets the status code to `400`.

```php
$response->bad($message);
```

### Unauthorized

Sets the status code to `401`.

```php
$response->unauthorized($message);
```

### Forbidden

Sets the status code to `403`.

```php
$response->forbidden($message);
```

## Set Header

Set any header.

```php
$response->setHeader('Content-type', 'application/json');
```

## Set Content-Type Header

```php
$response->send('json');
// Sets: Content-type: application/json
```

Shorthand options include:

- json
- json-ld
- xml
- html
- t-xml
- plain

## Sends

```php
$bool = $response->sends('json');
```

Shorthand options include:

- json
- json-ld
- xml
- html
- t-xml
- plain
- image

## Get Headers

```php
$response->getHeaders();
```

## Set TypeRocket Redirect

This method only applies to specific TypeRocket AJAX REST API requests.

```php
$response->setRedirect( home_url() );
```

Also, you can use the `canRedirect()` method to set the redirect URL of the response JSON body to the URL of a redirect object. This happens if that redirect object is the return value of a controller. For example: 

```php
tr_response()->canRedirect();
return tr_redirect()->toUrl(home_url());  
```

## With Redirect Message

Send a request with a redirect message transient.

```php
$response->withRedirectMessage();
```

You can then access the message using `tr_redirect_message()`.

## With Redirect Errors

Send a request with redirect errors transient.

```php
$response->withRedirectErrors();
```

You can then access the errors using `tr_redirect_errors()`.

## With Redirect Data

Send a request with a redirect data transient.

```php
$response->withRedirectData();
```

You can then access the data using `tr_redirect_data()`.


## Disable Page Cache

```php
$response->disablePageCache();
```

## Cancel Response

When the response would be used, cancel the response when it finishes.

```php
$response->setCancel(true);
```

Or, undo the cancel.

```php
$response->setCancel(false);
```