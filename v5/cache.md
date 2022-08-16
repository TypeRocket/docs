Title: Persistent File Cache
Description: File based persistent caching for WordPress using TypeRocket.

---

## Getting Started

TypeRocket provides a file-based caching system to help improve the speed and performance of your WordPress website.

### Example

For example, maybe your site searches Google Fonts for available fonts by the font's name. However, each unique search requires you to connect to the Google Fonts API slowly. Therefore, it would be best to cache the result of a single API request to avoid making multiple API connections. Take a look at the following code:

```php
$fonts = \TypeRocket\Utility\PersistentCache::new()->getOtherwisePut('google.fonts', function() {
    $api_key = typerocket_env('GOOGLE_FONTS_API_KEY');
    $url = "https://www.googleapis.com/webfonts/v1/webfonts?key={$api_key}";
    $body = wp_remote_retrieve_body( wp_remote_get($url) );
    return $body;
}, MINUTE_IN_SECONDS * 3);
```

Here we use the `\TypeRocket\Utility\PersistentCache` utility to connect to the Google Fonts API and store the result of the request for 3 minutes. The cached result is stored under the path set in your `paths.cache` configuration location. Once the three minutes have expired, the cached result is updated. 

## Put

To put a value into the file cache use the `put()` method and provide a key to store it. You can use the key to access the cached value at a later time.

```php
$key = 'my-unique-key';
$value = 'Some value to store';
$expired_after_seconds = MINUTE_IN_SECONDS * 3;
\TypeRocket\Utility\PersistentCache::new()->put($key, $value, $expired_after_seconds);
```

## Get

To get data from the file cache by key use the `get()` method. You must access the cached value using the key used to put it into the cache. The result will be the unexpired value or the default value.

```php
$key = 'my-unique-key';
$default = 'Some default value';
\TypeRocket\Utility\PersistentCache::new()->get($key, $default);
```

## Remove

To remove a value from the file cache use the `remove()` method. You must remove the cache by the key used to put it into the cache.

```php
$key = 'my-unique-key';
\TypeRocket\Utility\PersistentCache::new()->remove($key);
```

## Has Expired

To check if a cached value is expired use the `hasExpired()` method with the key used to put it into the cache. The result will be a `bool` value.

```php
$key = 'my-unique-key';
\TypeRocket\Utility\PersistentCache::new()->hasExpired($key);
```

## Get Seconds Remaining

To get the seconds remaining for a cached value use the `getSecondsRemaining()` method with the key used to put it into the cache. The result will be a `int` value.

```php
$key = 'my-unique-key';
\TypeRocket\Utility\PersistentCache::new()->getSecondsRemaining($key);
```

## Get Otherwise Put

To get data from the file cache by a key; but otherwise, put a computed value into the cache if the cache is expired or missing use the `getOtherwisePut()`.

```php
$key = 'my-unique-key';
$expired_after_seconds = MINUTE_IN_SECONDS * 3;

$fonts = \TypeRocket\Utility\PersistentCache::new()->getOtherwisePut($key, function() {
    return wp_generate_uuid4();
}, $expired_after_seconds);
```
