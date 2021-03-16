Title: Requests
Description: Requests are objects that represent the HTTP requests.

---

The `Request` object has several methods for working with the HTTP request.

*Note: WordPress automatically adds slashes to $_POST, $_GET, and other super globals. A Request object reflects unmodified super globals.*

## Make Request

```php
$request = new \TypeRocket\Http\Request();

// Or...

$request = tr_request();
```

## Get Fields

When TypeRocket fields are submitted a `Request` can access them using the `getFields()` method.

To get all the fields to call `getFields()` without an argument.

```php
$fields = $request->getFields();
```

To get a specific field call `getFields()` with the name of the field as the argument.

```php
$field = $request->getFields('field_name');
```

You can also do the same using the shorthand method:

```php
$fields =  $request->fields();
$field = $request->fields('field_name');
```

## Get Path

Get the request path.

```php
$path = $request->getPath();
```

## Get URI

Get the URI.

```php
$uri = $request->getUri();
```

## Get Full URI

```php
$request->getUriFull();
```

### Modified Full URI

Get the URL and modify the request query params.

```php
$request->getModifiedUri(['page' => 1]);
```

## Get Host

Get the Host.

```php
$host = $request->gethost();
```

## Get Form Method

Because forms can not submit anything other than `GET` and `POST` requests TypeRocket spoofs `DELETE` and `PUT`. To get the spoofed method use the `getFormMethod()` method.

```php
$method = $request->getFormMethod();
```

## Get Referer

```php
$request->getReferer();
```

## Get Current User

```php
$request->getCurrentUser();
```

## Is Method

```php
$request->isGet();
$request->isPost();
$request->isPut();
$request->isDelete();
```

## Is AJAX

```php
$request->isAjax();
```

Or, you can see if the request is a TypeRocket AJAX request.

```php
$request->isMarkedAjax();
```

## Get Header

```php
$request->getHeader('ACCEPT');
```

## Get Accepts

```php
$rquest->getAccepts();
// Returns and array
```

## Accept Contains & Wants 

If you need to search the content type, a request accepts in return.

```php
$rquest->acceptContains('application/json');
```

Or, you can use the shorthand `wants()`. This method accepts the following options:

- json
- html
- xml
- plain
- any
- image

```php
$rquest->wants('json');
```

## GET & POST Data

```php
$all = $rquest->getDataPost();
$single = $rquest->getDataPost('page');

$all = $rquest->getDataGet();
$single = $rquest->getDataGet('page');
```

## Get Input

If you are working with a request that returns a JSON response body use the `input()` or `getInput()` method.

```php
// returns an array of decoded JSON
$body = $request->input();
```

If no JSON body is found the `$_POST` data will get returned instead:

```php
// When no JSON is sent then 
// $_POST is retrieved 
$post = $request->input();
```

If no post data is found any `$_GET` data will be returned instead.

```php
// Only $_GET is set
$get = $request->input();
```

## GET JSON

The `getDataJson()`  works the same way as `input()` but does not look for `$_GET` data.

```php
$body = $request->getDataJson();
```

## Get Files

```php
$rquest->getDataFiles();
```

## Get Cookies

```php
$all = $request->getDataCookies();
$single = $rquest->getDataCookies('my_cookie');
```

## Check Nonce

The `checkNonce()` method is used by the `\App\Http\Middleware\VerifyNonce` middleware used by TypeRocket for all Kernel requests using the middleware class. It returns true if the check passes.

```php
$request->checkNonce();
```

### Axios

If you are using [axios](https://github.com/axios/axios) you can set a common header for the WordPress nonce TypeRocket uses.

```javascript
axios.defaults.headers.common['X-WP-NONCE'] = window.trHelpers.nonce;
```