Title: Rapid Pages
Description: Create static page caches to point your web server at for improved performance.

---

*Pro Only: This is a Pro only extension feature.*

## Rapid Pages

TypeRocket's Rapid Pages extension allows you to take advantage of the `advanced-cache.php` WordPress drop-in. To enable Rapid Pages:
 
1. Add the extension `\TypeRocketPro\Extensions\RapidPages` to your `app.extesnions` config setting.
2. Run the galaxy command `php galaxy extension:publish typerocket/professional rapid-pages`.
3. Add `define( 'WP_CACHE', true );` to your `wp-config.php` file.
4. Add `define('TYPEROCKET_RAPID_PAGES_FOLDER_PATH', ABSPATH . 'tr_cache/rapid_cache');` to your `wp-config.php` file after `ABSPATH` is defined.

## Unpublish

To unpublish Rapid Pages use the command:

```
php galaxy extension:publish typerocket/professional rapid-pages --mode=unpublish
```

## Nginx Configuration

Once you have enabled the `TypeRocketPro\Extension\RapidPages` you can also configure your web server to the cached files for even faster performance. With Nginx, you can use the `map` directive with custom variables to show the cache with `try_files` to guest users only.   

```
map $http_cookie $cachefiles {
    default "/tr_cache/rapid_cache/$new_request_uri.html";
    "~*wordpress_logged_in" "";
}

server {
    ....
    
    set $new_request_uri $request_uri;
    
    if ($request_uri ~ ^\/(.*)\/$) {
        set $new_request_uri $1;
    }
    
    if ($request_uri ~ ^\/$) {
        set $new_request_uri index;
    }

    .....
    
    location / {
        try_files $cachefiles $uri $uri/ /index.php?$query_string;
    }

    ....
}
```