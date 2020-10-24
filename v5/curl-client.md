Title: cURL Client
Description: A simple cURL client.

---

## Getting Started

*Pro Only: This is a Pro only extension feature.*

**IMPORTANT**: You must have the PHP cURL extension installed to use this class.

TypeRocket provides a simple [cURL]([https://www.php.net/manual/en/book.curl.php](https://www.php.net/manual/en/book.curl.php)) Http client. Take a look at sending a POST request.

```php
$url = 'https://exmaple.com';  
$data = [  
  'app' => 'TypeRocket',  
  'downlaods' => '10000',  
];  
$json = true; // Make request as JSON

$http = \TypeRocket\Utility\Http::post($url, $data, $json);
$returned = $http->exec();
```

### All Shorthand Methods

```php
\TypeRocket\Utility\Http::get($url);
\TypeRocket\Utility\Http::post($url, $data, $json);
\TypeRocket\Utility\Http::put($url, $data, $json);
\TypeRocket\Utility\Http::delete($url, $data, $json);
```

## Custom Instance

```php
$method = 'GET'; // Any valid HTTP method can be used
$http = new \TypeRocket\Utility\Http($url, $method);
```

## Headers

You can add headers:

```php
$http->headers([
    'Content-Type: application/json',
	'X-Custom: value'
]);
```

## Data

You can override the data sent:

```php
$json = true; // Make request as JSON
$http->data(['my' => 'data'], $json);
```

## Auth

Add basic HTTP authentication.

```php
$http->auth($username, $password);
```

## URL

A URL will always be present. But you can set a new one.

```php
$http->url('https://example.com');
```

## Get Curl Resource

Get, or override, the reference to the curl resource.

```php
$http->curl();
```

Inject an existing curl resource into the class and override the existing one.

```php
$curl = curl_init();
$url = 'https://example.com';
curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
curl_setopt($curl, CURLOPT_URL, $url);

// Close the existing curl request
$http = (new \TypeRocket\Utility\Http($url))->curl(null);

// Override
$http->curl($curl);
```

## Execute

Execute the curl request and get the returned value.

```php
$json = true; // Make request as JSON
$returned = $http->exec()
```

The returned value is a `CurlResponse` object;

