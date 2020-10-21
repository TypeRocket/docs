Title: Requests
Description: Requests are objects that represent the HTTP requests.

---

The `Request` object has several methods for working with the HTTP request.

*Note: WordPress automatically adds slashes to $_POST, $_GET and other super globals. A Request object reflects unmodified super globals.*

## Make Request

```php
$request = new \TypeRocket\Http\Request();
```

## Get Fields

When TypeRocket fields are submitted a `Request` can access them using the `getFields()` method.

To get all the fields call `getFields()` without an argument.

```php
$fields = $request->getFields();
```

To get a specific field call `getFields()` with the name of the field as the argument.

```php
$field = $request->getFields('field_name');
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