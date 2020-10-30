Title: Static Page Generator
Description: Create static page caches to point your web server at for improved performance.

---

## Nginx Configuration

Once you have enabled the `TypeRocketPro\Extension\StaticPageGenerator` you will need to configure your web server to the cached files. With Nginx, you can use the `map` directive with custom variables to show the cache with `try_files` to guest users only.   

```
map $http_cookie $cachefiles {
    default "/tr_cache/$new_request_uri.html";
    "~*wordpress_logged_in" "";
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name example.com www.example.com;
    server_tokens off;
    root /home/http/example.com/wordpress;
    
    set $new_request_uri $request_uri;
    
    if ($request_uri ~ ^\/(.*)\/$) {
        set $new_request_uri $1;
    }
    
    if ($request_uri ~ ^\/$) {
        set $new_request_uri index;
    }

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    index index.html index.htm index.php;

    charset utf-8;
    
    location / {
        try_files $cachefiles $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    access_log off;
    error_log  /var/log/nginx/example.com-error.log error;

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```