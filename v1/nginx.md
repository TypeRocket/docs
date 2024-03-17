Title: Nginx
Description: Nginx help and debugging

---

## Path Info

PHP-FPM uses the FastCGI protocol to communicate between itself & [Nginx](https://nginx.org/en/). There are certain FCGI parameters that must be implemented to ensure consistent PHP behavior. These parameters are then passed into PHP’s $_SERVER variable where it is then used by the PHP application. Configuration of this is handled via “FastCGI Params” which are discussed in the Nginx documentation: https://www.nginx.com/resources/wiki/start/topics/examples/phpfcgi/
We encountered an issue where TypeRocket’s page builder interface kept throwing 404 AJAX errors in the admin, whenever a page builder module was selected from the dropdown menu. Eventually, the problem was traced back to a missing FCGI parameter:

```
fastcgi_param PATH_INFO $fastcgi_path_info;
```

This omission was resulting in the URL rewrite functionality, which WordPress uses a great deal, to behave inconsistently. Adding the correct entry to the site’s Nginx config in the` location ~ \.php$ {}` block resolved the error. Alternatively, adding this entry to the server’s `fastcgi_params` file (usually located at `etc/nginx/fastcgi_params`), will also fix the problem for all Nginx & PHP-FPM sites on the server. Nginx & PHP-FPM will need to be restarted after this adjustment is made to either the site config or the global fastcgi_params config.