Title: Rapid Pages
Description: Create static page caches to point your web server at for improved performance.

---

## Nginx Configuration

Once you have enabled the `TypeRocketPro\Extension\RapidPages` you will need to configure your web server to the cached files. With Nginx, you can use the `map` directive with custom variables to show the cache with `try_files` to guest users only.   

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