Title: CSRF
Description: CSRF protection.

---

## Getting Started

You can add CSRF protection to your site's forms by using the middleware `\App\Http\Middleware\VerifyNonce`. You will need to register the middleware to your `\App\Http\Kernel`. By default, it is apply to any request using the `http` group (controllers called by internal WordPress hooks have their own CSRF protections).

```php
class Kernel extends HttpKernel
{
    protected $middleware = [
        'hooks' =>
            [],
        'http' =>
            [ \App\Http\Middleware\VerifyNonce::class ],
        ...
    ];
}
```

TypeRocket forms automatically come with the needed CSRF tokens needed for verification when you `echo` the `open()` method or the form itself.

```php
$form = tr_from([]);
echo $form->open();
echo $form->text('Name');
echo $form->close('Save');
```

If you have a custom HTML for you can use the `` function to add the CSRF token to your site.

```html
<form>
<?php echo tr_field_nonce(); ?>
</form>
```

## axios

If you are using [axios](https://github.com/axios/axios) you can add a CSRF token header to all requests it sends.

```html
<script>
axios.defaults.headers.common['X-WP-NONCE'] = '<?php echo tr_nonce(); ?>';
</script>
```

## Excluding Routes

You can exclude specific routes from the CSRF check if you like by adding those routes to your `\App\Http\Middleware\VerifyNonce` middleware `$except` property. You can also use the wildcard `*` to match any value for a specific path part. 

```php
class VerifyNonce extends BaseVerify  {

    public $except = [
        'api/my-path',
        'api/public/*'
    ];

}
```